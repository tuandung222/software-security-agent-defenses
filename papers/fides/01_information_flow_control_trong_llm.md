# Mô hình bảo mật Lattice và Information Flow Control trong LLM
> **Thông Tin Tài Liệu / Document Metadata:**
> - **Tác giả / Học viên:** Võ Phạm Tuấn Dũng (MSHV: 2570015) — `@tuandung222`
> - **Cán bộ Hướng dẫn:** TS. Lê Xuân Bách
> - **Chuyên ngành:** Thạc sĩ Khoa học Dữ liệu & Trí tuệ Nhân tạo Ứng dụng (CDNC AI - Khóa 261)
> - **Đơn vị:** Khoa Khoa học & Kỹ thuật Máy tính, Trường ĐH Bách Khoa, ĐHQG-HCM (HCMUT)
> - **Đề tài:** TrustSight (*An Empirical Study of Anchored Visual Perception for Prompt-Injection-Resistant Multimodal Agents*)
> - **Kho Lưu Trữ:** `tuandung222/software-security-agent-defenses`
> - **Ngày giờ biên soạn / Cập nhật (Timestamp):** 2026-10-06 01:45:00 (GMT+7)

## 1. Mô hình bảo mật Lattice ($L \sqsubseteq H$)

Lattice (Dàn) cung cấp cơ sở toán học để kiểm soát luồng thông tin (IFC) giữa các thành phần khác nhau của hệ thống. Mỗi thông tin, tiến trình hoặc đối tượng được gán một nhãn bảo mật. Một cấu trúc Lattice $(SC, \sqsubseteq)$ bao gồm tập hợp các lớp bảo mật (Security Classes - $SC$) và một quan hệ thứ tự bán phần (Partial Order - $\sqsubseteq$).

Ký hiệu cơ bản:
$$
L \sqsubseteq H \qquad (1)
$$
Điều này chỉ ra rằng thông tin có thể chảy từ mức $L$ (Low) lên $H$ (High), nhưng không thể ngược lại (luồng thông tin được bảo mật).

## 2. Các nhãn bảo mật (Security Labels)

Hệ thống IFC áp dụng hai dạng nhãn chính cho dữ liệu:
- **Confidentiality (Bảo mật):** Kiểm soát dữ liệu bí mật (ví dụ: thông tin người dùng nội bộ) không được rò rỉ ra public networks.
- **Integrity (Tính toàn vẹn):** Kiểm soát dữ liệu (ví dụ: từ internet, thư mục untrusted) không được phép ảnh hưởng đến dữ liệu toàn vẹn cao hoặc thao tác thực thi các file hệ thống quan trọng.

## 3. Nguyên lý No-Read-Up / No-Write-Down

Hai quy tắc kiểm soát truy cập bắt buộc từ các mô hình truyền thống (Bell-LaPadula và Biba) được ánh xạ vào IFC:

### Bell-LaPadula (Bảo vệ Confidentiality)
- **No-Read-Up:** Tiến trình ở mức thấp không được đọc dữ liệu mức cao.
- **No-Write-Down (*-property):** Tiến trình ở mức cao không được ghi đè/chia sẻ xuống vùng nhớ có mức bảo vệ thấp.

### Biba (Bảo vệ Integrity)
- **No-Read-Down:** Tiến trình (hay Agent) toàn vẹn cao không được lấy dữ liệu từ nguồn toàn vẹn thấp để đưa vào các thao tác điều khiển cốt lõi (chặn Prompt Injection).
- **No-Write-Up:** Tiến trình toàn vẹn thấp không được ghi dữ liệu thay đổi trạng thái của các thực thể toàn vẹn cao.

Trong LLM, mô hình Lattice IFC ngăn chặn những lời gọi hàm (Tool Calls) từ Agent khi nó đang bị thao túng (nhãn Integrity = Low) vào những sinks nguy hiểm như việc ghi file hay gọi API quản trị.
