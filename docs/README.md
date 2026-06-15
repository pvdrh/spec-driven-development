# Spec-Driven Development (SDD) - Auth Service

Chào mừng! Đây là hướng dẫn phát triển tính năng theo phương pháp **Spec-Driven Development** cho dự án.

## 📚 Cây tài liệu

Các file được tổ chức theo 4 tầng của Specification:

```
docs/
├── 1️⃣  Business-Requirement.md      ← "Vì sao làm?" - Mục tiêu, đo lường
├── 2️⃣  Use-Case.md                  ← "Ai làm gì?" - Kịch bản sử dụng
├── 3️⃣  Entity-Model.md              ← "Hệ thống nói về danh từ nào?" - Khái niệm
├── 4️⃣  Acceptance-Criteria.md       ← "Làm sao biết xong?" - Tiêu chí chấp nhận
├── 📖 Spec-Driven-Development.md    ← Hướng dẫn chi tiết (6 nguyên tắc, quy trình)
└── 📋 README.md                      ← File này
```

## 🚀 Bắt đầu nhanh

Tuân theo quy trình này khi phát triển tính năng mới:

### Bước 1: Định nghĩa yêu cầu kinh doanh
👉 Mở [Business-Requirement.md](./Business-Requirement.md)
- Thống nhất Goal (mục tiêu)
- Xác định Success Metrics (thước đo)
- Liệt kê Out of Scope (không làm)

### Bước 2: Thiết kế kịch bản sử dụng
👉 Mở [Use-Case.md](./Use-Case.md)
- Định nghĩa Actor (tác nhân)
- Mô tả Main Flow (luồng chính)
- Liệt kê Alternative Flows (biến thể)
- Xử lý Exceptions (ngoại lệ)

### Bước 3: Làm rõ khái niệm
👉 Mở [Entity-Model.md](./Entity-Model.md)
- Định nghĩa các thực thể (entities)
- Mô tả trạng thái (states)
- Vẽ quan hệ (relationships)

### Bước 4: Viết tiêu chí chấp nhận
👉 Mở [Acceptance-Criteria.md](./Acceptance-Criteria.md)
- Viết theo cấu trúc Given-When-Then
- Đảm bảo bao phủ toàn bộ Use Case
- Chuẩn bị cho việc viết test

### Bước 5: Phát triển & Kiểm thử
- Lên plan với AI (Claude Code, Cursor, v.v.)
- Viết code theo Spec
- Viết test bám sát Acceptance Criteria
- Chạy UAT với Stakeholder

## 🎯 6 Nguyên tắc vận hành

1. **Requirements-Driven** — Spec dẫn dắt code
2. **AI-Assisted** — AI hỗ trợ, con người quyết định
3. **Iterative Improvement** — Cải tiến liên tục
4. **Test-Protected** — Test bảo vệ behavior
5. **Stakeholder-Centric** — Lấy người liên quan làm trung tâm
6. **Traceable** — Duy trì sợi dây liên kết BR ↔ UC ↔ Code/Test

👉 Xem chi tiết tại [Spec-Driven-Development.md](./Spec-Driven-Development.md)

## 📋 Checklist khi phát triển tính năng

- [ ] Business Requirement được PO/BA duyệt
- [ ] Use Case được thiết kế đầy đủ
- [ ] Entity Model được làm rõ
- [ ] Acceptance Criteria được viết theo Given-When-Then
- [ ] Tất cả AC đã có test case tương ứng
- [ ] Code được phát triển theo Spec
- [ ] Tất cả test pass
- [ ] Stakeholder thực hiện UAT
- [ ] Feedback được ghi nhận vào "History" hoặc phiên bản tiếp theo

## 💡 Mẹo

- **Khi phát hiện vấn đề:** Không vá code ngay. Cập nhật Spec trước, sau đó regenerate code/test.
- **Test case naming:** Sử dụng cấu trúc `UC-XXX_AC-YY` để dễ truy vết (ví dụ: `UC-001_AC-01`).
- **History:** Mỗi file Spec có phần "History" để ghi nhận những phát hiện và lỗi từ UAT.

## 📞 Liên hệ

Nếu có câu hỏi, hãy tham khảo [Spec-Driven-Development.md](./Spec-Driven-Development.md) hoặc thảo luận với team.

---

**Last updated:** 2026-06-15
