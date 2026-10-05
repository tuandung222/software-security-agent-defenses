<Tổng quan: Task Decomposition & Security Design Patterns>
> **Thông Tin Tài Liệu / Document Metadata:**
> - **Tác giả / Học viên:** Võ Phạm Tuấn Dũng (MSHV: 2570015) — `@tuandung222`
> - **Cán bộ Hướng dẫn:** TS. Lê Xuân Bách
> - **Chuyên ngành:** Thạc sĩ Khoa học Dữ liệu & Trí tuệ Nhân tạo Ứng dụng (CDNC AI - Khóa 261)
> - **Đơn vị:** Khoa Khoa học & Kỹ thuật Máy tính, Trường ĐH Bách Khoa, ĐHQG-HCM (HCMUT)
> - **Đề tài:** TrustSight (*An Empirical Study of Anchored Visual Perception for Prompt-Injection-Resistant Multimodal Agents*)
> - **Kho Lưu Trữ:** `tuandung222/software-security-agent-defenses`
> - **Ngày giờ biên soạn / Cập nhật (Timestamp):** 2026-10-06 01:45:00 (GMT+7)

# Tổng quan về Kiến trúc Phòng vệ và Mẫu Thiết kế An ninh cho AI Agents

Tài liệu này cung cấp mục lục và cái nhìn tổng quan về các phương pháp phòng thủ chủ động (proactive defense) trong hệ thống AI Agent thông qua phân rã nhiệm vụ (Task Decomposition) và áp dụng các mẫu thiết kế an ninh (Security Design Patterns).

## 1. Danh sách Tài liệu

1. [01_task_shield_decomposition.md](01_task_shield_decomposition.md): Phân tích sâu kiến trúc Task Shield (ACL 2025).
2. [02_eth_zurich_agent_design_patterns.md](02_eth_zurich_agent_design_patterns.md): Tổng hợp các Security Design Patterns từ ETH Zurich.
3. [03_privilege_separation_and_least_authority.md](03_privilege_separation_and_least_authority.md): Áp dụng nguyên lý Phân tách Đặc quyền (Privilege Separation) và POLA (Principle of Least Authority).
4. [04_tong_hop_anti_patterns_trong_agent.md](04_tong_hop_anti_patterns_trong_agent.md): Tổng hợp các Anti-Patterns an ninh phổ biến trong triển khai AI Agent.

## 2. Ý tưởng Cốt lõi

Thay vì cố gắng bảo vệ bằng cách phân tích và lọc prompt ở đầu vào (Guardrails), hướng đi mới nhấn mạnh vào:
- **Kiến trúc hệ thống (System Architecture):** Tách biệt quyền hạn, giới hạn ngữ cảnh.
- **Phân rã (Decomposition):** Chia nhỏ luồng xử lý để LLM không phải nhận toàn bộ dữ liệu ở một bước duy nhất, giảm thiểu rủi ro Prompt Injection lan truyền.

$$
R_{system} = R_{planner} \times P(exploit | context)
$$ \qquad (1)
