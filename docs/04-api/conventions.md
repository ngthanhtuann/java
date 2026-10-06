# Quy ước API chung

> **Người viết:** TV3 (PiLo257) | **Reviewer:** TV1 (Tuấn Nguyễn Thanh) | **Task Jira:** SCRUM-21 | **Hạn nộp review:** Bản nháp Thứ Ba 6/10, chốt Thứ Tư 7/10
> **Trạng thái:** Nháp 
> Cách làm: tạo nhánh `docs/<mã SCRUM>-<tên-ngắn>` từ `develop`, điền vào file này, mở Pull Request vào `develop`. Xem `docs/README.md`.

> Đường găng của Sprint 1: mọi file OpenAPI của các module phải theo quy ước này.

## 1. URL và phương thức
- Tiền tố: `/api/v1/...`; tên tài nguyên **số nhiều**, chữ thường (`/families`, `/persons`).
- `GET` đọc, `POST` tạo, `PUT` sửa toàn bộ, `PATCH` sửa một phần, `DELETE` xóa.

## 2. Cấu trúc response
**Thành công**
```json
{ "success": true, "data": { }, "meta": { "page": 0, "size": 20, "total": 100 } }
```
**Lỗi**
```json
{ "success": false, "error": { "code": "AUTH_001", "message": "Sai email hoặc mật khẩu", "details": [] } }
```

## 3. Bảng mã lỗi
| Mã | HTTP | Ý nghĩa |
|---|---|---|
| AUTH_001 | 401 | Sai email hoặc mật khẩu |
| AUTH_002 | 401 | Token hết hạn hoặc không hợp lệ |
| PERM_001 | 403 | Không đủ quyền |
| VALID_001 | 400 | Dữ liệu không hợp lệ |
| NOT_FOUND_001 | 404 | Không tìm thấy |
| | | |

## 4. Phân trang và sắp xếp
`?page=0&size=20&sort=created_at,desc` (page bắt đầu từ 0).

## 5. Định dạng dữ liệu
- Ngày giờ: ISO-8601 (`2026-10-09T23:30:00+07:00`).
- Header xác thực: `Authorization: Bearer <access_token>`.
- Mã hóa UTF-8.

## 6. Bảo mật
> CORS, giới hạn kích thước request, rate limit đăng nhập và chat.
