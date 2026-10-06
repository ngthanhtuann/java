# SRS: Đặc tả yêu cầu phần mềm FamilyConnect

> **Người viết:** TV1 (Tuấn Nguyễn Thanh) | **Reviewer:** TV4 (Huy Quốc) | **Task Jira:** SCRUM-14 | **Hạn nộp review:** Thứ Năm 8/10
> **Trạng thái:** Nháp 
> Cách làm: tạo nhánh `docs/<mã SCRUM>-<tên-ngắn>` từ `develop`, điền vào file này, mở Pull Request vào `develop`. Xem `docs/README.md`.

**Phiên bản:** 0.1 (nháp) | **Ngày:** ...

---
## 1. Giới thiệu
### 1.1 Mục đích
> Viết 2-3 câu: tài liệu này dùng để làm gì, ai đọc.
### 1.2 Phạm vi sản phẩm
> FamilyConnect là gì, làm gì, không làm gì (tóm tắt).
### 1.3 Thuật ngữ và viết tắt
| Thuật ngữ | Giải thích |
|---|---|
| FR / NFR | Yêu cầu chức năng / phi chức năng |
| RBAC | Role-Based Access Control |
| RAG | Retrieval-Augmented Generation |
| | |
### 1.4 Tài liệu tham chiếu
- Đề cương đề tài (mục 3.2 a-g).

## 2. Mô tả tổng quan
### 2.1 Bối cảnh bài toán
> Tóm tắt mục a) Context của đề tài.
### 2.2 Phân tích các bên liên quan (stakeholder analysis)
| Bên liên quan | Vai trò | Mối quan tâm / kỳ vọng | Mức ảnh hưởng (Cao/TB/Thấp) |
|---|---|---|---|
| Giảng viên | Người đánh giá | Đáp ứng đúng đề tài, tài liệu đầy đủ | Cao |
| Thành viên gia đình | Người dùng chính | | |
| Trưởng chi / Quản lý gia phả | | | |
| Quản trị hệ thống | | | |
| Nhóm phát triển | | | |
### 2.3 Tác nhân (Actor)
| Actor | Mô tả | Quyền chính |
|---|---|---|
| Khách | Chưa đăng nhập | Đăng ký, đăng nhập |
| Thành viên | Đã được xác minh | |
| Trưởng chi / Quản lý gia phả | | |
| Quản trị hệ thống | | |
| AI Service | Hệ thống bên ngoài (LLM) | |
### 2.4 Môi trường và ràng buộc công nghệ
- Backend: **Java Spring Boot** (bắt buộc). Frontend: **React / Next.js**. **Web app bắt buộc**.
- Mobile: React Native + Expo (đa nền tảng). Database: PostgreSQL + pgvector. AI: Spring AI + LLM API.
- Triển khai: Docker Compose.
### 2.5 Giả định và phụ thuộc
> Ví dụ: có kết nối Internet để gọi LLM; dữ liệu demo do nhóm tạo.

## 3. Yêu cầu chức năng (mã FR)
> Đây là bảng quan trọng nhất của Sprint 1. **Gửi cả nhóm trước trưa Thứ Ba** để mọi người gắn mã FR vào use case.
> Mọi chức năng trong mục c) của đề tài đều phải có mã. Cột Ưu tiên: Must / Should / Could. Cột "Mức độ" ghi rõ nếu chỉ làm bản cơ bản.

| Mã | Yêu cầu (tiếng Việt) | Gốc trong đề tài | Ưu tiên | Mức độ thực hiện | Ghi chú |
|---|---|---|---|---|---|
| FR-AUTH-01 | Đăng ký và xác thực người dùng | User registration and authentication | Must | | |
| FR-AUTH-02 | Phân quyền theo vai trò (RBAC) | Role-Based Access Control | Must | | |
| FR-AUTH-03 | Quản lý hồ sơ người dùng | User profile management | Must | | |
| FR-AUTH-04 | Xác minh thành viên gia đình | Family member verification | Must | | |
| FR-GEN-01 | Quản lý gia đình | Family management | Must | | |
| FR-GEN-02 | Quản lý chi họ | Branch management | Must | | |
| FR-GEN-03 | Quản lý thành viên | (mục "management" của đề tài, ghi thiếu chữ; hiểu là Member management) | Must | | |
| FR-GEN-04 | Quản lý quan hệ cha-mẹ-con | Parent-child relationship management | Must | | |
| FR-GEN-05 | Quản lý hôn nhân | Marriage management | Must | | |
| FR-GEN-06 | Cây gia phả tương tác | Interactive genealogy tree | Must | | |
| FR-GEN-07 | Hiển thị trực quan quan hệ | Relationship visualization | Must | | |
| FR-GEN-08 | Truy vấn quan hệ họ hàng | Relationship query | Must | | |
| FR-COM-01 | Tạo và quản lý bài đăng | Create and manage posts | Must | | |
| FR-COM-02 | Bình luận và reaction | Comment and react to posts | Must | | |
| FR-COM-03 | Chia sẻ tin tức gia đình | Share family news | Must | | |
| FR-COM-04 | Chia sẻ ảnh | Photo sharing | Must | | |
| FR-COM-05 | Thông báo của gia đình | Family announcements | Must | | |
| FR-EVT-01 | Tạo sự kiện gia đình | Create family events | Must | | |
| FR-EVT-02 | Quản lý RSVP | RSVP management | Must | | |
| FR-EVT-03 | Quản lý người tham dự | Participant management | Must | | |
| FR-EVT-04 | Thư viện ảnh sự kiện | Event gallery | Must | | |
| FR-EVT-05 | Nhắc nhở sự kiện | Event reminders | Must | | |
| FR-DIR-01 | Danh bạ thành viên | (mục "directory" của đề tài, ghi thiếu chữ; hiểu là Member directory) | Must | | |
| FR-DIR-02 | Hồ sơ nghề nghiệp | Professional profiles | Must | | |
| FR-DIR-03 | Hồ sơ học vấn | Education profiles | Must | | |
| FR-DIR-04 | Tìm thành viên theo nghề, nơi ở, thế hệ | Search members by profession, location, or generation | Must | | |
| FR-HER-01 | Tài liệu lịch sử | Historical documents | Must | | |
| FR-HER-02 | Câu chuyện gia đình | Family stories | Must | | |
| FR-HER-03 | Người tiêu biểu của gia đình | Outstanding family members | Must | | |
| FR-HER-04 | Thư viện ảnh | Photo gallery | Must | | |
| FR-HER-05 | Kho lưu trữ số | Digital archives | Must | | |
| FR-AI-01 | Tìm kiếm ngữ nghĩa bằng AI | AI-powered semantic search | Must | | |
| FR-AI-02 | Trợ lý AI hỏi đáp tri thức gia đình | AI family knowledge assistant | Must | | |
| FR-AI-03 | AI giải thích quan hệ | AI relationship explanation | Must | | |
| FR-AI-04 | AI tóm tắt nội dung | AI content summarization | Must | | |
| FR-AI-05 | AI gợi ý thành viên và tài nguyên liên quan | AI recommendation of related members and family resources | Must | | |
| FR-DSH-01 | Thống kê gia đình | Family statistics | Must | | |
| FR-DSH-02 | Dashboard hoạt động cộng đồng | Community activity dashboard | Must | | |
| FR-DSH-03 | Thống kê sự kiện | Event statistics | Must | | |
| FR-DSH-04 | Thống kê nhân khẩu | (mục "demographics" của đề tài; hiểu là Family demographics) | Must | | |
| FR-DSH-05 | Tạo báo cáo | Report generation | Must | | |
| FR-ADM-01 | Quản lý người dùng | User management | Must | | |
| FR-ADM-02 | Kiểm duyệt nội dung | Content moderation | Must | | |
| FR-ADM-03 | Nhật ký kiểm toán (audit log) | Audit logging | Must | | |
| FR-ADM-04 | Sao lưu và khôi phục | Backup & Restore | Must | | |
| FR-ADM-05 | Cấu hình hệ thống | System configuration | Must | | |

## 4. Use case
Chi tiết từng module nằm trong thư mục `use-cases/`:
- [Auth, RBAC, Profile, Admin](use-cases/auth.md)
- [Gia phả](use-cases/genealogy.md)
- [Community và Events](use-cases/community-events.md)
- [Heritage, Directory, Dashboard](use-cases/heritage-directory-dashboard.md)
- [AI](use-cases/ai.md)

> Chèn use case diagram tổng (Mermaid hoặc ảnh draw.io) tại đây.

## 5. Yêu cầu phi chức năng
| Mã | Yêu cầu | Gốc trong đề tài | Tiêu chí đo | Ghi chú |
|---|---|---|---|---|
| NFR-01 | Ứng dụng web responsive | Responsive Web Application | | |
| NFR-02 | Ứng dụng di động đa nền tảng | Cross-platform Mobile Application | | |
| NFR-03 | Xác thực an toàn bằng JWT | Secure Authentication (JWT) | | |
| NFR-04 | Kiến trúc RESTful API | RESTful API Architecture | | |
| NFR-05 | Kiến trúc phần mềm dạng module | Modular Software Architecture | | |
| NFR-06 | Trực quan hóa đồ thị tương tác | Interactive Graph Visualization | | |
| NFR-07 | Cơ sở dữ liệu PostgreSQL | PostgreSQL Database | | |
| NFR-08 | Tích hợp dịch vụ AI | AI Service Integration | | |
| NFR-09 | Triển khai bằng Docker | Docker Deployment | | |
| NFR-10 | Tính sẵn sàng cao (mức cơ bản) | High Availability | | |
| NFR-11 | Ghi nhật ký kiểm toán | Audit Logging | | |
| NFR-12 | Hiệu năng: API cây gia phả < 1 giây với 5.000 người | (bổ sung) | | |
| NFR-13 | Bảo mật: mật khẩu băm BCrypt, chống SQL injection/XSS/IDOR | (bổ sung) | | |

## 6. Quyền riêng tư dữ liệu
Xem [privacy.md](privacy.md).

## 7. Phạm vi: làm gì và không làm gì
### 7.1 Trong phạm vi
> Liệt kê theo nhóm chức năng, ghi rõ mức độ (đầy đủ / cơ bản).
### 7.2 Ngoài phạm vi
> Chỉ ghi những thứ đề tài **không yêu cầu** (ví dụ thanh toán, mạng xã hội công khai, ứng dụng riêng cho từng hệ điều hành).
> Không đưa vào đây những mục đề tài đã yêu cầu (Mobile, Backup & Restore, System configuration...).

## 8. Ma trận truy vết
| Mã FR | Use case | Bảng DB | API | Màn hình | Test case | Task Jira |
|---|---|---|---|---|---|---|
| FR-AUTH-01 | UC-AUTH-01 | user | POST /auth/register | Đăng ký | | SCRUM-35 |
