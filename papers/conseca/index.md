# Tổng quan Giải pháp Conseca: Dynamic Code Synthesis & Sandbox Evaluation

> **Thông Tin Tài Liệu / Document Metadata:**
> - **Tác giả / Học viên:** Võ Phạm Tuấn Dũng (MSHV: 2570015) — `@tuandung222`
> - **Cán bộ Hướng dẫn:** TS. Lê Xuân Bách
> - **Chuyên ngành:** Thạc sĩ Khoa học Dữ liệu & Trí tuệ Nhân tạo Ứng dụng (CDNC AI - Khóa 261)
> - **Đơn vị:** Khoa Khoa học & Kỹ thuật Máy tính, Trường ĐH Bách Khoa, ĐHQG-HCM (HCMUT)
> - **Đề tài:** TrustSight (*An Empirical Study of Anchored Visual Perception for Prompt-Injection-Resistant Multimodal Agents*)
> - **Kho Lưu Trữ:** `tuandung222/software-security-agent-defenses`
> - **Ngày giờ biên soạn / Cập nhật (Timestamp):** 2026-10-06 01:45:00 (GMT+7)

## 1. Giới thiệu Conseca

Conseca (Google, arXiv:2501.17070) là một cơ chế phòng thủ đột phá hướng đến việc kiểm soát hành vi của các tác tử ngôn ngữ lớn (LLM Agents) bằng cách ứng dụng **Dynamic Code Synthesis** và **Sandbox Evaluation**. Thay vì thực thi trực tiếp các hành động dạng chuỗi (textual actions) hoặc JSON sinh ra từ LLM - vốn tiềm ẩn nhiều rủi ro về độ tin cậy và an toàn - Conseca biến đổi các quy tắc (policy) và ý định của tác tử thành mã nguồn Python thực thi được.

Mục tiêu chính: Kiểm soát luồng dữ liệu (Dataflow) và sự thay đổi trạng thái (State mutation) trước khi cho phép tác tử thực sự gọi (commit) các API nguy hiểm ra môi trường thật.

## 2. Ý tưởng Cốt lõi

Tư tưởng cốt lõi của Conseca là:
1. Không tin tưởng vào chuỗi văn bản (JSON/Text) xuất ra bởi LLM.
2. Dịch các hành động và policy bảo mật thành một **Policy Program**.
3. Thực thi đoạn mã này trong một môi trường cô lập (Sandbox).
4. Quan sát các side-effects và State Mutations; nếu vi phạm chính sách bảo mật, lập tức từ chối hoặc chặn hành động đó.

Bằng việc yêu cầu Agent tổng hợp (synthesize) mã Python thể hiện logic của hành động, hệ thống có thể:
- Xác minh bằng các công cụ phân tích tĩnh (Static Analysis như AST parsing).
- Kiểm tra kết quả qua quá trình chạy thử (Dry-run).

## 3. Nội dung trong chuỗi tài liệu

Thư mục này gồm các phần đi sâu vào kỹ thuật Conseca:
- [01_kien_truc_dynamic_code_generation.md](01_kien_truc_dynamic_code_generation.md): Chi tiết kiến trúc sinh mã động (Dynamic Code Generation).
- [02_synthesizer_va_parser_logic.md](02_synthesizer_va_parser_logic.md): Cơ chế Synthesizer và Parser.
- [03_sandbox_evaluator_va_state_check.md](03_sandbox_evaluator_va_state_check.md): Bộ đánh giá Sandbox (Dry-run Evaluation) và kiểm tra trạng thái.
- [04_thuc_nghiem_agentdojo_va_ranh_gioi.md](04_thuc_nghiem_agentdojo_va_ranh_gioi.md): Đánh giá thực nghiệm và phân tích giới hạn của hệ thống.
