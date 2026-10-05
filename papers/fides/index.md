# Tổng quan FIDES & Information Flow Control (IFC) trong hệ thống AI Agent
> **Thông Tin Tài Liệu / Document Metadata:**
> - **Tác giả / Học viên:** Võ Phạm Tuấn Dũng (MSHV: 2570015) — `@tuandung222`
> - **Cán bộ Hướng dẫn:** TS. Lê Xuân Bách
> - **Chuyên ngành:** Thạc sĩ Khoa học Dữ liệu & Trí tuệ Nhân tạo Ứng dụng (CDNC AI - Khóa 261)
> - **Đơn vị:** Khoa Khoa học & Kỹ thuật Máy tính, Trường ĐH Bách Khoa, ĐHQG-HCM (HCMUT)
> - **Đề tài:** TrustSight (*An Empirical Study of Anchored Visual Perception for Prompt-Injection-Resistant Multimodal Agents*)
> - **Kho Lưu Trữ:** `tuandung222/software-security-agent-defenses`
> - **Ngày giờ biên soạn / Cập nhật (Timestamp):** 2026-10-06 01:45:00 (GMT+7)

## 1. Giới thiệu FIDES và IFC

Trong bối cảnh các hệ thống AI Agent ngày càng tinh vi, nguy cơ rò rỉ dữ liệu nhạy cảm hoặc bị thao túng qua Prompt Injection là một thách thức lớn. FIDES là một kiến trúc đề xuất áp dụng Information Flow Control (IFC) cho các tác vụ của LLM nhằm bảo vệ hệ thống khỏi các rủi ro này. IFC cho phép kiểm soát luồng dữ liệu thay vì chỉ kiểm soát quyền truy cập tại biên hệ thống (như Access Control).

Bằng cách theo dõi cách thức dữ liệu di chuyển từ các nguồn không đáng tin cậy hoặc nhạy cảm đến các đích (sinks) đặc quyền, hệ thống có thể chặn đứng những luồng bất hợp lệ ngay cả khi LLM Agent sinh ra các chuỗi hành động không xác định.

## 2. Vì sao IFC là mô hình chuẩn tắc nhất?

Thay vì phụ thuộc vào các heuristics chặn từ khóa hay phân loại câu lệnh có nhiều kẽ hở, IFC dựa trên nền tảng toán học bảo mật chắc chắn, kiểm soát luồng dữ liệu (Information Flow) dựa trên nhãn (labels).

- **Kiểm soát rò rỉ dữ liệu:** Đảm bảo dữ liệu được gắn nhãn bảo mật cao (High) không thể chảy vào các kênh đầu ra (Low sinks).
- **Chống Prompt Injection:** Dữ liệu từ các kênh không đáng tin cậy (Tainted) không thể ảnh hưởng đến luồng điều khiển của các hành động đặc quyền.

## 3. Cấu trúc Tài liệu

Trong thư mục này, chúng ta sẽ đi sâu vào các khía cạnh khác nhau của IFC đối với AI Agents:
- [Mô hình bảo mật Lattice và các nhãn bảo mật](01_information_flow_control_trong_llm.md)
- [Cơ sở lý thuyết hình thức từ The LLMbda Calculus và Non-Interference](02_llmbda_calculus_va_non_interference.md)
- [Cơ chế Dynamic Taint Tracking tại runtime](03_dynamic_taint_tracking_tai_runtime.md)
- [So sánh FIDES với các hệ thống IFC kinh điển](04_so_sanh_voi_classic_ifc_he_thong.md)
