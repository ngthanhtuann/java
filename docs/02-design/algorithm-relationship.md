# Thuật toán xác định quan hệ họ hàng (A là gì của B)

> **Người viết:** TV2 (Nguyễn Minh Trí) | **Reviewer:** TV5 (Lê Nhựt) | **Task Jira:** SCRUM-19 | **Hạn nộp review:** Thứ Năm 8/10
> **Trạng thái:** Nháp 
> Cách làm: tạo nhánh `docs/<mã SCRUM>-<tên-ngắn>` từ `develop`, điền vào file này, mở Pull Request vào `develop`. Xem `docs/README.md`.

## 1. Các quan hệ cần hỗ trợ
Cha/mẹ, con, anh chị em, ông bà, cháu, chú bác, cô dì, cậu dì, anh em họ.

## 2. Cách gọi tên họ hàng trong tiếng Việt
> Bên nội / bên ngoại, hơn tuổi / kém tuổi, xưng hô hai chiều.

## 3. Thuật toán
1. Lấy tất cả tổ tiên của A và của B bằng truy vấn đệ quy (recursive CTE) trên `parent_child`, kèm số bước.
2. Tìm tổ tiên chung gần nhất (LCA).
3. Đếm số bước từ A và từ B lên tổ tiên chung: `(a, b)`.
4. Tra bảng ánh xạ `(a, b)` sang tên quan hệ.

### Bảng ánh xạ `(a, b)` → quan hệ
| Bước từ A | Bước từ B | Quan hệ của A với B | Ghi chú (nội/ngoại, hơn/kém tuổi) |
|---|---|---|---|
| 0 | 1 | A là cha/mẹ của B | |
| 1 | 0 | A là con của B | |
| 1 | 1 | Anh chị em | |
| | | | |

### Mã giả
```
function relationship(A, B):
    ...
```
### Độ phức tạp
> ...

## 4. Bảng ca kiểm thử mẫu (>= 20 ca)
> Đầu vào là một sơ đồ gia phả mẫu; gửi TV5 (Lê Nhựt) để viết test tự động.

| # | A | B | Kết quả mong đợi | Giải thích |
|---|---|---|---|---|
| 1 | | | | |
