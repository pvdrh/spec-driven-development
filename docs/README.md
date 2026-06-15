# 📚 docs/ — Index điều hướng

Thư mục này chứa **hướng dẫn SDD chi tiết** và **template Spec mẫu** (BR / UC / Entity / AC).

> 💡 Cần overview tổng quan về SDD? Xem [README.md ở root](../README.md).

## Cây tài liệu

```
docs/
├── README.md                       ← File này (index)
├── Spec-Driven-Development.md      ← 📖 Hướng dẫn chi tiết: 6 nguyên tắc + 4 tầng + quy trình
├── Business-Requirement.md         ← 1️⃣ Template Tầng 1 — BR
├── Use-Case.md                     ← 2️⃣ Template Tầng 2 — UC
├── Entity-Model.md                 ← 3️⃣ Template Tầng 3 — Entity Model
└── Acceptance-Criteria.md          ← 4️⃣ Template Tầng 4 — AC
```

## File nào dùng khi nào?

| File | Mục đích | Khi nào mở? |
|------|---------|-------------|
| [Spec-Driven-Development.md](./Spec-Driven-Development.md) | Hướng dẫn đầy đủ: 6 nguyên tắc, 4 tầng, quy trình Greenfield/Brownfield | Khi cần tra cứu lý thuyết hoặc onboard người mới |
| [Business-Requirement.md](./Business-Requirement.md) | Định nghĩa **Goal, Success Metrics, Out of Scope** | Đầu tiên — cùng PO/BA thống nhất mục tiêu |
| [Use-Case.md](./Use-Case.md) | Mô tả kịch bản: **Actor, Trigger, Main Flow, Exceptions** | Sau khi BR được duyệt |
| [Entity-Model.md](./Entity-Model.md) | Làm rõ **khái niệm, trạng thái, quan hệ** giữa entities | Song song với Use Case |
| [Acceptance-Criteria.md](./Acceptance-Criteria.md) | Viết **Given-When-Then** cho từng Use Case | Cuối cùng, trước khi code |

---

**Last updated:** 2026-06-15
