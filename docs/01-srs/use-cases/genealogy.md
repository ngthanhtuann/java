# Use case: Gia phả (family, branch, person, quan hệ)

> **Người viết:** TV2 (Nguyễn Minh Trí) | **Reviewer:** TV5 (Lê Nhựt) | **Task Jira:** SCRUM-18 | **Hạn nộp review:** Thứ Năm 8/10
> **Trạng thái:** Nháp 
> Cách làm: tạo nhánh `docs/<mã SCRUM>-<tên-ngắn>` từ `develop`, điền vào file này, mở Pull Request vào `develop`. Xem `docs/README.md`.

## 1. Phạm vi và Actor
> Module này làm gì, ai dùng. **Quy tắc cần ghi rõ:** tối đa 2 cha/mẹ ruột, cha mẹ sinh trước con, chặn vòng lặp tổ tiên, xử lý người đã mất, ly hôn, tái hôn, con nuôi. API cây trả về dạng `nodes`/`edges` cho React Flow.

## 2. Danh sách use case
> Dưới đây là gợi ý ban đầu. Chỉnh, thêm, bớt cho đúng; gắn **mã FR** lấy từ `srs.md` sau khi Tuấn gửi bảng FR.

| Mã UC | Tên | Actor | Mã FR | Ưu tiên |
|---|---|---|---|---|
| UC-GEN-01 | Tạo gia đình | Trưởng chi | FR-GEN-01 | Must |
| UC-GEN-02 | Quản lý chi họ | Trưởng chi | FR-GEN-02 | Must |
| UC-GEN-03 | Thêm, sửa, xóa thành viên | Trưởng chi | FR-GEN-03 | Must |
| UC-GEN-04 | Thêm quan hệ cha-mẹ-con | Trưởng chi | FR-GEN-04 | Must |
| UC-GEN-05 | Thêm hôn nhân | Trưởng chi | FR-GEN-05 | Must |
| UC-GEN-06 | Xem cây gia phả | Thành viên | FR-GEN-06 | Must |
| UC-GEN-07 | Mở và thu nhánh cây | Thành viên | FR-GEN-06 | Must |
| UC-GEN-08 | Tra quan hệ A là gì của B | Thành viên | FR-GEN-08 | Must |
| UC-GEN-09 | Làm nổi bật đường quan hệ trên cây | Thành viên | FR-GEN-07 | Must |

## 3. Đặc tả từng use case
> Đã tạo sẵn 2 use case đầu. Sao chép mẫu từ `_template.md` cho các use case còn lại. Cần đủ **luồng chính, luồng thay thế, tiền/hậu điều kiện**.

### UC-GEN-01: Tạo gia đình
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


### UC-GEN-02: Quản lý chi họ
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
| `family` | Gia đình / dòng họ | |
| `branch` | Chi họ | |
| `person` | Thành viên (người) | |
| `parent_child` | Quan hệ cha/mẹ - con (ruột/nuôi) | |
| `marriage` | Hôn nhân (ngày cưới, ly hôn) | |

Mẫu mô tả một bảng:

**Bảng `ten_bang`**
| Cột | Kiểu | Khóa / Ràng buộc | Mô tả |
|---|---|---|---|
| id | UUID | PK | |
| created_at | TIMESTAMP | NOT NULL | |
| updated_at | TIMESTAMP | NOT NULL | |
| deleted_at | TIMESTAMP | NULL | xóa mềm |

## 6. API của module
> Chi tiết viết trong `../../04-api/openapi/genealogy.yaml`. Tóm tắt endpoint ở đây.

| Phương thức | Đường dẫn | Mô tả | Quyền (role) |
|---|---|---|---|
| | /api/v1/... | | |

## 7. Màn hình liên quan
> Dán link Figma và ảnh wireframe (xem `docs/05-ui/figma-links.md`).

## 8. Câu hỏi còn mở
- [ ] ...
