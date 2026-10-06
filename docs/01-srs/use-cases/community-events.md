# Use case: Community và Events

> **Người viết:** TV3 (PiLo257) | **Reviewer:** TV1 (Tuấn Nguyễn Thanh) | **Task Jira:** SCRUM-20 | **Hạn nộp review:** Thứ Năm 8/10
> **Trạng thái:** Nháp 
> Cách làm: tạo nhánh `docs/<mã SCRUM>-<tên-ngắn>` từ `develop`, điền vào file này, mở Pull Request vào `develop`. Xem `docs/README.md`.

## 1. Phạm vi và Actor
> Module này làm gì, ai dùng. Mô tả cách phân trang bảng tin (cursor hoặc page/size) và quy tắc ai được xem/sửa/xóa bài.

## 2. Danh sách use case
> Dưới đây là gợi ý ban đầu. Chỉnh, thêm, bớt cho đúng; gắn **mã FR** lấy từ `srs.md` sau khi Tuấn gửi bảng FR.

| Mã UC | Tên | Actor | Mã FR | Ưu tiên |
|---|---|---|---|---|
| UC-COM-01 | Tạo, sửa, xóa bài đăng | Thành viên | FR-COM-01 | Must |
| UC-COM-02 | Bình luận | Thành viên | FR-COM-02 | Must |
| UC-COM-03 | Thả reaction | Thành viên | FR-COM-02 | Must |
| UC-COM-04 | Chia sẻ tin tức gia đình | Trưởng chi | FR-COM-03 | Must |
| UC-COM-05 | Tải và chia sẻ ảnh | Thành viên | FR-COM-04 | Must |
| UC-COM-06 | Đăng thông báo (announcement) | Trưởng chi | FR-COM-05 | Must |
| UC-EVT-01 | Tạo sự kiện | Thành viên, Trưởng chi | FR-EVT-01 | Must |
| UC-EVT-02 | RSVP (Tham dự / Không / Có thể) | Thành viên | FR-EVT-02 | Must |
| UC-EVT-03 | Quản lý người tham dự | Người tạo sự kiện | FR-EVT-03 | Must |
| UC-EVT-04 | Thư viện ảnh sự kiện | Thành viên | FR-EVT-04 | Must |
| UC-EVT-05 | Nhắc nhở sự kiện | Hệ thống | FR-EVT-05 | Must |

## 3. Đặc tả từng use case
> Đã tạo sẵn 2 use case đầu. Sao chép mẫu từ `_template.md` cho các use case còn lại. Cần đủ **luồng chính, luồng thay thế, tiền/hậu điều kiện**.

### UC-COM-01: Tạo, sửa, xóa bài đăng
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


### UC-COM-02: Bình luận
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
> Gợi ý tên bảng ở dưới. Với mỗi bảng, điền cột, kiểu dữ liệu, khóa, ràng buộc. **Gửi cho TV2 (Trí) gộp vào ERD tổng muộn nhất đầu tuần 2.** Quy ước: tên bảng `snake_case` số ít, có `created_at`, `updated_at`, `deleted_at` (xóa mềm).

| Bảng | Mục đích | Trạng thái |
|---|---|---|
| `post` | Bài đăng (có trường loại: POST / NEWS) | |
| `comment` | Bình luận | |
| `reaction` | Reaction | |
| `media` | Ảnh và tệp tải lên | |
| `announcement` | Thông báo ghim | |
| `notification` | Thông báo gửi người dùng | |
| `event` | Sự kiện | |
| `event_participant` | Người tham dự và trạng thái RSVP | |

Mẫu mô tả một bảng:

**Bảng `ten_bang`**
| Cột | Kiểu | Khóa / Ràng buộc | Mô tả |
|---|---|---|---|
| id | UUID | PK | |
| created_at | TIMESTAMP | NOT NULL | |
| updated_at | TIMESTAMP | NOT NULL | |
| deleted_at | TIMESTAMP | NULL | xóa mềm |

## 6. API của module
> Chi tiết viết trong `../../04-api/openapi/community-events.yaml`. Tóm tắt endpoint ở đây.

| Phương thức | Đường dẫn | Mô tả | Quyền (role) |
|---|---|---|---|
| | /api/v1/... | | |

## 7. Màn hình liên quan
> Dán link Figma và ảnh wireframe (xem `docs/05-ui/figma-links.md`).

## 8. Câu hỏi còn mở
- [ ] ...
