# Thực Nghiệm Benchmark và Chi Phí Vận Hành của Progent

> **Thông Tin Tài Liệu / Document Metadata:**
> - **Tác giả / Học viên:** Võ Phạm Tuấn Dũng (MSHV: 2570015) — `@tuandung222`
> - **Cán bộ Hướng dẫn:** TS. Lê Xuân Bách
> - **Chuyên ngành:** Thạc sĩ Khoa học Dữ liệu & Trí tuệ Nhân tạo Ứng dụng (CDNC AI - Khóa 261)
> - **Đơn vị:** Khoa Khoa học & Kỹ thuật Máy tính, Trường ĐH Bách Khoa, ĐHQG-HCM (HCMUT)
> - **Đề tài:** TrustSight (*An Empirical Study of Anchored Visual Perception for Prompt-Injection-Resistant Multimodal Agents*)
> - **Kho Lưu Trữ:** `tuandung222/software-security-agent-defenses`
> - **Ngày giờ biên soạn / Cập nhật (Timestamp):** 2026-10-06 01:45:00 (GMT+7)

## 1. Mở Đầu
Bài viết này tóm tắt kết quả đánh giá Progent trên các tập dữ liệu tiêu chuẩn và phân tích chi phí vận hành (overhead) khi triển khai vào môi trường thực tế (production).

## 2. Kết Quả Trên AgentBench và AgentDojo
Thực nghiệm cho thấy Progent giảm thiểu tỉ lệ thực thi thành công của kẻ tấn công (Attack Success Rate) gần bằng 0 mà chỉ ảnh hưởng tối thiểu đến hiệu năng của Agent trên các benchmark tiêu chuẩn.

## 3. So Sánh Độ Trễ
So với việc dùng LLM-based filtering, Progent sử dụng cơ chế kiểm tra cục bộ, đem lại độ trễ thấp hơn nhiều. 
Độ trễ được tính bằng công thức:
$$
\Delta T = T_{local\_verify} - T_{llm\_verify} \ll 0
\qquad (1)
$$
Điều này thể hiện sự tối ưu về chi phí token và tốc độ xử lý khi áp dụng các ràng buộc vào cấp độ runtime.

## 4. Tính Khả Thi Trong Production
Với overhead thấp, khả năng cấp quyền tinh chỉnh, việc tích hợp Progent trong các hệ thống doanh nghiệp là hoàn toàn khả thi và cần thiết để bảo vệ khỏi các rủi ro leo thang đặc quyền.
