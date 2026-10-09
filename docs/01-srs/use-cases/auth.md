# Use case: Auth, RBAC, Profile, Admin

> **Người viết:** TV1 (Tuấn Nguyễn Thanh) | **Reviewer:** TV4 (Huy Quốc) | **Task Jira:** SCRUM-26 | **Hạn nộp review:** Thứ Năm 15/10 (Sprint 2)
> **Trạng thái:** Nháp 
> Cách làm: tạo nhánh `docs/<mã SCRUM>-<tên-ngắn>` từ `develop`, điền vào file này, mở Pull Request vào `develop`. Xem `docs/README.md`.

## 1. Phạm vi và Actor
> Module này làm gì, ai dùng. Bổ sung thêm **ma trận phân quyền** (role x chức năng) ở mục 4 và **luồng JWT** (access token, refresh token, nơi FE lưu token).
>
> Ghi chú phạm vi: FR-AUTH-02 (Phân quyền theo vai trò, RBAC) thuộc tài liệu này; đặc tả bổ sung ở Sprint 2.

## 2. Danh sách use case
> Dưới đây là gợi ý ban đầu. Chỉnh, thêm, bớt cho đúng; gắn **mã FR** lấy từ `srs.md` sau khi Tuấn gửi bảng FR.

| Mã UC | Tên | Actor | Mã FR | Ưu tiên |
|---|---|---|---|---|
| UC-AUTH-01 | Đăng ký tài khoản | Khách | FR-AUTH-01 | Must |
| UC-AUTH-02 | Đăng nhập | Khách, Thành viên | FR-AUTH-01 | Must |
| UC-AUTH-03 | Đăng xuất và làm mới token | Thành viên | FR-AUTH-01 | Must |
| UC-AUTH-04 | Đổi mật khẩu / quên mật khẩu | Thành viên | FR-AUTH-01 | Must |
| UC-AUTH-05 | Xem và cập nhật hồ sơ cá nhân | Thành viên | FR-AUTH-03 | Must |
| UC-AUTH-06 | Yêu cầu tham gia gia đình (xác minh thành viên) | Thành viên mới | FR-AUTH-04 | Must |
| UC-AUTH-07 | Duyệt hoặc từ chối yêu cầu tham gia | Trưởng chi | FR-AUTH-04 | Must |
| UC-AUTH-08 | Quản lý người dùng (khóa, đổi vai trò) | Quản trị | FR-ADM-01 | Must |
| UC-AUTH-09 | Kiểm duyệt nội dung | Quản trị | FR-ADM-02 | Must |
| UC-AUTH-10 | Xem audit log | Quản trị | FR-ADM-03 | Must |
| UC-AUTH-11 | Sao lưu và khôi phục dữ liệu | Quản trị | FR-ADM-04 | Must |
| UC-AUTH-12 | Cấu hình hệ thống | Quản trị | FR-ADM-05 | Must |

## 3. Đặc tả từng use case
> Đã tạo sẵn 2 use case đầu. Sao chép mẫu từ `_template.md` cho các use case còn lại. Cần đủ **luồng chính, luồng thay thế, tiền/hậu điều kiện**.

### UC-AUTH-01: Đăng ký tài khoản
| Mục | Nội dung |
|---|---|
| Mã FR liên quan | FR-... |
| Actor | |
| Mô tả ngắn | |
| Tiền điều kiện | |
| Luồng chính | 1. ...<br>2. ...<br>3. ... |
| Luồng thay thế / ngoại lệ | 2a. ... |
| Hậu điều kiện | |
| Quy tắc nghiệp vụ | BR-... |


### UC-AUTH-02: Đăng nhập
| Mục | Nội dung |
|---|---|
| Mã FR liên quan | FR-... |
| Actor | |
| Mô tả ngắn | |
| Tiền điều kiện | |
| Luồng chính | 1. ...<br>2. ...<br>3. ... |
| Luồng thay thế / ngoại lệ | 2a. ... |
| Hậu điều kiện | |
| Quy tắc nghiệp vụ | BR-... |


## 4. Quy tắc nghiệp vụ
| Mã | Quy tắc |
|---|---|
| BR-01 | |

## 5. Thiết kế bảng cơ sở dữ liệu của module
> Gợi ý tên bảng ở dưới. Với mỗi bảng, điền cột, kiểu dữ liệu, khóa, ràng buộc. **Gửi cho TV2 (Trí) gộp vào ERD tổng muộn nhất đầu tuần 2.** Quy ước: tên bảng `snake_case` số ít (ngoại lệ duy nhất là `users`, vì `user` là từ khóa PostgreSQL), có `created_at`, `updated_at`, `deleted_at` (xóa mềm, kiểu `TIMESTAMPTZ`). Khóa ngoại chỉ dùng trong cùng module; tham chiếu sang bảng của module khác chỉ lưu UUID có index, kiểm tra ở service (`architecture.md` mục 7, quyết định 9).

| Bảng | Mục đích | Trạng thái |
|---|---|---|
| `users` | Tài khoản người dùng (mật khẩu BCrypt) | |
| `role` | Vai trò: GUEST, MEMBER, BRANCH_ADMIN, SYSTEM_ADMIN | |
| `user_role` | Gán vai trò cho người dùng | |
| `refresh_token` | Refresh token | |
| `member_verification` | Yêu cầu tham gia gia đình | |
| `audit_log` | Nhật ký thao tác | |
| `system_config` | Tham số cấu hình hệ thống | |

Mẫu mô tả một bảng:

**Bảng `ten_bang`**
| Cột | Kiểu | Khóa / Ràng buộc | Mô tả |
|---|---|---|---|
| id | UUID | PK | |
| created_at | TIMESTAMPTZ | NOT NULL | |
| updated_at | TIMESTAMPTZ | NOT NULL | |
| deleted_at | TIMESTAMPTZ | NULL | xóa mềm |

## 6. API của module
> Chi tiết viết trong `../../04-api/openapi/auth.yaml`. Tóm tắt endpoint ở đây.

| Phương thức | Đường dẫn | Mô tả | Quyền (role) |
|---|---|---|---|
| | /api/v1/... | | |

## 7. Màn hình liên quan
> Dán link Figma và ảnh wireframe (xem `docs/05-ui/figma-links.md`).

## 8. Câu hỏi còn mở
- [ ] ...
