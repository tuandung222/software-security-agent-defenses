# Thực nghiệm Conseca trên AgentDojo và Giới hạn Hệ thống

> **Thông Tin Tài Liệu / Document Metadata:**
> - **Tác giả / Học viên:** Võ Phạm Tuấn Dũng (MSHV: 2570015) — `@tuandung222`
> - **Cán bộ Hướng dẫn:** TS. Lê Xuân Bách
> - **Chuyên ngành:** Thạc sĩ Khoa học Dữ liệu & Trí tuệ Nhân tạo Ứng dụng (CDNC AI - Khóa 261)
> - **Đơn vị:** Khoa Khoa học & Kỹ thuật Máy tính, Trường ĐH Bách Khoa, ĐHQG-HCM (HCMUT)
> - **Đề tài:** TrustSight (*An Empirical Study of Anchored Visual Perception for Prompt-Injection-Resistant Multimodal Agents*)
> - **Kho Lưu Trữ:** `tuandung222/software-security-agent-defenses`
> - **Ngày giờ biên soạn / Cập nhật (Timestamp):** 2026-10-06 01:45:00 (GMT+7)

## 1. Môi trường Đánh giá AgentDojo

Nghiên cứu Conseca đã được thực nghiệm chi tiết trên **AgentDojo**, một nền tảng giả lập tác tử chuyên biệt để đánh giá bảo mật thông qua tấn công Prompt Injection và các lỗ hổng của LLM. Môi trường kiểm thử chứa nhiều thao tác mô phỏng hệ thống tài chính, email, quản lý file, nhằm đo lường khả năng phòng ngự trước các hành động độc hại.

### Hai chỉ số chính (Metrics):
1. **ASR (Attack Success Rate)**: Tỉ lệ tấn công thành công. Conseca đã hạ thấp đáng kể chỉ số này nhờ cơ chế Sandbox và Policy Gatekeeper.
2. **Utility (Độ khả dụng)**: Khả năng của Agent trong việc hoàn thành đúng nhiệm vụ được giao trên dữ liệu sạch. Quá trình kiểm tra đôi khi gây ra False Positives khiến mức độ khả dụng giảm nhẹ.

## 2. Chi phí Trễ và Token (Latency & Token Costs)

Quá trình chuyển đổi từ JSON action sang cấu trúc Python AST đòi hỏi chi phí lớn về Token sinh ra từ LLM. Do mã Python thường chứa nhiều kí tự hơn và đòi hỏi tư duy logic phức tạp, thời gian chờ (Latency) của mỗi bước thực thi cũng tăng lên đáng kể.

Gọi $C_{base}$ là chi phí của hệ thống chuẩn (Base Agent) và $C_{conseca}$ là chi phí khi dùng Conseca, ta có:

$$
C_{conseca} = C_{LLM\_synthesis} + C_{AST\_parsing} + C_{Dry\_run} \gg C_{base}
$$ \qquad (1)

## 3. Những Ranh Giới (Limitations)

Mặc dù mạnh mẽ, giải pháp Conseca có những nhược điểm cố hữu:
- **Biểu diễn Policy phức tạp**: Nhiều chính sách bảo mật thực tế không thể dễ dàng viết thành một hàm kiểm tra đơn giản bằng mã Python.
- **Side-effect lẩn tránh (Evasion)**: Các mã tinh vi có thể cố gắng lẩn tránh bộ Dynamic Taint Tracking nếu hệ thống theo dõi thông tin (Information Flow Control) không đủ chặt chẽ (ví dụ qua Timing Attacks).
- Không có tích hợp sẵn với các hệ thống xác thực cấp doanh nghiệp như SAP hay Oracle. 

Giải pháp cần một bộ nguyên lý cân bằng giữa tính bảo mật, hiệu suất (Latency), và sự chính xác (Utility) để ứng dụng rộng rãi.
