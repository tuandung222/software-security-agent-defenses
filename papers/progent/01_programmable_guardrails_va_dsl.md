# Thiết Kế Ngôn Ngữ Chính Sách (Policy DSL) trong Progent

> **Thông Tin Tài Liệu / Document Metadata:**
> - **Tác giả / Học viên:** Võ Phạm Tuấn Dũng (MSHV: 2570015) — `@tuandung222`
> - **Cán bộ Hướng dẫn:** TS. Lê Xuân Bách
> - **Chuyên ngành:** Thạc sĩ Khoa học Dữ liệu & Trí tuệ Nhân tạo Ứng dụng (CDNC AI - Khóa 261)
> - **Đơn vị:** Khoa Khoa học & Kỹ thuật Máy tính, Trường ĐH Bách Khoa, ĐHQG-HCM (HCMUT)
> - **Đề tài:** TrustSight (*An Empirical Study of Anchored Visual Perception for Prompt-Injection-Resistant Multimodal Agents*)
> - **Kho Lưu Trữ:** `tuandung222/software-security-agent-defenses`
> - **Ngày giờ biên soạn / Cập nhật (Timestamp):** 2026-10-06 01:45:00 (GMT+7)

## 1. Giới thiệu Policy DSL
Progent giới thiệu một ngôn ngữ chính sách (Policy DSL) cho phép các kỹ sư khai báo quyền sử dụng công cụ (tool) một cách an toàn.

## 2. Cú pháp Khai báo Quyền (Tool Permissions)
Thay vì cấp quyền thực thi shell tùy ý, chính sách giới hạn chặt chẽ:

```yaml
permissions:
  - tool: "read_file"
    args:
      path: "/var/log/*"
```

## 3. Ràng Buộc Tham Số (Parameter Constraints)
Ràng buộc tham số đảm bảo agent không thể lạm dụng công cụ bằng cách truyền vào các đối số độc hại. Cơ chế Sandbox AST Parser / Code Validator sẽ xác thực các đối số này.

Hàm kiểm tra hợp lệ được biểu diễn:
$$
V(args) = \prod_{k \in keys} \delta(args[k] \in allowed\_set)
\qquad (1)
$$

## 4. Context Conditions
Các điều kiện ngữ cảnh kiểm tra trạng thái trước khi cho phép thực thi, ngăn chặn việc gọi công cụ trong các điều kiện không an toàn.
