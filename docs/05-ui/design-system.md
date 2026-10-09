# Design system

> **Người viết:** TV5 (Lê Nhựt) | **Reviewer:** TV3 (PiLo257) | **Task Jira:** SCRUM-25 | **Hạn nộp review:** Thứ Năm 8/10
> **Trạng thái:** Nháp 
> Cách làm: tạo nhánh `docs/<mã SCRUM>-<tên-ngắn>` từ `develop`, điền vào file này, mở Pull Request vào `develop`. Xem `docs/README.md`.

## 1. Bảng màu
| Vai trò | Mã màu | Dùng cho |
|---|---|---|
| Chủ đạo | #2C1810 | Sidebar, tiêu đề, chữ chính, nền tiêu đề form |
| Phụ | #C9A84C | Nút chính, mục điều hướng đang chọn, điểm nhấn |
| Thành công | #2E7D32	| Thông báo thành công, badge “Mới”, trạng thái hợp lệ |
| Lỗi | #C62828 | Lỗi nhập liệu, cảnh báo lỗi, thao tác xóa |

Các màu hỗ trợ:

| Token	| Mã màu | Dùng cho |
|---|---|---|
| Background| #F5F0E8 | Nền trang |
| Surface |	#FFFFFF | Card, modal, header |
| Surface muted | #F9F6F0 | Nền ô nhập, vùng nội dung phụ |
| Text secondary |	#8B7355 |	Mô tả, nhãn phụ |
| Border |	#D4C4A8 |	Viền ô nhập, đường phân cách |
| Success background |	#E8F5E9 |	Nền thông báo thành công |
| Error background | #FCE4EC |	Nền thông báo lỗi |
| Info | #1565C0 |	Badge “Đã cập nhật”, thông tin |
| Warning |	#E65100 |	Badge “Chờ xác nhận” |
## 2. Font và cỡ chữ (hỗ trợ tiếng Việt)
| Cấp | Font | Cỡ |
|---|---|---|
| Tiêu đề trang | Inter Bold — 700 | 24px, line-height 32px |
| Tiêu đề card/modal | Inter Semi Bold — 600 | 18px, line-height 26px |
| Nội dung | Inter Regular — 400 | 14px, line-height 22px |
| Nhãn form, nút | Inter Medium/Semi Bold — 500/600 | 14px, line-height 20px |
| Chú thích, badge | Inter Regular/Medium — 400/500 | 12px, line-height 18px |
| Số liệu thống kê | Inter Bold — 700 | 28px, line-height 36px |

## 3. Component
| Component | Đặc tả đề xuất |
|---|---|
 |Nút |	Cao 40px; mobile 44px; bo góc 8px. Primary nền vàng/chữ nâu; secondary nền trắng/viền beige; danger nền đỏ/chữ trắng. Có hover, focus, disabled, loading. |
 |Ô nhập / select |	Cao 44px; chữ 14px; bo góc 8px; label phía trên. Focus có vòng viền rõ; lỗi có viền đỏ và thông báo dưới ô. |
| Bảng |	Header nền kem; hàng tối thiểu 44px; chữ 14px; đường phân cách nhẹ. Mobile cho cuộn ngang hoặc chuyển sang danh sách card. |
| Card |	Nền trắng; bo góc 12px; padding 20–24px; viền nhẹ hoặc bóng nhẹ. |
| Modal |	Rộng tối đa 640px, không vượt chiều rộng màn hình trừ 32px; có tiêu đề, đóng, nội dung và vùng hành động. Nội dung dài cuộn bên trong. |
| Thanh điều hướng |	Sidebar nền nâu đậm; mục đang chọn nền vàng/chữ nâu. Mobile chuyển thành menu drawer. |
| Avatar |	Hình tròn; cỡ 32/40/48px; có ảnh hoặc chữ viết tắt tên. Không dùng màu làm dấu hiệu phân biệt duy nhất. |
| Badge |	Chữ 12px; bo tròn; nền nhạt và chữ đậm theo trạng thái. Luôn có nhãn chữ. |
| Toast |	Có icon, thông điệp và nút đóng; success/error/info/warning. Desktop góc trên phải, mobile bên dưới header. |
## 4. Layout
Header, sidebar, vùng nội dung; có bản responsive.

- Sidebar: 240px.
- Header: 64px.
- Nội dung: padding ngang 28px, dọc 24px.
- Khoảng cách giữa các khu vực: 24px.
- Bố cục: thống kê phía trên, các khu vực nội dung chia hai cột.
Responsive đề xuất
Thiết kế hiện tại mới có bản desktop; chưa có bản tablet/mobile riêng.
| Kích thước màn hình |	Cách bố trí |
|---|---|
| Desktop ≥ 1024px |	Sidebar 240px; nội dung hai cột khi đủ chỗ; thống kê bốn cột |
| Tablet 768–1023px |	Sidebar chuyển thành drawer; thống kê hai cột; nội dung chính một cột |
| Mobile < 768px |	Header gọn; padding 16px; form chọn hai người xếp dọc; nút có thể xuống dòng |
| Mobile nhỏ < 480px |	Thống kê một cột nếu nội dung không đủ chỗ |

> Mã màu và cỡ chữ cũng ghi vào `tailwind.config` của frontend.
export default {
  theme: {
    extend: {
      colors: {
        primary: "#2C1810",
        secondary: "#C9A84C",
        success: "#2E7D32",
        error: "#C62828",
        info: "#1565C0",
        warning: "#E65100",

        background: "#F5F0E8",
        surface: "#FFFFFF",
        "surface-muted": "#F9F6F0",
        "text-primary": "#2C1810",
        "text-secondary": "#8B7355",
        border: "#D4C4A8",
        "success-soft": "#E8F5E9",
        "error-soft": "#FCE4EC",
      },

      fontFamily: {
        sans: ["Inter", "sans-serif"],
      },

      fontSize: {
        "page-title": ["24px", { lineHeight: "32px", fontWeight: "700" }],
        "section-title": ["18px", { lineHeight: "26px", fontWeight: "600" }],
        body: ["14px", { lineHeight: "22px" }],
        label: ["14px", { lineHeight: "20px", fontWeight: "500" }],
        caption: ["12px", { lineHeight: "18px" }],
        metric: ["28px", { lineHeight: "36px", fontWeight: "700" }],
      },

      borderRadius: {
        control: "8px",
        card: "12px",
      },

      spacing: {
        sidebar: "240px",
        header: "64px",
      },
    },
  },
};
