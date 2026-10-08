# Quy ước API chung

> **Người viết:** TV3 (PiLo257) | **Reviewer:** TV1 (Tuấn Nguyễn Thanh)
> **Task Jira:** SCRUM-21 | **Trạng thái:** Nháp

Tài liệu này là quy ước chung cho các API của FamilyConnect. Các file OpenAPI của các module phải tuân theo các quy ước dưới đây.

## 1. URL và HTTP Method

### 1.1. URL

- API sử dụng tiền tố `/api/v1/...`.
- Tên tài nguyên dùng dạng số nhiều, chữ thường.

Ví dụ:

```text
/api/v1/families
/api/v1/events
/api/v1/posts
```

### 1.2. HTTP Method

| Method | Mục đích |
|---|---|
| GET | Đọc dữ liệu |
| POST | Tạo dữ liệu mới |
| PUT | Cập nhật toàn bộ |
| PATCH | Cập nhật một phần |
| DELETE | Xóa dữ liệu |
## 2. Cấu trúc Response

### 2.1. Thành công

```json
{
  "success": true,
  "data": {},
  "meta": {
    "page": 0,
    "size": 20,
    "total": 100
  }
}
```

- `success`: trạng thái request.
- `data`: dữ liệu trả về.
- `meta`: thông tin bổ sung, dùng cho phân trang.
### 2.2. Lỗi

```json
{
  "success": false,
  "error": {
    "code": "AUTH_001",
    "message": "Sai email hoặc mật khẩu",
    "details": []
  }
}
```

Đây là cấu trúc lỗi thống nhất cho web/mobile.

## 3. Mã lỗi

| Mã | HTTP | Ý nghĩa |
|---|---:|---|
| AUTH_001 | 401 | Sai email hoặc mật khẩu |
| AUTH_002 | 401 | Token hết hạn hoặc không hợp lệ |
| PERM_001 | 403 | Không đủ quyền |
| PERM_002 | 403 | Thành viên chưa được xác minh |
| VALID_001 | 400 | Dữ liệu không hợp lệ |
| NOT_FOUND_001 | 404 | Không tìm thấy tài nguyên |
| RATE_001 | 429 | Vượt quá giới hạn request |

API phải sử dụng đúng HTTP status tương ứng với lỗi.
## 4. Phân trang

API trả về danh sách sử dụng:
```text
?page=0&size=20&sort=created_at,desc
```
- `page`: số trang, bắt đầu từ 0.
- `size`: số lượng bản ghi.
- `sort`: trường và chiều sắp xếp (`asc` hoặc `desc`).

Thông tin phân trang nằm trong meta.

## 5. Định dạng dữ liệu

- Ngày giờ sử dụng ISO-8601.
- JSON sử dụng camelCase.
- Encoding sử dụng UTF-8.

Ví dụ:

```json
{
  "createdAt": "2026-10-09T23:30:00+07:00",
  "updatedAt": "2026-10-09T23:30:00+07:00"
}
```

## 6. Authentication và Authorization

API yêu cầu đăng nhập sử dụng:

```text
Authorization: Bearer <access_token>
```

Backend phải kiểm tra quyền của người dùng và trạng thái xác minh thành viên khi chức năng yêu cầu.

```json
{
  "success": false,
  "error": {
    "code": "PERM_001",
    "message": "Không đủ quyền",
    "details": []
  }
}
```

## 7. Bảo mật

- Cấu hình CORS phù hợp với môi trường.
- Giới hạn kích thước request.
- Áp dụng rate limit cho API đăng nhập và chat.
- Request vượt giới hạn trả về HTTP 429 và `RATE_001`.
## 8. Quy ước OpenAPI

Các file OpenAPI đặt tại:

```text
docs/04-api/openapi/
```
Mỗi endpoint cần mô tả:

- Method và URL.
- Request parameters/body.
- Response thành công.
- Response lỗi.
- HTTP status.
- Authentication/quyền truy cập nếu có.

DTO web/mobile được định nghĩa theo schema trong OpenAPI và sử dụng camelCase.

Các module phải tuân theo tài liệu này khi xây dựng API mới.