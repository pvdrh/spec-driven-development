# Hướng dẫn Phát triển Tính năng mới với Spec-Driven Development (SDD)

## 1. Sáu nguyên tắc vận hành cốt lõi

Để làm việc hiệu quả, an toàn với AI theo SDD, cần tuân thủ 6 nguyên tắc sau:

1. **Requirements-Driven (Yêu cầu dẫn dắt):** Spec phải dẫn dắt code, khi một khái niệm mới xuất hiện, nó phải được cập nhật vào spec trước hoặc ít nhất cùng lúc với code.

2. **AI-Assisted (AI hỗ trợ, con người quyết định):** AI gánh vác phần việc lặp lại (CRUD, sinh test, boilerplate), nhưng con người (Dev, PO) phải là người quyết định các luật nghiệp vụ, pháp lý và tiền bạc.

3. **Iterative Improvement (Cải tiến liên tục):** Không cần viết spec hoàn hảo từ ngày đầu. Quy trình nên linh hoạt, lặp lại qua các vòng feedback để học và cập nhật thêm vào spec.

4. **Test-Protected (Bảo vệ bằng bộ kiểm thử):** Các bài test phải bám sát Acceptance Criteria từ spec để tạo "hàng rào" bảo vệ, cho phép AI regenerate (viết lại) code mà không sợ lệch hành vi cũ.

5. **Stakeholder-Centric (Lấy người liên quan làm trung tâm):** Spec phải được đại diện nghiệp vụ (PO/BA) đọc và duyệt để phát hiện các quy tắc "đương nhiên" mà dev hay AI không biết.

6. **Traceable (Tính truy xuất):** Phải duy trì sợi dây liên kết 3 chiều: $Business Requirement \leftrightarrow Use Case \leftrightarrow Code/Test$ để nhanh chóng truy vết mỗi khi có lỗi.

## 2. Bốn tầng cấu trúc của Specification

Yêu cầu hệ thống trong SDD không được gộp chung mà chia thành 4 tầng rõ ràng:

- **Tầng 1: Business Requirement (BR):** Trả lời câu hỏi "Vì sao làm?". Chứa nội dung đo lường được bao gồm Goal (Mục tiêu), Success Metrics (Thước đo) và Out of Scope (Phần không làm).

- **Tầng 2: Use Case:** Trả lời "Ai làm gì với hệ thống?". Nó mô tả các kịch bản tương tác gồm: Actor (Tác nhân), Trigger (Hành động kích hoạt), Main Flow (Luồng chính), Alternative Flows (Biến thể) và Exceptions (Ngoại lệ).

- **Tầng 3: Entity Model (Mô hình thực thể):** Trả lời "Hệ thống đang nói về những danh từ nào?". Nó định nghĩa rõ tên gọi, trạng thái và quan hệ giữa các khái niệm nghiệp vụ để AI không tự đoán mò.

- **Tầng 4: Acceptance Criteria (AC - Tiêu chí chấp nhận):** Trả lời "Làm sao biết là xong đúng?". Đây là cầu nối giữa spec và bộ kiểm thử (test).

## 3. Các bước phát triển tính năng mới (Greenfield)

### Bước 1: Khởi tạo Business Requirement (BR)

**Mô tả:** PO/BA cùng team ngồi lại để thống nhất mục tiêu, đo lường và đặc biệt là làm rõ giới hạn của tính năng trong lần ra mắt đầu tiên (MVP).

**Ví dụ (Tính năng Xin nghỉ phép):**

- **Goal:** Giảm thời gian xử lý phép cuối tháng của HR xuống dưới 2 giờ.
- **In Scope (MVP):** Xin nghỉ phép năm, nghỉ ốm, báo cáo xuất CSV.
- **Out of Scope:** Chưa làm duyệt đa cấp, chưa làm mobile app.

### Bước 2: Thiết kế Use Case và Entity Model

**Mô tả:** Chuyển đổi BR thành danh sách các kịch bản sử dụng (Use Case) và làm rõ từ vựng nghiệp vụ.

**Ví dụ:**

- **Use Cases:** `UC-001 (Đăng nhập)`, `UC-004 (Xin nghỉ phép)`.
- **Entity Model:** Thực thể `LeaveRequest` chứa các trường `employee_id`, `type`, `from_date`, `to_date`, `status`.

### Bước 3: Viết Spec chi tiết & Chuẩn hóa Acceptance Criteria (AC)

**Mô tả:** Chọn một Use Case quan trọng nhất làm chuẩn, sau đó định nghĩa rõ các luồng và tiêu chí nghiệm thu.

**Chuẩn hóa AC:** Viết theo cấu trúc `Given` (Giả định bối cảnh) - `When` (Khi có hành động) - `Then` (Thì kết quả là) - `And` (Và các hệ quả phụ).

**Ví dụ thực tế (Cho UC-004: Xin nghỉ phép):**

- **AC-1: Submit yêu cầu hợp lệ**
  - **Given:** Nhân viên có 8 ngày phép, không có yêu cầu nào đang chờ duyệt.
  - **When:** Nhân viên nộp đơn xin nghỉ phép năm từ 2025-03-10 đến 2025-03-12.
  - **Then:** Hệ thống tạo một bản ghi LeaveRequest ở trạng thái "pending".
  - **And:** Gửi email tới quản lý, và số dư ngày phép KHÔNG bị trừ (chỉ trừ khi duyệt).

- **AC-2: Xử lý ngoại lệ vượt số ngày phép**
  - **Given:** Nhân viên chỉ còn 2 ngày phép.
  - **When:** Nhân viên nộp yêu cầu nghỉ 5 ngày.
  - **Then:** Hệ thống từ chối yêu cầu.
  - **And:** Hiển thị thông báo lỗi "Số ngày phép còn lại: 2".

### Bước 4: Lập kế hoạch và AI sinh Code (Plan & Implement)

**Mô tả:** Đưa Spec cho AI agent (như Claude Code, Cursor, Spec Kit) yêu cầu lên plan và phân tách task (tạo route, sinh API, tạo giao diện, viết test).

**Nguyên tắc vận hành:** Dev dẫn dắt AI đi qua từng task nhỏ. Nếu AI đưa ra quyết định nghiệp vụ sai hoặc thiếu sót, dev phải chỉnh sửa từ cấp độ Spec rồi mới cho AI code tiếp thay vì tự sửa tay vào code. Cấu trúc tên của Test case cần chứa chính xác ID của UC và AC tương ứng.

### Bước 5: Triển khai kiểm thử, Demo và vòng lặp phản hồi

**Mô tả:** Kiểm tra độ bao phủ của bộ test xem tất cả các AC đã được pass hay chưa. Sau đó deploy bản staging để đại diện nghiệp vụ dùng thử (UAT).

**Xử lý phản hồi:** Nếu người dùng phát hiện kịch bản thiếu (ví dụ hệ thống chưa trừ đi ngày nghỉ lễ Tết), không vá code ngay. Ghi nhận lỗi đó vào mục *History* của file Spec hiện tại hoặc chuyển sang BR phiên bản tiếp theo, sau đó mới regenerate lại bộ code và test mới.

## 4. Tổ chức cấu trúc thư mục tiêu chuẩn

### Cấu trúc tiêu chuẩn (Ideal Structure):
```
project-root/
├── /specs                          ← Nơi lưu giữ sự thật về nghiệp vụ (source of truth)
│   ├── /business-requirements      ← Chứa các file BR (VD: BR-001-hr-tool.md)
│   ├── /use-cases                  ← Phân chia Use Case theo Bounded Context
│   │   ├── /checkout               ← Ngữ cảnh thanh toán (VD: UC-042.md)
│   │   └── /shipping               ← Ngữ cảnh giao hàng
│   ├── /entities                   ← Mô hình thực thể chia theo ngữ cảnh
│   │   ├── checkout-context.md
│   │   └── shipping-context.md
│   ├── /diagrams                   ← Sơ đồ luồng, kiến trúc
│   └── README.md
```

### Ví dụ từ Auth Service (Source Code thực tế):
```
auth_service/
├── /docs                           ← Nơi lưu giữ sự thật về nghiệp vụ (source of truth)
│   ├── Business-Requirement.md     ← 📄 [Tầng 1: BR](./Business-Requirement.md)
│   ├── Use-Case.md                 ← 📄 [Tầng 2: Use Case](./Use-Case.md)
│   ├── Entity-Model.md             ← 📄 [Tầng 3: Entity Model](./Entity-Model.md)
│   ├── Acceptance-Criteria.md      ← 📄 [Tầng 4: Acceptance Criteria](./Acceptance-Criteria.md)
│   ├── Spec-Driven-Development.md  ← 📄 Hướng dẫn này (6 nguyên tắc & 4 tầng cấu trúc)
│   └── README.md                   ← Chỉ dẫn nhanh và điều hướng
```

**Hướng dẫn sử dụng từng file:**

| File | Mục đích | Bắt đầu từ đâu? |
|------|---------|-----------------|
| [Business-Requirement.md](./Business-Requirement.md) | Định nghĩa Goal, Success Metrics, Out of Scope | Đầu tiên, cùng PO/BA thống nhất mục tiêu |
| [Use-Case.md](./Use-Case.md) | Mô tả kịch bản: Actor, Trigger, Main Flow, Exceptions | Sau khi BR được duyệt |
| [Entity-Model.md](./Entity-Model.md) | Làm rõ khái niệm, trạng thái, quan hệ giữa entities | Song song với Use Case |
| [Acceptance-Criteria.md](./Acceptance-Criteria.md) | Viết Given-When-Then cho từng Use Case | Cuối cùng, trước khi code |