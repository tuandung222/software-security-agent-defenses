# Hướng Dẫn & Chỉ Thị Bắt Buộc Dành Cho AI Agents (AGENTS.md)

> **Thông Tin Tài Liệu / Document Metadata:**
> - **Tác giả / Học viên:** Võ Phạm Tuấn Dũng (MSHV: 2570015) — `@tuandung222`
> - **Cán bộ Hướng dẫn:** TS. Lê Xuân Bách
> - **Chuyên ngành:** Thạc sĩ Khoa học Dữ liệu & Trí tuệ Nhân tạo Ứng dụng (CDNC AI - Khóa 261)
> - **Đơn vị:** Khoa Khoa học & Kỹ thuật Máy tính, Trường ĐH Bách Khoa, ĐHQG-HCM (HCMUT)
> - **Đề tài Luận văn:** TrustSight (*An Empirical Study of Anchored Visual Perception for Prompt-Injection-Resistant Multimodal Agents*)
> - **Kho lưu trữ chuyên khảo:** `tuandung222/software-security-agent-defenses`

---

## 1. Phạm Vi & Ranh Giới Nghiên Cứu (Scope & Boundaries)

- **Phạm vi trọng tâm:** Repository này chuyên khảo sâu về **Giải pháp An ninh Hệ thống & Kỹ nghệ Phần mềm (Software Engineering & Systems Security Defenses)** dành cho AI Agents / Multimodal Agents đối phó với Prompt Injection (trực tiếp, gián tiếp và Visual Prompt Injection - VPI).
- **Các hệ thống & công trình nghiên cứu cốt lõi:**
  1. **CaMeL** (Debenedetti et al., arXiv:2503.18813): Dual-Model Isolation, Quarantined Processing, Untrusted Data Confinement.
  2. **CaMeL-CUA / CaMeLs Can Use Computers Too** (Debenedetti et al., arXiv:2601.09923): Single-Shot Planning, Observe-Verify-Act, Computer-Use Agent Security trên OSWorld, Branch Steering Defense.
  3. **Conseca** (Tsai, Bagdasarian - Google, 2025, arXiv:2501.17070): Contextual Agent Security, Just-in-Time (JIT) Dynamic Policy Synthesis, Deterministic Policy Enforcer.
  4. **Progent** (Shi et al., 2025, arXiv:2504.11703): Programmable Privilege Control for LLM Agents, JSON Schema Capability Enforcement.
  5. **Task Shield** (Jia et al., ACL 2025, arXiv:2412.16682): Defending Multimodal Agents from Indirect Prompt Injections via Original User Goal Consistency.
  6. **Design Patterns for Securing LLM Agents against Prompt Injections** (Beurer-Kellner, Fischer, Vechev - ETH Zurich, 2025, arXiv:2506.08837): Plan-then-Execute, Action-Selector, Information-Flow Control (IFC).
  7. **FIDES** (Wyss et al., IEEE S&P / Oakland 2025, arXiv:2505.12781): Information-Flow Control (IFC), Dynamic Taint Tracking, Security Labels, Egress Confinement.
  8. **The LLMbda Calculus** (arXiv:2506.08837): Formal Foundations for AI Agents, Conversations, and Information Flow.
  9. **Nền tảng An ninh Hệ thống Cổ điển:** Anderson 1972 (Reference Monitor: Complete Mediation, Tamper-proof, Verifiable), Saltzer & Schroeder 1975 (Fail-Safe Defaults / Default-Deny, Economy of Mechanism), Hardy 1988 (Confused Deputy).
- **Ranh giới nghiêm ngặt:** Tuyệt đối không nhầm lẫn sang các phương pháp can thiệp trọng số mô hình (LoRA SFT, activation steering, pruning, quantization) vốn thuộc kho lưu trữ song hành `model-based-vpi-defenses`.

---

## 2. Quy Chuẩn Kỹ Thuật Bắt Buộc (Mandatory Technical Standards)

### 2.1. Cấm Tuyệt Đối `\tag{...}` Trong KaTeX
- Lệnh `\tag{...}` làm crash giao diện xem tài liệu trên VS Code, Notion, Obsidian với lỗi:
  ```text
  KaTeX parse error: \tag works only in display equations
  ```
- **Bắt buộc dùng `\qquad (1)` hoặc `\quad (1)`** bên trong khối công thức nhiều dòng `$$\n ... \qquad (1) \n$$`.

### 2.2. Quy Chuẩn Biểu Đồ Mermaid
- Tuyệt đối không dùng khoảng nửa mở `[0, n)` hoặc `[n, n+Delta_n)` trong nhãn node/subgraph. Thay bằng `(0 đến n)`.
- Luôn bọc nhãn có ký tự đặc biệt trong cặp dấu ngoặc kép: `NodeId["Nội dung..."]`.
- `direction` chỉ dùng trong `flowchart TD` / `flowchart LR`.

### 2.3. Liêm Chính Học Thuật (Zero AI Slop)
- Không bịa định lý toán học ảo (pseudo-theorems), không tự phong các khối `if/else` thành "Lattice Theorem" hay "d-separation Lemma".
- Gọi đúng bản chất kỹ thuật: JSON Schema Validator, SQLite Reference Lookup, Single-use Random Nonce Token, Authorization Gateway.
- Không dùng liên kết tuyệt đối `file:///`.

### 2.4. Kiểm Thẩm Cú Pháp Trước Khi Commit
- Luôn chạy:
  ```bash
  python3 scripts/check_markdown_katex.py
  ```
  Repo đã kích hoạt pre-commit hook tự động chặn vi phạm.
