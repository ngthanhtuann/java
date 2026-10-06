# FamilyConnect

**Nền tảng Cộng đồng Gia đình số tích hợp Trí tuệ nhân tạo** (AI-powered Digital Family Community Platform).
Đồ án môn học: quản lý gia phả kết hợp dịch vụ cộng đồng gia đình (bài đăng, sự kiện, di sản, danh bạ) và trợ lý AI.

## Công nghệ
| Thành phần | Công nghệ |
|---|---|
| Backend | Java 17, Spring Boot 3 (Web, Security, Data JPA, Validation), Flyway, springdoc-openapi |
| Cơ sở dữ liệu | PostgreSQL 16 + pgvector |
| Frontend web | React / Next.js (task SCRUM-33) |
| Mobile | React Native + Expo (task SCRUM-85) |
| AI | Spring AI + LLM API (các task sau) |
| Triển khai | Docker Compose |

## Cấu trúc thư mục
```
backend/    Spring Boot (API, nghiệp vụ, kết nối cơ sở dữ liệu)
frontend/   Next.js (sẽ thêm)
mobile/     Expo (sẽ thêm)
docker/     Cấu hình Docker bổ sung
docs/       Toàn bộ tài liệu (SRS, thiết kế, API, kiểm thử, quy trình)
.github/    CI và mẫu Pull Request
docker-compose.yml
```

## Chạy hệ thống

### Yêu cầu cài đặt
- **Docker Desktop** (để chạy cơ sở dữ liệu và backend)
- **JDK 17** và **Maven** (chỉ cần nếu chạy backend bằng IDE hoặc chạy test ở máy)
- Git

### Cách 1: chạy toàn bộ bằng Docker (một lệnh)
```bash
docker compose up --build
```
Chờ vài phút lần đầu (tải image và build). Kiểm tra:

| Việc | Cách |
|---|---|
| Backend sống | mở http://localhost:8080/actuator/health, phải thấy `{"status":"UP"}` |
| Swagger UI | mở http://localhost:8080/swagger-ui.html |

Trên Windows PowerShell, dùng `curl.exe http://localhost:8080/actuator/health` (không phải `curl`) hoặc mở bằng trình duyệt.

Tắt hệ thống: `docker compose down` (thêm `-v` để xóa cả dữ liệu cơ sở dữ liệu).

### Cách 2: chạy backend bằng IDE (khi đang lập trình)
```bash
docker compose up -d db          # chỉ chạy cơ sở dữ liệu
cd backend
mvn spring-boot:run              # hoặc chạy FamilyConnectApplication trong IntelliJ/VS Code
```

### Chạy test
```bash
docker compose up -d db
cd backend
mvn verify
```
Test cần cơ sở dữ liệu đang chạy vì kiểm tra cả Flyway migration.

### Biến môi trường
Sao chép `.env.example` thành `.env` nếu muốn đổi mật khẩu cơ sở dữ liệu. **Không commit file `.env`.**

## Quy ước làm việc (bắt buộc đọc)
Toàn bộ quy trình Jira và GitHub, đặt tên nhánh, commit, Pull Request: xem
**[docs/00-process/quy-trinh-lam-viec.md](docs/00-process/quy-trinh-lam-viec.md)**.

Tóm tắt:
- Mỗi task một nhánh riêng: `feature/SCRUM-xx-ten` (code), `docs/SCRUM-xx-ten` (tài liệu), `bugfix/SCRUM-xx-ten`.
- Commit có mã task: `SCRUM-xx: nội dung`.
- Mở Pull Request vào `develop`, chọn reviewer, có 1 Approve mới merge. Không push thẳng vào `main` hoặc `develop`.

## CI
Mỗi Pull Request và mỗi lần push vào `develop`, `main` chạy GitHub Actions (`.github/workflows/ci.yml`):
build Maven và chạy test với PostgreSQL (pgvector) làm service.

## Tài liệu
Xem [docs/README.md](docs/README.md) để biết ai viết file nào và các quy ước tài liệu.
