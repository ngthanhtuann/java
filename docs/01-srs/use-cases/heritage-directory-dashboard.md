# Use case: Heritage, Directory, Dashboard

> **Người viết:** TV4 (Huy Quốc) | **Reviewer:** TV2 (Nguyễn Minh Trí) | **Task Jira:** SCRUM-22 | **Hạn nộp review:** Thứ Năm 8/10
> **Trạng thái:** Nháp 
> Cách làm: tạo nhánh `docs/<mã SCRUM>-<tên-ngắn>` từ `develop`, điền vào file này, mở Pull Request vào `develop`. Xem `docs/README.md`.

## 1. Phạm vi và Actor
> Module này làm gì, ai dùng. **Dashboard:** liệt kê từng chỉ số (tổng thành viên, theo thế hệ, nam/nữ, nghề nghiệp, nơi ở, bài đăng theo tuần, sự kiện sắp tới) kèm câu SQL dự kiến ở mục 4.

## 2. Danh sách use case
> Dưới đây là gợi ý ban đầu. Chỉnh, thêm, bớt cho đúng; gắn **mã FR** lấy từ `srs.md` sau khi Tuấn gửi bảng FR.

| Mã UC | Tên | Actor | Mã FR | Ưu tiên |
|---|---|---|---|---|
| UC-HER-01 | Quản lý tài liệu lịch sử | Trưởng chi | FR-HER-01 | Must |
| UC-HER-02 | Viết câu chuyện gia đình | Thành viên | FR-HER-02 | Must |
| UC-HER-03 | Quản lý người tiêu biểu | Trưởng chi | FR-HER-03 | Must |
| UC-HER-04 | Thư viện ảnh (album) | Thành viên | FR-HER-04 | Must |
| UC-HER-05 | Kho lưu trữ số (digital archives) | Trưởng chi | FR-HER-05 | Must |
| UC-DIR-01 | Quản lý hồ sơ nghề nghiệp | Thành viên | FR-DIR-02 | Must |
| UC-DIR-02 | Quản lý hồ sơ học vấn | Thành viên | FR-DIR-03 | Must |
| UC-DIR-03 | Tìm thành viên theo nghề, nơi ở, thế hệ | Thành viên | FR-DIR-04 | Must |
| UC-DSH-01 | Xem dashboard thống kê | Trưởng chi, Quản trị | FR-DSH-01..04 | Must |
| UC-DSH-02 | Xuất dữ liệu CSV | Trưởng chi, Quản trị | FR-DSH-05 | Must |
| UC-DSH-03 | Tạo báo cáo PDF | Trưởng chi, Quản trị | FR-DSH-05 | Must |

## 3. Đặc tả từng use case
> Đã tạo sẵn 2 use case đầu. Sao chép mẫu từ `_template.md` cho các use case còn lại. Cần đủ **luồng chính, luồng thay thế, tiền/hậu điều kiện**.

### UC-HER-01: Quản lý tài liệu lịch sử
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


### UC-HER-02: Viết câu chuyện gia đình
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
| `heritage_document` | Tài liệu lịch sử | |
| `family_story` | Câu chuyện gia đình | |
| `outstanding_member` | Người tiêu biểu | |
| `album` | Album ảnh | |
| `digital_archive` | Kho lưu trữ số | |
| `profession_profile` | Hồ sơ nghề nghiệp | |
| `education_profile` | Hồ sơ học vấn | |

Mẫu mô tả một bảng:

**Bảng `ten_bang`**
| Cột | Kiểu | Khóa / Ràng buộc | Mô tả |
|---|---|---|---|
| id | UUID | PK | |
| created_at | TIMESTAMP | NOT NULL | |
| updated_at | TIMESTAMP | NOT NULL | |
| deleted_at | TIMESTAMP | NULL | xóa mềm |

## 6. API của module
> Chi tiết viết trong `../../04-api/openapi/heritage-directory-dashboard.yaml`. Tóm tắt endpoint ở đây.

| Phương thức | Đường dẫn | Mô tả | Quyền (role) |
|---|---|---|---|
| | /api/v1/... | | |

## 7. Màn hình liên quan
> Dán link Figma và ảnh wireframe (xem `docs/05-ui/figma-links.md`).

## 8. Câu hỏi còn mở
- [ ] ...
