# Cơ chế Synthesizer và Parser Logic trong Conseca

> **Thông Tin Tài Liệu / Document Metadata:**
> - **Tác giả / Học viên:** Võ Phạm Tuấn Dũng (MSHV: 2570015) — `@tuandung222`
> - **Cán bộ Hướng dẫn:** TS. Lê Xuân Bách
> - **Chuyên ngành:** Thạc sĩ Khoa học Dữ liệu & Trí tuệ Nhân tạo Ứng dụng (CDNC AI - Khóa 261)
> - **Đơn vị:** Khoa Khoa học & Kỹ thuật Máy tính, Trường ĐH Bách Khoa, ĐHQG-HCM (HCMUT)
> - **Đề tài:** TrustSight (*An Empirical Study of Anchored Visual Perception for Prompt-Injection-Resistant Multimodal Agents*)
> - **Kho Lưu Trữ:** `tuandung222/software-security-agent-defenses`
> - **Ngày giờ biên soạn / Cập nhật (Timestamp):** 2026-10-06 01:45:00 (GMT+7)

## 1. Sandbox AST Parser / Code Validator

Bản chất của các mã Python do LLM sinh ra là không đáng tin. Do đó, Conseca sử dụng một **Sandbox AST Parser** (Bộ Phân Tích Cây Cú Pháp Trừu Tượng) để đảm bảo an toàn. Trọng tâm của bộ Parser này là loại bỏ toàn bộ các nút AST độc hại.

### Danh sách kiểm soát an toàn (Whitelist Enforcement)
Hệ thống chỉ cho phép một số giới hạn các lệnh và hàm nội tích (built-in functions).
Mọi thư viện như `os`, `sys`, `subprocess` bị cấm tuyệt đối. Parser sẽ đi qua cây AST và kiểm tra từng nút (NodeVisitor trong Python). Nếu tìm thấy một `Call` node gọi một hàm ngoài tập hợp whitelist, toàn bộ chương trình sẽ bị huỷ bỏ (Rejected).

## 2. Giải mã các Ràng buộc Bảo mật

Một Policy trong Conseca không phải là một chuỗi ngôn ngữ tự nhiên đơn thuần mà được cụ thể hoá thành các điều kiện mã:
- **Pre-conditions (Tiền điều kiện):** Ràng buộc kiểm tra trước khi thực thi hàm.
- **Invariants (Bất biến):** Điều kiện phải luôn đúng trong suốt quá trình chạy (ví dụ như không được sửa thuộc tính của root).
- **Post-conditions (Hậu điều kiện):** Điều kiện đảm bảo sau khi hàm chạy xong (ví dụ dữ liệu đầu ra không chứa mã nhạy cảm).

## 3. Công thức Tĩnh học (Static Validation)

Quá trình kiểm tra cú pháp có thể được mô phỏng bằng biểu thức logic về các tập hàm $F_{allowed}$ và $F_{used}$:

$$
Validation(AST) = 
\begin{cases} 
True & \text{nếu } \forall f \in F_{used} \subseteq F_{allowed} \text{ và } \text{Không có Side-effect độc hại} \\ 
False & \text{ngược lại} 
\end{cases}
$$ \qquad (1)

Sự kết hợp giữa LLM (tạo sinh linh hoạt) và AST Parser (kiểm tra chặt chẽ) tạo nên một lá chắn vững chắc trước các nỗ lực tấn công Prompt Injection nhằm thay đổi hành vi (ví dụ: bắt Agent gọi hàm `delete_all_files()`).
