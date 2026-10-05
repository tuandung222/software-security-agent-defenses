# The LLMbda Calculus và Cơ chế Non-Interference
> **Thông Tin Tài Liệu / Document Metadata:**
> - **Tác giả / Học viên:** Võ Phạm Tuấn Dũng (MSHV: 2570015) — `@tuandung222`
> - **Cán bộ Hướng dẫn:** TS. Lê Xuân Bách
> - **Chuyên ngành:** Thạc sĩ Khoa học Dữ liệu & Trí tuệ Nhân tạo Ứng dụng (CDNC AI - Khóa 261)
> - **Đơn vị:** Khoa Khoa học & Kỹ thuật Máy tính, Trường ĐH Bách Khoa, ĐHQG-HCM (HCMUT)
> - **Đề tài:** TrustSight (*An Empirical Study of Anchored Visual Perception for Prompt-Injection-Resistant Multimodal Agents*)
> - **Kho Lưu Trữ:** `tuandung222/software-security-agent-defenses`
> - **Ngày giờ biên soạn / Cập nhật (Timestamp):** 2026-10-06 01:45:00 (GMT+7)

## 1. Cơ sở lý thuyết từ The LLMbda Calculus

LLMbda Calculus đề xuất một phép tính lambda (lambda calculus) được điều chỉnh, mô hình hóa sự tương tác giữa các LLM (các probabilistic functions) và môi trường. Nó định nghĩa rõ ràng cách ngữ cảnh prompt biến đổi (context updates) và ảnh hưởng của các thành phần trong prompt lên đầu ra của chuỗi suy luận.

Trong mô hình này, việc đánh giá trạng thái được biểu diễn thông qua hàm xác suất kết hợp ngữ nghĩa của agent:
$$
P(a \mid c, p) \qquad (1)
$$
Trong đó, hành động $a$ phụ thuộc vào ngữ cảnh $c$ và prompt $p$.

## 2. Non-Interference giữa Untrusted Data và Privileged Sinks

Một tính chất bảo mật chính yếu được chứng minh trong LLMbda là **Non-Interference** (không giao thoa). Nó quy định rằng: nếu dữ liệu nhạy cảm không ảnh hưởng đến phần hiển thị cho môi trường (low output), thì không có rò rỉ dữ liệu nào xảy ra.

Tương tự đối với tính toàn vẹn, các dữ liệu bị taint (từ untrusted sources) sẽ tuân theo Non-Interference nếu và chỉ nếu nó không làm thay đổi các kết quả quan trọng tại privileged sinks.

Định lý Non-Interference:
$$
\forall \text{ state } S_1, S_2: (S_1 \approx_L S_2) \implies (\text{execute}(S_1) \approx_L \text{execute}(S_2)) \qquad (2)
$$

## 3. Điều kiện đảm bảo an toàn toán học

LLMbda Calculus thiết lập các rules đánh giá (evaluation rules) sao cho mọi hành vi sinh chuỗi (token generation) hay sử dụng công cụ (tool uses) đều phải thỏa mãn bộ lọc (filter) tại runtime. Nếu hệ thống LLM vi phạm điều kiện luồng dữ liệu an toàn, hành vi sẽ bị đình chỉ, qua đó đảm bảo các thuộc tính Non-Interference trong suốt vòng đời của Agent.
