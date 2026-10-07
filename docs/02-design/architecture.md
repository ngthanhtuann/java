# Kiến trúc tổng thể
> **Người viết:** TV4 (Huy Quốc) | **Reviewer:** TV2 (Nguyễn Minh Trí) | **Task Jira:** SCRUM-23 | **Hạn nộp review:** Thứ Tư 7/10
> **Trạng thái:** Chờ review

## 1. Sơ đồ thành phần
> Thể hiện đúng kiến trúc đề tài: **Web Management Portal**, **Mobile Application**, **AI Service Layer** kết nối qua **RESTful API**. Các khối logic trong Spring Boot backend và cách xử lý lỗi tập trung.

```mermaid
flowchart LR
    subgraph Frontend ["Frontend (web/mobile)"]
        Web[Next.js Web]
        Mobile[Mobile App - React Native/Expo]
    end

    subgraph Backend ["Spring Boot Backend (khối nguyên khối)"]
        API["API Gateway (REST Controller)"]
        subgraph CoreModules ["Core Modules"]
            Auth[Auth Module]
            row1_2["Người dùng, Admin<br/>Hồ sơ, vai trò, kiểm duyệt, sao lưu"]
            Gene[Genealogy Module]
            Commu[Community Module]
            Event[Events Module]
            Heri[Heritage Module]
            Direc[Directory Module]
            Dash[Dashboard Module]
        end
        Ai("AI Service (AiGateway interface) <br/> - tìm kiếm ngữ nghĩa <br/> - chatbot <br/> - phân tích/gợi ý")
    end

    subgraph External ["Dịch vụ bên ngoài"]
        LLM[LLM API]
    end
    
    subgraph DatabaseLayer ["Lớp dữ liệu"]
        DB[(PostgreSQL + pgvector)]
        Uploads[("Local/Cloud Storage <br/> (ảnh, tệp tải lên)")]
    end

    Frontend -->|HTTPS REST /api/v1| API
    API -->|gọi service| CoreModules
    API -->|gọi service| Ai
    CoreModules -->|CRUD| DB
    Ai -->|embeddings/vector search| DB
    Ai -->|truy vấn| LLM
    Ai -->|lưu/đọc tệp| Uploads
    
    classDef errorType fill:#f9f,stroke:#333,stroke-width:2px,color:black;
    API -.->|Response: ErrorResponse <br/> (theo conventions.md)| Frontend

    linkStyle 0,1,2,3,4,5,6,7 stroke:#444,stroke-width:1px;
    linkStyle 8 stroke:red,stroke-dasharray: 5 5,stroke-width:2px;
## 2. Quyết định thiết kế lớp AI Service

### 2.1. Quyết định kiến trúc
Lớp AI Service được quyết định tích hợp trực tiếp bên trong kiến trúc nguyên khối (Modular Monolith) của Spring Boot hiện tại, sử dụng thư viện Spring AI.
* **Phân định định tuyến:** Tất cả các API liên quan đến trí tuệ nhân tạo được nhóm thống nhất dưới tiền tố đường dẫn `/api/v1/ai/**`.
* **Phân lập mã nguồn:** Toàn bộ logic AI được đóng gói thành một package độc lập hoàn toàn (gồm `controller`, `service`/`AiGateway`, `repository`, `dto`, `entity`).
* **Giao tiếp:** Khi còn là một khối, các module khác gọi AI qua interface Java `AiGateway`. REST chỉ dùng khi tách thành service riêng (xem mục 2.4).

### 2.2. Lý do lựa chọn (Ưu điểm trong giai đoạn đầu)
* **Tối ưu hóa nguồn lực và triển khai:** Việc duy trì một máy chủ backend nguyên khối giúp đơn giản hóa quy trình CI/CD, tiết kiệm chi phí hạ tầng (không phải thuê nhiều server) và giảm thiểu độ phức tạp trong việc quản lý, giám sát hệ thống mạng nội bộ.
* **Độ trễ thấp (Low Latency) cho luồng RAG:** Đặt AI Service cùng môi trường với Core API cho phép nó truy xuất trực tiếp xuống cơ sở dữ liệu PostgreSQL. Việc trích xuất dữ liệu gia phả làm ngữ cảnh (context) diễn ra tức thời, loại bỏ hoàn toàn độ trễ mạng so với việc gọi chéo giữa các microservices riêng biệt.
* **Tận dụng hệ sinh thái Spring:** Spring AI cung cấp các interface chuẩn hóa, giúp thao tác dễ dàng với LLM bên ngoài và cơ sở dữ liệu vector (pgvector).

### 2.3. Rủi ro tiềm ẩn
* **Cạnh tranh tài nguyên (Resource Contention):** Các tác vụ xử lý của AI (như embedding, tìm kiếm vector, mapping dữ liệu RAG) ngốn rất nhiều CPU và RAM. Nếu lượng truy cập AI tăng đột biến, nó có thể chiếm dụng tài nguyên của hệ thống, làm chậm hoặc gây gián đoạn (timeout) các API nghiệp vụ cốt lõi khác.
* **Điểm nghẽn về mở rộng (Scaling Bottleneck):** Khi tính năng AI bị quá tải, hệ thống bắt buộc phải nhân bản (scale out) toàn bộ ứng dụng Spring Boot cồng kềnh, dẫn đến lãng phí tài nguyên không cần thiết.

### 2.4. Chiến lược phân tách thành dịch vụ độc lập (Microservice Extraction) sau này
Nhờ tuân thủ nguyên lý thiết kế module hóa từ đầu, quá trình chuyển đổi sang Microservices khi dự án mở rộng sẽ diễn ra rất mượt mà:
* **Tách rời mã nguồn (Decoupling):** Bóc tách toàn bộ package AI hiện tại sang một dự án hoàn toàn mới (có thể viết lại bằng Python/FastAPI hoặc bản Spring Boot nhẹ).
* **Định tuyến tại API Gateway (Nginx):** Chỉ cần cập nhật cấu hình của Nginx Load Balancer để `proxy_pass` thẳng sang máy chủ AI Service mới. Web và Mobile hoàn toàn không phải sửa đổi code gọi API.
* **Phân lập cơ sở dữ liệu:** Các bảng có tiền tố `ai_` trong schema public (như `ai_embedding_chunk`, `ai_chat_session`...) có thể chuyển sang schema `ai` riêng bằng một migration khi tách service, và cấp tài khoản DB độc lập chỉ có quyền read-only trên dữ liệu Core.
## 3. Sơ đồ triển khai Docker
graph TD
    Mobile["Mobile App<br/>React Native / Expo"]
    Web["Trình duyệt<br/>Next.js Web"]
    LLM["LLM API bên ngoài<br/>(Internet, HTTPS)"]

    subgraph DockerHost ["Docker Environment (máy chủ triển khai)"]
        Nginx["Container: Nginx<br/>Cổng 80 / 443"]
        subgraph Net ["Docker network: familyconnect-net"]
            NextJS["Container: Next.js Web<br/>cổng nội bộ 3000"]
            Spring1["Container: Spring Boot API #1<br/>cổng nội bộ 8080"]
            Spring2["Container: Spring Boot API #2<br/>cổng nội bộ 8080"]
            DB["Container: PostgreSQL + pgvector<br/>cổng 5432, không mở ra ngoài"]
        end
        Pgdata[("Volume: pgdata")]
        Uploads[("Volume: uploads<br/>ảnh, tệp tải lên")]
    end

    Mobile -->|"HTTPS"| Nginx
    Web -->|"HTTPS"| Nginx
    Nginx -->|"/"| NextJS
    Nginx -->|"/api/v1/*"| Spring1
    Nginx -->|"/api/v1/*"| Spring2
    Spring1 --> DB
    Spring2 --> DB
    Spring1 --> Uploads
    Spring2 --> Uploads
    Spring1 -->|"HTTPS"| LLM
    Spring2 -->|"HTTPS"| LLM
    DB --- Pgdata
Loading
Ghi chú ngay dưới sơ đồ: "Hiện chạy 1 bản backend; đích triển khai ở Sprint 7 (SCRUM-96) là 2 bản."
## 4. Sơ đồ trình tự
### 4.1 Đăng nhập JWT
sequenceDiagram
    participant U as Người dùng
    participant FE as Web/Mobile
    participant BE as Spring Boot Backend
    U->>FE: Nhập email, mật khẩu
    FE->>BE: POST /api/v1/auth/login
    note over BE: Xác thực thông tin người dùng
    alt thông tin không hợp lệ
        BE-->>FE: Response: ErrorResponse (AUTH_001)
    else thông tin hợp lệ
        note over BE: Ký Access Token & Refresh Token
        BE-->>FE: access token + refresh token
    end

### 4.2 Hỏi chatbot AI
sequenceDiagram
    participant U as Người dùng
    participant FE as Web/Mobile
    participant Gateway as API Gateway (Spring Boot)
    participant AiServ as AI Service Module
    participant DB as PostgreSQL + pgvector
    participant LLM as LLM API bên ngoài

    U->>FE: Nhập câu hỏi, gửi
    FE->>Gateway: POST /api/v1/ai/chat (ChatRequest, userId)

    Gateway->>AiServ: ask(ChatRequest, userId)
    
    alt tài khoản chưa xác minh
        Gateway-->>FE: 403 ErrorResponse (PERM_002)
    else vượt giới hạn câu hỏi
        Gateway-->>FE: 429 ErrorResponse (RATE_001)
    else hợp lệ
        Gateway->>AiServ: tạo embedding cho câu hỏi
        AiServ->>DB: tìm kiếm vector top-k (lọc theo family_id)
        DB-->>AiServ: danh sách đoạn văn ngữ cảnh
        AiServ->>AiServ: ghép ngữ cảnh + prompt hệ thống
        AiServ->>+LLM: gửi prompt (HTTPS)
        LLM-->>-AiServ: phản hồi từ LLM
        AiServ->>AiServ: lưu lịch sử hội thoại vào bảng ai_chat_message
        AiServ-->>Gateway: ChatResponse (bao gồm nguồn)
        Gateway-->>FE: 200 OK (ChatResponse)
    end
## 5. Cấu trúc mã nguồn

### Backend (Spring Boot)
```text
src/main/java/com/familyconnect/
├── common/                 # config, security (JWT filter), exception, audit, ErrorResponse
└── modules/
    ├── auth/               # đăng nhập, refresh token
    ├── user/               # hồ sơ, vai trò
    ├── admin/              # quản lý user, kiểm duyệt, audit, sao lưu, cấu hình
    ├── genealogy/          # gia đình, chi họ, thành viên, quan hệ, cây
    ├── community/          # bài đăng, bình luận, ảnh (media), thông báo
    ├── events/             # sự kiện, RSVP, nhắc nhở
    ├── heritage/           # tài liệu, câu chuyện, kho lưu trữ số
    ├── directory/          # danh bạ, hồ sơ nghề nghiệp, học vấn
    ├── dashboard/          # thống kê, báo cáo
    └── ai/                 # tìm kiếm ngữ nghĩa, chatbot, giải thích, tóm tắt, gợi ý
        └── (mỗi module đều có) controller/ service/ repository/ dto/ entity/
### Frontend (Next.js)
src/
├── app/          # Chứa cấu trúc định tuyến (Routing - App Router).
├── components/   # Chứa các UI Component độc lập, tái sử dụng (Header, Sidebar...).
├── lib/          # Chứa các tiện ích và cấu hình dùng chung toàn dự án.
└── services/     # Lớp trung gian gọi API từ Spring Boot. Dùng kiểu dữ liệu sinh từ OpenAPI.
### Mobile (Expo)
src/
├── navigation/   # Chứa các file cấu hình luồng chuyển trang (React Navigation).
├── screens/      # Chứa các file giao diện toàn màn hình (HomeScreen, ChatAiScreen...).
├── components/   # Chứa các thành phần UI dùng chung (Card, ListItem...).
└── services/     # Dùng chung hợp đồng API: kiểu dữ liệu sinh từ file OpenAPI (docs/04-api/openapi/), không viết tay hai nơi.

## 6. Đáp ứng yêu cầu phi chức năng (NFR)
Hệ thống được thiết kế không chỉ để giải quyết các luồng nghiệp vụ mà còn đảm bảo nền tảng kỹ thuật vững chắc thông qua việc tuân thủ 5 yêu cầu phi chức năng cốt lõi sau:

### 6.1. Kiến trúc mô-đun (Modular Architecture)
* **Cách đáp ứng:** Mã nguồn Spring Boot được chia dọc thành 10 gói (package) độc lập theo từng miền nghiệp vụ như đã liệt kê ở Mục 5.
* **Giá trị mang lại:** Giảm thiểu sự phụ thuộc chéo (loose coupling). Khi một tính năng bị lỗi, nó sẽ được khoanh vùng trong mô-đun đó mà không làm sập toàn bộ hệ thống. Đây là bước đệm hoàn hảo để bóc tách thành Microservices.

### 6.2. Chuẩn giao tiếp RESTful
* **Cách đáp ứng:** Mọi giao tiếp giữa lớp Client (Web/Mobile) và Server đều tuân thủ nguyên tắc REST. Lỗi API được trả về đồng nhất theo quy chuẩn `docs/04-api/conventions.md` với định dạng `{success:false, error:{code,message,details}}`.
* **Giá trị mang lại:** Đảm bảo Web Next.js và Mobile App có thể gọi chung 100% một bộ API duy nhất và xử lý lỗi đồng bộ.

### 6.3. Xác thực phi trạng thái (JWT - JSON Web Token)
* **Cách đáp ứng:** Hệ thống không lưu trữ phiên đăng nhập (session) trên RAM. Máy chủ cấp phát JWT (Access/Refresh Token). Client đính kèm token vào header `Authorization: Bearer` (Web lưu cookie httpOnly, Mobile lưu expo-secure-store).
* **Giá trị mang lại:** Giúp tiết kiệm bộ nhớ cho server và cho phép hệ thống dễ dàng mở rộng chiều ngang (Scale-out) phía sau Nginx.

### 6.4. Tính sẵn sàng cao cơ bản (Basic High Availability)
* **Cách đáp ứng:** Toàn bộ hệ thống được đóng gói thành container độc lập. Sử dụng Nginx làm API Gateway kiêm Load Balancer (Mục tiêu Sprint 7 - SCRUM-96 là 2 bản Spring Boot sau Nginx; hiện chạy 1 bản). Thiết lập chính sách `restart: always` và dùng Docker Volume (`pgdata`, `uploads`) để mount dữ liệu ra ngoài.
* **Giá trị mang lại:** Sẵn sàng phân phối lưu lượng tải, tự động phục hồi khi tiến trình sập và đảm bảo an toàn dữ liệu cứng.

### 6.5. Kiểm vết và Lưu vết hệ thống (Audit Logging)
* **Ghi log nghiệp vụ:** Hệ thống tích hợp một `AuditService` hoạt động ngầm để ghi nhận các sự kiện bảo mật quan trọng (ai, làm gì, lúc nào).
* **Truy xuất nguồn gốc:** Cột chuẩn cho các bảng nghiệp vụ: `id`, `created_at`, `updated_at`, `deleted_at`, thêm `created_by`, `updated_by`. (Tuyệt đối không dùng `created_date`).
* **Bảo mật log:** Audit log chỉ ghi sự kiện, tuyệt đối không chứa nội dung prompt thô. Lịch sử hội thoại vẫn được lưu riêng ở bảng `ai_chat_message` theo đúng thời hạn trong `privacy.md`.

## 7. Quyết định kiến trúc đã chốt (bắt buộc áp dụng)

| # | Quyết định | Lý do | Ai phải áp dụng |
|---|---|---|---|
| 1 | Một schema `public`. Bảng AI có tiền tố `ai_`: `ai_embedding_chunk`, `ai_chat_session`, `ai_chat_message`, `ai_summary_cache`. Chỉ module `ai` đọc, ghi các bảng này; module khác không join. Khi tách service: một migration chuyển `ai_*` sang schema `ai` và cấp tài khoản DB riêng | Đơn giản trong 8 tuần, vẫn sẵn sàng tách sau | Trí (ERD), Lê Nhựt (AI) |
| 2 | Web và mobile đều dùng thư mục `services/` để gọi API. Kiểu dữ liệu sinh từ OpenAPI (`docs/04-api/openapi/`), không viết tay hai nơi | Một hợp đồng API duy nhất | Lê Nhựt (mobile), Huy Quốc (web) |
| 3 | Module gọi nhau qua interface Java khi còn một khối. REST nội bộ chỉ dùng khi tách service | Nhanh, dễ test | Tất cả backend |
| 4 | Cột chuẩn: `id`, `created_at`, `updated_at`, `deleted_at`, thêm `created_by`, `updated_by` cho bảng nghiệp vụ. Không dùng `created_date` | Khớp ERD | Trí, tất cả backend |
| 5 | Backend gồm `common/` và `modules/<tên>/{controller,service,repository,dto,entity}`. Tên module cố định: `auth`, `user`, `admin`, `genealogy`, `community`, `events`, `heritage`, `directory`, `dashboard`, `ai`. `SecurityConfig` chuyển vào `common/config` | Một cấu trúc cho mọi người | Tất cả backend, Tuấn (SCRUM-35) |
| 6 | Lỗi trả về theo `docs/04-api/conventions.md` (`success:false, error:{code,message,details}`) | Một chuẩn lỗi cho web và mobile | Tất cả backend |
| 7 | AI: tài khoản chưa xác minh không dùng được (403, `PERM_002`); giới hạn số câu hỏi (429, `RATE_001`); lọc theo `family_id` trước khi tạo ngữ cảnh | Theo `docs/01-srs/privacy.md` | Lê Nhựt |
| 8 | Mục tiêu triển khai: 2 bản Spring Boot sau Nginx (Sprint 7, SCRUM-96); hiện chạy 1 bản | Đáp ứng NFR-10 | Huy Quốc (Docker) |
