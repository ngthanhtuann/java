# Bảo mật và quyền riêng tư dữ liệu gia đình

> **Người viết:** TV1 (Tuấn Nguyễn Thanh) | **Reviewer:** TV4 (Huy Quốc) | **Task Jira:** SCRUM-17 | **Hạn nộp review:** Thứ Tư 7/10 đến Thứ Năm 8/10
> **Trạng thái:** Nháp 
> Cách làm: tạo nhánh `docs/<mã SCRUM>-<tên-ngắn>` từ `develop`, điền vào file này, mở Pull Request vào `develop`. Xem `docs/README.md`.

## 1. Dữ liệu nhạy cảm
| Loại dữ liệu | Ví dụ | Mức nhạy cảm |
|---|---|---|
| Thông tin người còn sống | Họ tên, ngày sinh, số điện thoại, địa chỉ | |
| Trẻ em | | |
| Người đã mất | | |
| Tài khoản | Email, mật khẩu | |

## 2. Quy tắc hiển thị: trường dữ liệu theo vai trò
| Trường dữ liệu | Khách | Thành viên chưa xác minh | Thành viên | Trưởng chi | Quản trị |
|---|---|---|---|---|---|
| Họ tên | | | | | |
| Ngày sinh | | | | | |
| Số điện thoại | | | | | |
| Địa chỉ | | | | | |
| Cây gia phả | | | | | |

## 3. Quy tắc cho trẻ em và người đã mất
> Ví dụ: ẩn ngày sinh đầy đủ của trẻ em, chỉ hiện năm sinh.

## 4. Dữ liệu gửi cho LLM bên ngoài
> Chỉ gửi đoạn cần thiết; không gửi mật khẩu, số điện thoại. Ghi rõ cách lọc theo `family_id` và quyền xem.

## 5. Xử lý bảo mật kỹ thuật
- Mật khẩu: BCrypt. Token: JWT (access ngắn hạn + refresh token).
- Secret lưu bằng biến môi trường, không commit lên Git.
- CORS, rate limit, kiểm tra IDOR.

> Tóm tắt nội dung này đưa vào mục NFR trong SRS và mục "Other comments" của đề cương.
