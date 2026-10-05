# Phân Quyền Chi Tiết (Granular Privilege Delegation)

> **Thông Tin Tài Liệu / Document Metadata:**
> - **Tác giả / Học viên:** Võ Phạm Tuấn Dũng (MSHV: 2570015) — `@tuandung222`
> - **Cán bộ Hướng dẫn:** TS. Lê Xuân Bách
> - **Chuyên ngành:** Thạc sĩ Khoa học Dữ liệu & Trí tuệ Nhân tạo Ứng dụng (CDNC AI - Khóa 261)
> - **Đơn vị:** Khoa Khoa học & Kỹ thuật Máy tính, Trường ĐH Bách Khoa, ĐHQG-HCM (HCMUT)
> - **Đề tài:** TrustSight (*An Empirical Study of Anchored Visual Perception for Prompt-Injection-Resistant Multimodal Agents*)
> - **Kho Lưu Trữ:** `tuandung222/software-security-agent-defenses`
> - **Ngày giờ biên soạn / Cập nhật (Timestamp):** 2026-10-06 01:45:00 (GMT+7)

## 1. Mở Đầu
Phân quyền chi tiết (Granular Privilege Delegation) trong Progent không chỉ hạn chế loại công cụ mà agent có thể gọi, mà còn phân biệt rõ ràng giữa các hành động đọc và ghi.

## 2. Phân Biệt Read-only vs State-mutating Tools
- **Read-only**: Các công cụ như `cat`, `ls` không làm thay đổi trạng thái của hệ thống.
- **State-mutating**: Các công cụ như `rm`, `git commit` sẽ làm thay đổi trạng thái và yêu cầu quyền hạn cao hơn.

## 3. Single-use HMAC Nonce Token
Để cấp phát quyền tạm thời cho từng hành động thay đổi trạng thái, Progent tạo ra các Token ngẫu nhiên dùng 1 lần (Single-use HMAC nonce token).
Công thức tạo token:
$$
T_{HMAC} = HMAC(Key, Action\_ID || Timestamp || Nonce)
\qquad (1)
$$
Token này được kiểm chứng trước khi bất kỳ tác vụ đột biến nào được thực thi.
