# So sánh FIDES với các hệ thống IFC kinh điển
> **Thông Tin Tài Liệu / Document Metadata:**
> - **Tác giả / Học viên:** Võ Phạm Tuấn Dũng (MSHV: 2570015) — `@tuandung222`
> - **Cán bộ Hướng dẫn:** TS. Lê Xuân Bách
> - **Chuyên ngành:** Thạc sĩ Khoa học Dữ liệu & Trí tuệ Nhân tạo Ứng dụng (CDNC AI - Khóa 261)
> - **Đơn vị:** Khoa Khoa học & Kỹ thuật Máy tính, Trường ĐH Bách Khoa, ĐHQG-HCM (HCMUT)
> - **Đề tài:** TrustSight (*An Empirical Study of Anchored Visual Perception for Prompt-Injection-Resistant Multimodal Agents*)
> - **Kho Lưu Trữ:** `tuandung222/software-security-agent-defenses`
> - **Ngày giờ biên soạn / Cập nhật (Timestamp):** 2026-10-06 01:45:00 (GMT+7)

## 1. Giới thiệu

Hệ thống điều khiển luồng thông tin (IFC) đã được nghiên cứu từ lâu với các ngôn ngữ bảo mật cấp hệ điều hành hoặc ngôn ngữ lập trình cụ thể như Jif (Java Information Flow), HiStar (OS), Flume. FIDES mang các ý tưởng này ứng dụng vào mô hình sinh ngôn ngữ phi xác định (LLM Agents).

## 2. Bảng so sánh nhanh

| Đặc điểm | Hệ thống IFC Kinh điển (Jif, Flume, HiStar) | FIDES / LLM IFC |
| :--- | :--- | :--- |
| **Môi trường thực thi** | Ngôn ngữ tĩnh, Hệ điều hành (Kernel) | Môi trường Prompt và luồng thực thi phi xác định của LLM |
| **Gán nhãn (Labeling)** | Compile-time tĩnh, OS process tagging | Runtime Taint Tracking dựa vào ngữ cảnh dữ liệu của Tool Calls |
| **Độ linh hoạt** | Khắt khe, yêu cầu người code annotate (Jif) | Uyển chuyển hơn, giải quyết qua các cơ chế Declassification |
| **Xử lý Declassification** | Xác định, tĩnh | Xác suất, Probabilistic declassification (thách thức chính) |

## 3. Thách thức cho LLM: Probabilistic Declassification

Một trong những khác biệt cốt lõi là bản chất xác suất của sinh ngôn ngữ từ LLM. LLM có thể vô tình tiết lộ thông tin qua cách diễn đạt hay cấu trúc dữ liệu xuất ra. Các hệ thống IFC tĩnh xử lý các vi phạm qua logic nhị phân truyền thống, nhưng với LLM, việc declassify (giảm cấp bảo mật) cần đánh giá ngữ nghĩa. 

Sự bảo vệ của FIDES phải vượt qua rào cản luồng ẩn (implicit flows) xảy ra khi LLM có thể phân tích hành vi "bóng" (shadow behaviors) thông qua nhiều token sinh ra. Hệ số rò rỉ rủi ro có thể được hình thức hóa:
$$
Leakage = H(Secret) - H(Secret \mid Observables) \qquad (1)
$$
Việc áp dụng lý thuyết Information Flow Control trong LLM như FIDES mở ra một hướng giải quyết mang tính nền tảng hơn hẳn so với phòng thủ qua rule-based mỏng manh.
