# ADR: Spike AI, RAG, Spring AI, pgvector

> **Người làm:** TV5 (Lê Nhựt) | **Reviewer:** TV3 (PiLo257) | **Task Jira:** SCRUM-24
> **Trạng thái:** Chờ review (còn các mục chưa đo, bổ sung ở Sprint 2) | **Ngày thử nghiệm:** chưa ghi, bổ sung ở SCRUM-34 (Sprint 2)
> Mã thử nghiệm nằm ở nhánh `spike/SCRUM-24-rag` (không merge vào `develop`). **Không có API key trong repo.** Thiết kế chi tiết RAG viết ở `ai-rag.md` (SCRUM-34, Sprint 2).

## 1. Bối cảnh

FamilyConnect cần chatbot hỏi đáp gia phả và tìm kiếm ngữ nghĩa bằng tiếng Việt (FR-AI trong `srs.md`). Spike này kiểm tra ba việc: tạo embedding tiếng Việt, lưu và tìm kiếm bằng pgvector, gọi LLM với ngữ cảnh đính kèm. Từ đó chọn nhà cung cấp LLM.

## 2. Môi trường thử

| Hạng mục | Giá trị |
|---|---|
| Nhà cung cấp | Google Gemini API (gói miễn phí) |
| Model embedding | `gemini-embedding-001` |
| Model LLM | Chưa đo, bổ sung ở SCRUM-34 (Sprint 2) |
| Thư viện | Spring AI, pgvector (phiên bản chưa ghi, bổ sung ở SCRUM-34 (Sprint 2)) |
| Dữ liệu thử | 10 câu tiếng Việt về gia phả (dữ liệu giả, không dùng dữ liệu thật của gia đình) |
| Khóa API | Biến môi trường `GEMINI_API_KEY`, không commit |

## 3. Kết quả

### 3.1. Embedding tiếng Việt

| Chỉ số | Kết quả |
|---|---|
| Số chiều mặc định của model | 3072 |
| Thời gian tạo embedding trung bình | 683,5 ms (số lần đo: Chưa đo, bổ sung ở SCRUM-34 (Sprint 2)) |

### 3.2. Tìm kiếm ngữ nghĩa (10 câu, lấy 3 câu gần nhất)

| Chỉ số | Kết quả |
|---|---|
| Thời gian tìm kiếm trung bình | 588,3 ms |
| P95 | 628,2 ms |
| Thời gian trên đã gồm bước tạo embedding cho câu hỏi chưa? | Chưa đo, bổ sung ở SCRUM-34 (Sprint 2) |

Chất lượng tiếng Việt: Chưa có bảng đánh giá 10 câu; bổ sung ở Sprint 2.

### 3.3. Gọi LLM với ngữ cảnh

| Chỉ số | Kết quả |
|---|---|
| Độ trễ trả lời (đo từ lúc gửi đến khi nhận đủ) | Chưa đo, bổ sung ở SCRUM-34 (Sprint 2) |
| Chất lượng câu trả lời tiếng Việt | Chưa đo, bổ sung ở SCRUM-34 (Sprint 2) |
| Hành vi khi ngữ cảnh không có thông tin | Chưa đo, bổ sung ở SCRUM-34 (Sprint 2) |

### 3.4. Chi phí ước tính

| Hạng mục | Giá |
|---|---|
| LLM, đầu vào | $0,75 / 1 triệu token |
| LLM, đầu ra | $3,75 / 1 triệu token |
| Mức giá áp dụng đến | 31/12/2026; từ 01/01/2027 tăng gấp đôi ($1,50 / $7,50) |
| Gói miễn phí | Đầu vào và đầu ra miễn phí, nhưng nội dung được Google dùng để cải thiện sản phẩm |
| Nguồn | https://ai.google.dev/gemini-api/docs/pricing (tra ngày 09/10/2026; giá trên áp dụng cho dòng Gemini Flash 3.6, 3.7, 3.8; tên model đã dùng: Chưa đo, bổ sung ở SCRUM-34 (Sprint 2)) |

Ước tính cho nhóm: Chưa đo, bổ sung ở SCRUM-34 (Sprint 2).

## 4. Phát hiện kỹ thuật cần xử lý

1. **Số chiều vector.** `gemini-embedding-001` trả 3072 chiều mặc định, nhưng pgvector chỉ tạo được chỉ mục HNSW và IVFFlat cho kiểu `vector` tối đa 2000 chiều. Hai cách xử lý:
   - Chọn `outputDimensionality` 768 hoặc 1536 khi gọi API (đề xuất, vì nhẹ và đủ cho gia phả), hoặc
   - Dùng kiểu `halfvec(3072)` (chỉ mục tối đa 4000 chiều).
   - **Quyết định cho thiết kế:** chưa chốt (768, 1536 hoặc `halfvec`); chốt ở SCRUM-34 và ghi vào `ai-rag.md`.
2. **Tên bảng.** Bảng vector đặt là `ai_embedding_chunk` (tiền tố `ai_` theo `architecture.md` mục 7), không dùng `embedding_chunk`.

## 5. Rủi ro

| Rủi ro | Mức | Cách giảm |
|---|---|---|
| Gói miễn phí giới hạn số yêu cầu, có thể hết hạn mức khi demo | Cao | Cache câu trả lời (`ai_summary_cache`), giới hạn số câu hỏi mỗi người mỗi giờ (RATE_001), có phương án chuyển nhà cung cấp |
| Dữ liệu gia đình gửi ra dịch vụ bên ngoài; theo trang giá của Google, nội dung gửi qua gói miễn phí được dùng để cải thiện sản phẩm | Cao | Chỉ gửi ngữ cảnh đã lọc theo `family_id` và quyền xem; không gửi mật khẩu, số điện thoại, thông tin trẻ dưới 16 tuổi (`privacy.md`); dùng dữ liệu giả khi thử; chuyển sang gói trả phí (không dùng dữ liệu để huấn luyện) trước khi chạy với dữ liệu thật |
| LLM bịa thông tin ngoài ngữ cảnh | Trung bình | Prompt chỉ cho trả lời theo ngữ cảnh; ngưỡng điểm tối thiểu; kèm nguồn trong câu trả lời |
| Độ trễ khoảng 0,6 giây cho mỗi bước tìm kiếm, cộng thời gian LLM | Trung bình | Tìm kiếm và gọi LLM chạy bất đồng bộ trên giao diện; có chỉ báo đang tải; timeout trả AI_001 |
| Phụ thuộc một nhà cung cấp | Thấp | Gọi qua interface của Spring AI để đổi nhà cung cấp bằng cấu hình |
| Giá LLM tăng gấp đôi từ 01/01/2027 | Trung bình | Tính chi phí theo giá mới trong báo cáo cuối kỳ; cân nhắc giới hạn số câu hỏi |

## 6. Quyết định

**Quyết định sơ bộ chọn Gemini, xác nhận sau khi hoàn tất đo ở Sprint 2.** Google Gemini dự kiến là nhà cung cấp LLM và embedding cho giai đoạn đồ án, tích hợp qua Spring AI.

Lý do sơ bộ: đã tích hợp được, đã đo thời gian tạo embedding và tìm kiếm (mục 3.1, 3.2), có gói miễn phí phù hợp phát triển. Chất lượng tiếng Việt và độ trễ gọi LLM chưa đo (mục 3.2, 3.3). Chưa có so sánh với OpenAI; bổ sung ở SCRUM-34 (Sprint 2) nếu có thử.

Điều kiện đi kèm: giải quyết số chiều vector (mục 4), thực hiện các biện pháp giảm rủi ro ở mục 5.

## 7. Việc tiếp theo

- SCRUM-34: viết `ai-rag.md` (phân đoạn dữ liệu, prompt hệ thống, ngưỡng điểm, giới hạn, quyền dữ liệu) dựa trên quyết định này.
- Bảng `ai_embedding_chunk` đưa vào ERD (SCRUM-28) với số chiều đã chốt.
