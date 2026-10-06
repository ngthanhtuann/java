# ERD tổng hợp

> **Người viết:** TV2 (Nguyễn Minh Trí) | **Reviewer:** TV5 (Lê Nhựt) | **Task Jira:** SCRUM-28 | **Hạn nộp review:** Thứ Ba 13/10 (Sprint 2)
> **Trạng thái:** Nháp 
> Cách làm: tạo nhánh `docs/<mã SCRUM>-<tên-ngắn>` từ `develop`, điền vào file này, mở Pull Request vào `develop`. Xem `docs/README.md`.

## 1. Quy ước đặt tên
- Tên bảng `snake_case`, số ít (`person`, `parent_child`).
- Khóa chính `id` (UUID hoặc BIGINT, thống nhất một kiểu).
- Cột `created_at`, `updated_at`, xóa mềm `deleted_at`.
- Thay đổi schema chỉ qua **Flyway migration**, báo cả nhóm.

## 2. ERD
> Dùng Mermaid `erDiagram` hoặc ảnh từ dbdiagram.io / draw.io (lưu ảnh trong `../images/`).

```mermaid
erDiagram
  FAMILY ||--o{ BRANCH : has
  FAMILY ||--o{ PERSON : has
  PERSON ||--o{ PARENT_CHILD : "is parent"
  PERSON ||--o{ MARRIAGE : "in"
```

## 3. Danh sách bảng theo module
| Bảng | Module | Người sở hữu | Trạng thái |
|---|---|---|---|
| user, role, user_role, refresh_token, member_verification, audit_log, system_config | Auth/Admin | TV1 | |
| family, branch, person, parent_child, marriage | Gia phả | TV2 | |
| post, comment, reaction, media, announcement, notification, event, event_participant | Community/Events | TV3 | |
| heritage_document, family_story, outstanding_member, album, digital_archive, profession_profile, education_profile | Heritage/Directory | TV4 | |
| embedding_chunk, chat_session, chat_message, ai_summary_cache | AI | TV5 | |

## 4. Kiểm tra xung đột
- [ ] Không còn khóa ngoại treo
- [ ] Mỗi bảng có người sở hữu
- [ ] Đã họp 30 phút xác nhận ERD v1 (đóng băng)
