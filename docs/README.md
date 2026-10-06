# Tài liệu dự án FamilyConnect

Nơi lưu toàn bộ tài liệu của đồ án **FamilyConnect: Nền tảng Cộng đồng Gia đình số tích hợp AI**.
Mọi tài liệu viết bằng Markdown và thay đổi **chỉ qua Pull Request** vào nhánh `develop`.

## Mục lục và người phụ trách

| Thư mục / file | Nội dung | Người viết | Reviewer | Task Jira |
|---|---|---|---|---|
| `01-srs/srs.md` | SRS: giới thiệu, bên liên quan, yêu cầu chức năng (mã FR), phi chức năng | TV1 Tuấn | TV4 | SCRUM-14 |
| `01-srs/privacy.md` | Bảo mật và quyền riêng tư dữ liệu | TV1 Tuấn | TV4 | SCRUM-17 |
| `01-srs/use-cases/auth.md` | Use case Auth, RBAC, Profile, Admin | TV1 Tuấn | TV4 | SCRUM-26 |
| `01-srs/use-cases/genealogy.md` | Use case Gia phả | TV2 Trí | TV5 | SCRUM-18 |
| `01-srs/use-cases/community-events.md` | Use case Community và Events | TV3 PiLo257 | TV1 | SCRUM-20 |
| `01-srs/use-cases/heritage-directory-dashboard.md` | Use case Heritage, Directory, Dashboard | TV4 Huy Quốc | TV2 | SCRUM-22 |
| `01-srs/use-cases/ai.md` | Use case AI | TV5 Lê Nhựt | TV3 | SCRUM-34 |
| `02-design/architecture.md` | Kiến trúc tổng thể (Web, Mobile, AI Service Layer) | TV4 Huy Quốc | TV2 | SCRUM-23 |
| `02-design/algorithm-relationship.md` | Thuật toán quan hệ họ hàng | TV2 Trí | TV5 | SCRUM-19 |
| `02-design/ai-rag.md` | Thiết kế AI/RAG | TV5 Lê Nhựt | TV3 | SCRUM-34 |
| `02-design/business-process.md` | Mô hình hóa quy trình nghiệp vụ | TV3 PiLo257 | TV1 | SCRUM-83 |
| `02-design/mobile.md` | Thiết kế Mobile (React Native + Expo) | TV5 Lê Nhựt | TV3 | SCRUM-84 |
| `03-database/erd.md`, `data-dictionary.md` | ERD tổng và từ điển dữ liệu | TV2 Trí | TV5 | SCRUM-28 |
| `04-api/conventions.md` | Quy ước API chung | TV3 PiLo257 | TV1 | SCRUM-21 |
| `04-api/openapi/*.yaml` | Đặc tả OpenAPI từng module | chủ module | reviewer module | SCRUM-26, 18, 20, 22, 34 |
| `05-ui/` | Design system, link Figma | TV5 + từng người | | SCRUM-25, 29, 30, 32, 84 |
| `06-test/test-plan.md` | Kế hoạch kiểm thử | TV1 Tuấn | TV4 | SCRUM-27 |
| `decisions/` | Ghi quyết định kỹ thuật (ADR) | người quyết định | | SCRUM-24, 31, 84 |
| `07-deploy/`, `08-user-guide/`, `09-report/` | Triển khai, hướng dẫn sử dụng, báo cáo cuối | Sprint 7-8 | | |

## Quy ước chung
- File viết bằng **Markdown**, tên file **chữ thường, không dấu, nối bằng gạch ngang**.
- **Mỗi người chỉ sửa file của mình.** Chỉ Tuấn sửa `srs.md` và file này; người khác gửi nội dung cho Tuấn.
- Sơ đồ: dùng **Mermaid** trong Markdown (GitHub tự vẽ) hoặc vẽ bằng draw.io rồi xuất ảnh PNG vào `images/` và chèn vào bằng `![mô tả](../images/ten-anh.png)`.
- Wireframe vẽ trong Figma, chỉ lưu link và ảnh xuất ra trong `05-ui/`.
- Mã yêu cầu dùng thống nhất: `FR-<NHÓM>-<số>` (chức năng), `NFR-<số>` (phi chức năng), `UC-<NHÓM>-<số>` (use case).
- Mọi thay đổi sau **Docs Gate** (cuối Sprint 2) phải cập nhật lại tài liệu gốc (ERD, OpenAPI, SRS) trước khi sửa code.

## Quy trình viết tài liệu (task nào cũng làm như vậy)
1. Nhận task trên Jira, kéo sang **In Progress**.
2. Tạo nhánh từ `develop`: `git checkout -b docs/SCRUM-18-genealogy`.
3. Điền vào file của mình, commit có mã task: `SCRUM-18: use case module Gia phả`.
4. Push và mở **Pull Request** vào `develop`, chọn đúng **Reviewer**, dán link PR vào task Jira, kéo task sang **In Review**.
5. Reviewer comment từng dòng, **Approve** hoặc **Request changes**. Người viết sửa và push thêm.
6. Có Approve thì **Squash and merge**, xóa nhánh; reviewer kéo task sang **Done**.

## Khi nào một task được coi là Done
- Có tiêu chí hoàn thành trong mô tả task trên Jira và đã tự kiểm tra theo đó.
- Mọi thay đổi (code hoặc tài liệu) đã vào `develop` qua Pull Request, có ít nhất 1 Approve từ reviewer.
- Task code: có test, không còn secret trong code, API mới đã có trong Swagger.
- Task tài liệu: đủ các mục theo khung sườn, đúng mã FR và quy ước API.
- Link Pull Request đã dán vào task Jira; reviewer là người kéo task sang Done.

## Hạn của Sprint 1 (Thứ Sáu 9/10, 23:30)
- Thứ Tư 7/10: kiến trúc (SCRUM-23), quy ước API chốt (SCRUM-21).
- Thứ Năm 8/10: tất cả tài liệu nộp review.
- Thứ Sáu 9/10: reviewer comment, sửa, merge, họp Review + Retro. Sáng Thứ Bảy báo cáo.
