1. Sơ đồ thành phần kiến trúc (Component Diagram)
Hệ thống FamilyConnect áp dụng mô hình kiến trúc Client-Server với giao diện đa nền tảng và backend nguyên khối phân rã theo mô-đun (Modular Monolith). Lớp AI Service được tích hợp trực tiếp nhưng hoạt động như một dịch vụ độc lập nội bộ.
graph TD
    Mobile["Mobile App<br/>React Native / Expo"]
    Web["Next.js Web<br/>Cổng quản lý"]

    subgraph SpringBoot ["Spring Boot API"]
        direction TB
        Common["common (dùng chung)<br/>JWT filter, audit log, ProblemDetail"]
        
        row1_1["Gia phả<br/>Quan hệ họ hàng"]
        row1_2["Người dùng<br/>Hồ sơ, vai trò"]
        row1_3["Xác thực<br/>Đăng nhập, refresh"]
        
        row2_1["Dashboard<br/>Thống kê"]
        row2_2["Heritage, Directory<br/>Di sản, danh bạ"]
        row2_3["Community, Events<br/>Cộng đồng, sự kiện"]
        
        AIPackage["Gói ai: /api/v1/ai/**<br/>controller, AiGateway, Spring AI adapter"]

        Common ~~~ row1_2
        row1_1 ~~~ row2_1
        row1_2 ~~~ row2_2
        row1_3 ~~~ row2_3
        row2_2 ~~~ AIPackage
    end

    DB["PostgreSQL + pgvector<br/>schema public, schema ai"]
    LLM["LLM API bên ngoài<br/>Chat, embedding"]

    Mobile -->|"RESTful API"| SpringBoot
    Web -->|"RESTful API"| SpringBoot
    
    SpringBoot -->|"JPA read/write"| DB
    AIPackage -->|"Semantic search"| DB
    AIPackage -->|"Prompt / response"| LLM


2. Quyết định thiết kế Lớp Dịch vụ AI (AI Service Layer)
2.1 Quyết định kiến trúc Lớp AI Service được quyết định tích hợp trực tiếp bên trong kiến trúc nguyên khối (Modular Monolith) của Spring Boot hiện tại, sử dụng thư viện Spring AI.
Phân định định tuyến: Tất cả các API liên quan đến trí tuệ nhân tạo được nhóm thống nhất dưới tiền tố đường dẫn /api/v1/ai/**.
Phân lập mã nguồn: Toàn bộ logic AI được đóng gói thành một package độc lập hoàn toàn (gồm controller, service/AiGateway, adapter riêng).
Giao tiếp: Mặc dù nằm chung một mã nguồn, package AI được thiết kế để hoạt động như một lớp độc lập, các module khác khi cần tương tác sẽ gọi qua các interface chuẩn hoặc giao tiếp qua REST nội bộ nhằm duy trì tính liên kết lỏng (loose coupling).
2.2 Lý do lựa chọn (Ưu điểm trong giai đoạn đầu)
Tối ưu hóa nguồn lực và triển khai: Việc duy trì một máy chủ backend nguyên khối giúp đơn giản hóa quy trình CI/CD, tiết kiệm chi phí hạ tầng (không phải thuê nhiều server) và giảm thiểu độ phức tạp trong việc quản lý, giám sát hệ thống mạng nội bộ.
Độ trễ thấp (Low Latency) cho luồng RAG: Đặt AI Service cùng môi trường với Core API cho phép nó truy xuất trực tiếp xuống cơ sở dữ liệu PostgreSQL. Việc trích xuất dữ liệu gia phả làm ngữ cảnh (context) diễn ra tức thời, loại bỏ hoàn toàn độ trễ mạng (network latency) so với việc gọi chéo giữa các microservices riêng biệt.
Tận dụng hệ sinh thái Spring: Spring AI cung cấp các interface chuẩn hóa, giúp kỹ sư Java thao tác dễ dàng với LLM bên ngoài và cơ sở dữ liệu vector (pgvector) mà không ép buộc đội dự án phải chuyển sang một stack công nghệ hoàn toàn mới (như Python) ngay từ đầu.
2.3 Rủi ro tiềm ẩn
Cạnh tranh tài nguyên (Resource Contention): Các tác vụ xử lý của AI (như embedding, tìm kiếm vector, mapping dữ liệu RAG) ngốn rất nhiều CPU và RAM. Nếu lượng truy cập AI tăng đột biến, nó có thể chiếm dụng tài nguyên của hệ thống, làm chậm hoặc gây gián đoạn (timeout) các API nghiệp vụ cốt lõi khác (đăng nhập, xem danh bạ).
Điểm nghẽn về mở rộng (Scaling Bottleneck): Khi tính năng AI bị quá tải, hệ thống không thể chỉ tăng cường phần cứng cho riêng khối AI, mà bắt buộc phải nhân bản (scale out) toàn bộ ứng dụng Spring Boot cồng kềnh, dẫn đến lãng phí tài nguyên không cần thiết.
Hạn chế về thư viện học máy chuyên sâu: Dù Spring AI đang phát triển mạnh, nhưng Python vẫn là nền tảng thống trị về AI mã nguồn mở. Việc gắn logic vào Java có thể gây bất lợi nếu tương lai dự án muốn tự huấn luyện mô hình (train model) hoặc tùy biến các thuật toán AI phức tạp.
2.4 Chiến lược phân tách thành dịch vụ độc lập (Microservice Extraction) sau này Nhờ tuân thủ nguyên lý thiết kế module hóa từ đầu, quá trình chuyển đổi sang Microservices khi dự án mở rộng sẽ diễn ra rất mượt mà mà không làm gián đoạn hệ thống Client:
Tách rời mã nguồn (Decoupling): Bốc tách toàn bộ package AI hiện tại sang một dự án hoàn toàn mới. Đội dự án có thể cân nhắc viết lại service này bằng Python (FastAPI/Flask) để tối ưu hóa hiệu năng xử lý AI, hoặc tiếp tục dùng một bản Spring Boot siêu nhẹ.
Định tuyến tại API Gateway (Nginx): Chỉ cần cập nhật cấu hình của Nginx Load Balancer. Khi nhận request có tiền tố /api/v1/ai/**, Nginx sẽ tự động proxy_pass thẳng sang máy chủ AI Service mới thay vì máy chủ Core API. Ứng dụng Web và Mobile hoàn toàn không phải sửa đổi bất kỳ dòng code gọi API nào. 
Phân lập cơ sở dữ liệu: Dịch vụ AI mới sẽ được cấp một tài khoản kết nối riêng tới PostgreSQL, được phân quyền chỉ thao tác (Read/Write) trên schema ai (chứa dữ liệu vector) và chỉ có quyền
đọc (Read-only) trên schema public để đảm bảo tính toàn vẹn dữ liệu của các module nghiệp vụ khác.


3. Sơ đồ triển khai hệ thống (Deployment Diagram)
Hệ thống được container hóa hoàn toàn bằng Docker. Sử dụng Nginx làm API Gateway để điều hướng lưu lượng từ thiết bị di động (Mobile) và trình duyệt (Web) gọi chung một nguồn API nội bộ.
graph TD
    Mobile["Mobile App<br/>React Native / Expo"]
    Web["Next.js Web<br/>Trình duyệt"]

    subgraph DockerHost ["Docker Environment (Máy chủ triển khai)"]
        direction TB
        
        Nginx["Container: Nginx Load Balancer<br/>Cổng: 80 / 443"]
        
        subgraph DockerNetwork ["Docker Bridge Network: familyconnect-net"]
            direction TB
            NextJS["Container: Next.js Web<br/>Cổng nội bộ: 3000"]
            Spring["Container: Spring Boot API<br/>Cổng nội bộ: 8080"]
            DB_Container["Container: PostgreSQL + pgvector<br/>Cổng nội bộ: 5432"]
            
            NextJS ~~~ Spring ~~~ DB_Container
        end
        
        Volume[("Docker Volume<br/>pgdata_volume")]
    end

    Mobile -->|"HTTPS (RESTful API)"| Nginx
    Web -->|"HTTPS (RESTful API)"| Nginx
    
    Nginx -->|"Proxy Pass: /"| NextJS
    Nginx -->|"Proxy Pass: /api/v1/*"| Spring
    
    Spring -->|"JDBC / TCP"| DB_Container
    DB_Container ---|"Mount dữ liệu cứng"| Volume


4. Thiết kế luồng xử lý (Sequence Diagrams)
4.1. Luồng xác thực đăng nhập JWT (Stateless Authentication)
sequenceDiagram
    autonumber
    
    actor User as Người dùng
    participant Client as Web / Mobile
    participant Nginx as Nginx
    participant AuthCtrl as AuthController
    participant AuthSvc as AuthService
    participant UserRepo as UserRepository
    participant DB as PostgreSQL
    participant Audit as AuditService

    User->>Client: Nhập email và mật khẩu
    Client->>Nginx: POST /api/v1/auth/login
    Nginx->>AuthCtrl: chuyển tiếp tới một backend
    AuthCtrl->>AuthSvc: login(LoginRequest)
    AuthSvc->>UserRepo: findByEmail
    UserRepo->>DB: SELECT user
    DB-->>UserRepo: user và password hash
    UserRepo-->>AuthSvc: User
    AuthSvc->>AuthSvc: BCrypt so khớp mật khẩu
    
    alt hợp lệ
        AuthSvc->>AuthSvc: tạo access token và refresh token
        AuthSvc->>Audit: ghi LOGIN_SUCCESS
        AuthSvc-->>AuthCtrl: TokenResponse
        AuthCtrl-->>Client: 200 TokenResponse
        
        Note right of Client: Các request sau gửi header Authorization Bearer
        Client->>Client: lưu token
        
    else sai thông tin
        AuthSvc->>Audit: ghi LOGIN_FAILED
        AuthCtrl-->>Client: 401 ProblemDetail
    end


4.2. Luồng hỏi Chatbot AI (Tích hợp RAG Pipeline)
sequenceDiagram
    autonumber
    
    actor User as Người dùng
    participant Client as Web / Mobile
    participant Nginx as Nginx
    participant Filter as JwtAuthFilter
    participant Ctrl as AiController
    participant Gateway as AiGateway
    participant DB as PostgreSQL pgvector
    participant LLM as LLM API
    participant Audit as AuditService

    User->>Client: Nhập câu hỏi
    Client->>Nginx: POST /api/v1/ai/chat kèm Bearer token
    Nginx->>Filter: chuyển tiếp
    Filter->>Filter: kiểm tra chữ ký, hạn dùng, vai trò
    
    alt token sai hoặc hết hạn
        Filter-->>Client: 401 ProblemDetail
    else hợp lệ
        Filter->>Ctrl: request kèm Authentication
        Ctrl->>Gateway: ask(ChatRequest, userId)
        Gateway->>LLM: tạo embedding cho câu hỏi
        LLM-->>Gateway: vector
        Gateway->>DB: tìm top-k gần nhất, lọc theo quyền
        DB-->>Gateway: các đoạn ngữ cảnh
        Gateway->>LLM: prompt gồm chỉ dẫn hệ thống, ngữ cảnh, câu hỏi
        
        alt LLM lỗi hoặc quá hạn
            LLM-->>Gateway: timeout
            Gateway-->>Ctrl: fallback
            Ctrl-->>Client: 503 ProblemDetail
        else thành công
            LLM-->>Gateway: câu trả lời
            Gateway-->>Ctrl: ChatResponse gồm answer và citations
            Gateway->>Audit: ghi AI_QUERY, không lưu prompt thô
            Ctrl-->>Client: 200 ChatResponse
        end
    end
    
    Client-->>User: Hiển thị câu trả lời


5. Cấu trúc mã nguồn & Thư mục dự án
Hệ thống áp dụng kiến trúc phân rã theo chức năng (Feature-based structure) cho backend và cấu trúc phân lớp logic cho frontend/mobile nhằm tối ưu hóa khả năng mở rộng.
Spring Boot Backend (Java)
src/main/java/com/familyconnect/
├── common/                 # Chứa mã dùng chung: config, exceptions, security (JWT filter), audit log
└── modules/                # Chứa các gói nghiệp vụ độc lập
    ├── auth/               # Mô-đun Xác thực
    ├── directory/          # Mô-đun Danh bạ
    └── ai/                 # Mô-đun AI Service
        ├── controller/     # Tiếp nhận các HTTP Request và trả về HTTP Response.
        ├── service/        # Nơi xử lý logic nghiệp vụ chính (gọi Spring AI, RAG, build prompt).
        ├── repository/     # Chứa interface giao tiếp CSDL (PostgreSQL, pgvector).
        ├── dto/            # Các class định nghĩa dữ liệu đầu vào (Request) và đầu ra (Response).
        └── entity/         # Các class ánh xạ trực tiếp thành các bảng trong CSDL.


Next.js Web Frontend
src/
├── app/          # Chứa cấu trúc định tuyến (Routing) theo App Router.
├── components/   # Chứa các UI Component độc lập tái sử dụng (Header, Sidebar, CustomButton).
├── lib/          # Chứa tiện ích (utilities) và cấu hình dùng chung (Axios instance, format date).
└── services/     # Lớp trung gian gọi API từ Spring Boot (auth.service.ts, ai.service.ts).


Mobile App (React Native/Expo)
src/
├── navigation/   # File cấu hình luồng chuyển trang (Stack Navigator, Bottom Tab Navigator).
├── screens/      # Các file giao diện toàn màn hình (HomeScreen, ChatAiScreen).
├── components/   # UI Components dùng chung cho thiết bị di động.
└── services/     # Lớp gọi API HTTP (Đồng bộ logic và DTO hoàn toàn với thư mục 'services' của Web).


6. Đáp ứng các yêu cầu phi chức năng (NFRs)
Hệ thống được thiết kế không chỉ để giải quyết các luồng nghiệp vụ mà còn đảm bảo nền tảng kỹ thuật vững chắc thông qua việc tuân thủ 5 yêu cầu phi chức năng cốt lõi sau:
6.1. Kiến trúc mô-đun (Modular Architecture)
Cách đáp ứng: Mã nguồn Spring Boot không chia theo các lớp ngang truyền thống (gom tất cả controller vào một chỗ) mà được chia dọc thành các gói (package) độc lập theo từng miền nghiệp vụ (Ví dụ: auth, heritage, ai, directory).
Giá trị mang lại: Giảm thiểu sự phụ thuộc chéo (loose coupling). Khi một tính năng bị lỗi, nó sẽ được khoanh vùng trong mô-đun đó mà không làm sập toàn bộ hệ thống. Kiến trúc này cũng là bước đệm hoàn hảo để dễ dàng bóc tách một mô-đun (như AI Service) thành Microservice độc lập trong tương lai.
6.2. Chuẩn giao tiếp RESTful
Cách đáp ứng: Mọi giao tiếp giữa lớp Client (Web/Mobile) và Server đều tuân thủ nghiêm ngặt nguyên tắc REST. Sử dụng định dạng dữ liệu chuẩn JSON, định nghĩa các URL hướng tài nguyên (ví dụ: /api/v1/auth/login, /api/v1/ai/chat) và tuân thủ các phương thức HTTP (GET, POST, PUT, DELETE).
Giá trị mang lại: Đảm bảo Web Next.js và Mobile App có thể gọi chung 100% một bộ API duy nhất. Trả về đúng các mã trạng thái HTTP chuẩn xác để Client dễ dàng xử lý (VD: 200 OK cho thành công, 401 Unauthorized khi sai token, 503 Service Unavailable khi API bên ngoài bị lỗi).
6.3. Xác thực phi trạng thái (JWT - JSON Web Token)
Cách đáp ứng: Hệ thống không lưu trữ phiên đăng nhập (session) trên RAM của máy chủ. Thay vào đó, sau khi xác thực thành công, máy chủ cấp phát một JWT (gồm Access Token và Refresh Token). Client sẽ đính kèm token này vào header Authorization: Bearer trong mọi request tiếp theo.
Giá trị mang lại: Kiến trúc stateless (phi trạng thái) giúp tiết kiệm bộ nhớ cho server. Đồng thời, nó cho phép hệ thống dễ dàng mở rộng chiều ngang (Scale-out) – có thể chạy 2, 3 container Spring Boot cùng lúc phía sau Nginx mà không cần lo lắng về việc đồng bộ session giữa các máy chủ.
6.4. Tính sẵn sàng cao cơ bản (Basic High Availability)
Cách đáp ứng: Toàn bộ hệ thống được đóng gói thành các container độc lập qua Docker.
Sử dụng Nginx làm API Gateway kiêm Load Balancer, sẵn sàng phân phối lưu lượng tải nếu dự án chạy nhiều bản sao backend.
Thiết lập chính sách tự động phục hồi (restart: always hoặc unless-stopped trong Docker) giúp container tự động khởi động lại nếu tiến trình bị treo hoặc sập.
Dữ liệu của PostgreSQL được ánh xạ ra ngoài ổ cứng vật lý (Docker Volume), đảm bảo dữ liệu không bị mất ngay cả khi container cơ sở dữ liệu bị xóa.
6.5. Kiểm vết và Lưu vết hệ thống (Audit Logging)
Cách đáp ứng: Hệ thống tích hợp một AuditService hoạt động ngầm (như đã thiết kế trong sơ đồ trình tự).
Ghi log nghiệp vụ: Ghi nhận tự động các sự kiện bảo mật quan trọng như LOGIN_SUCCESS, LOGIN_FAILED, hay AI_QUERY.
Truy xuất nguồn gốc dữ liệu: Các Entity trong CSDL kế thừa một lớp cơ sở (Auditable) để tự động điền các trường created_by, updated_at, created_date mỗi khi có thao tác thêm/sửa/xóa trên các tài liệu gia phả.
Bảo mật log: Đảm bảo tuân thủ nguyên tắc an toàn thông tin: Tuyệt đối không ghi log mật khẩu thô của người dùng và không lưu trữ các prompt thô (chứa dữ liệu cá nhân nhạy cảm) khi gọi LLM API.
