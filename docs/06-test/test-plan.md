# Kế hoạch kiểm thử (Test Plan)

> **Người viết:** TV1 (Tuấn Nguyễn Thanh) | **Reviewer:** TV4 (Huy Quốc) | **Task Jira:** SCRUM-27 | **Hạn nộp review:** Thứ Năm 15/10 (Sprint 2)
> **Trạng thái:** Nháp 
> Cách làm: tạo nhánh `docs/<mã SCRUM>-<tên-ngắn>` từ `develop`, điền vào file này, mở Pull Request vào `develop`. Xem `docs/README.md`.

## 1. Phạm vi và mức kiểm thử
| Mức | Công cụ | Ai làm |
|---|---|---|
| Unit test (backend) | JUnit, Mockito | Chủ module |
| API test | Postman / MockMvc | Chủ module + reviewer |
| Kiểm thử giao diện (thủ công) | | Reviewer |
| Hiệu năng | k6 / JMeter | TV2 |
| Kiểm thử hồi quy toàn hệ thống | | TV1 |

## 2. Quy tắc test chéo
| Người test | Module được test |
|---|---|
| TV1 | TV3 (Community, Events) |
| TV2 | TV4 (Heritage, Directory, Dashboard) |
| TV3 | TV5 (AI) |
| TV4 | TV1 (Auth, Admin) |
| TV5 | TV2 (Gia phả) |

## 3. Mẫu test case
| ID | Module | Mô tả | Tiền điều kiện | Bước thực hiện | Kết quả mong đợi | Kết quả thực tế | Trạng thái | Mã FR |
|---|---|---|---|---|---|---|---|---|
| TC-AUTH-001 | Auth | Đăng nhập đúng | Đã có tài khoản | 1. ... | Nhận JWT | | | FR-AUTH-01 |

## 4. Quy trình báo bug trên Jira
- Loại: Bug. Tiêu đề: `[Module] mô tả ngắn`.
- Nội dung: bước tái hiện, kết quả mong đợi, kết quả thực tế, ảnh chụp.
- Mức độ: **Critical** (chặn demo), **Major** (sai chức năng chính), **Minor** (giao diện, nhỏ).

## 5. Tiêu chí hoàn thành kiểm thử
- Không còn bug Critical/Major.
- Báo cáo kiểm thử đầy đủ (Sprint 7).
