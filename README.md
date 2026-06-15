# Spec-Driven Development (SDD)

> *Kiến tạo cầu nối giữa ý định con người và năng lực thực thi của AI.*
> Khung tư duy & thực hành cho Engineering Team trong kỷ nguyên AI.

Dự án này là **project mẫu** minh họa cách áp dụng **Spec-Driven Development (SDD)** — phương pháp đặt **Spec làm trung tâm**, dùng Spec như **giao diện (interface)** giữa con người và AI/Code.

---

## 1. SDD là gì?

**Spec-Driven Development** chuyển đổi Spec từ một **tài liệu hành chính** (viết xong rồi cất đi) thành một **giao diện giao tiếp sống** giữa con người và AI.

```
   Con người  ◀──────▶  Specification  ◀──────▶  AI & Code
   (Ý định)              (Interface)             (Thực thi)
```

- **Thống nhất ý định:** Spec chốt lại những gì làm và làm vì sao.
- **Nguồn chân lý:** Khi bị lệch, AI bám vào Spec trước, lõi code sau.
- **Thực thi:** AI đóng vai trò sinh code, sinh test, tài liệu dựa trên Spec.

---

## 2. Tại sao cần SDD? — 3 cám dỗ của Vibe Coding

Pair-programming với AI cực kỳ nhanh, nhưng *“cảm giác nhanh”* dễ che giấu 3 vấn đề:

| # | Cám dỗ | Hậu quả |
|---|--------|---------|
| 1 | **Mất ngữ cảnh (Context Loss)** | Yêu cầu nằm rải rác trong chat, không ai nắm trọn bức tranh. |
| 2 | **Trí nhớ ngắn hạn (Short-term Memory)** | AI không nhớ lại quyết định đã chốt; team lặp đi lặp lại câu hỏi cũ. |
| 3 | **Cảm giác an toàn ảo (False Security)** | Code chạy được ≠ đúng nghiệp vụ. Không có chứng minh, không có lưới an toàn. |

### Ma trận đối chiếu: Tốc độ vs Sự bền vững

| Khía cạnh | Vibe Coding | Spec-Driven Development |
|-----------|-------------|--------------------------|
| Tốc độ Tuần 1 | Rất nhanh | Chậm hơn (đầu tư viết Spec) |
| Tốc độ Tháng 3 | Chậm dần do mất ngữ cảnh | Ổn định, ngữ cảnh được lưu trữ |
| Onboard người mới | Đọc code thì lâu | Đọc Spec, test, doc |
| Refactor Code | Sợ hãi, sợ phá vỡ rule ngầm | Tự tin, Test bảo vệ tiêu chí nghiệm thu (AC) |
| Vai trò của PO/BA | Người đặt hàng từ xa | Đồng tác giả của hệ thống |

---

## 3. Sáu Nguyên tắc Vận hành Cốt lõi

1. **Requirements-Driven** — Spec dẫn dắt Code. Không có Spec, không có Code.
2. **AI-Assisted** — AI làm việc lặp lại, con người quyết định nghiệp vụ.
3. **Iterative Improvement** — Spec không hoàn hảo ngày đầu; lặp qua các vòng feedback.
4. **Test-Protected** — Test bám sát AC, làm hàng rào để AI regenerate code an toàn.
5. **Stakeholder-Centric** — PO/BA đọc & duyệt Spec để bắt rule “đương nhiên”.
6. **Traceable** — Duy trì sợi dây: **Business Requirement ↔ Use Case ↔ Code/Test**.

👉 Chi tiết: [docs/Spec-Driven-Development.md](./docs/Spec-Driven-Development.md)

---

## 4. Bốn Tầng Cấu trúc của Specification

```
┌─────────────────────────────────────────────────┐
│ Tầng 4: Acceptance Criteria (AC)                │  Làm sao biết là xong đúng?
├─────────────────────────────────────────────────┤
│ Tầng 3: Entity Model & Ngôn ngữ chung           │  Hệ thống nói về danh từ nào?
├─────────────────────────────────────────────────┤
│ Tầng 2: Use Case (UC)                           │  Ai làm gì với hệ thống?
├─────────────────────────────────────────────────┤
│ Tầng 1: Business Requirement (BR)               │  Vì sao làm? (đo lường được)
└─────────────────────────────────────────────────┘
```

### Tầng 1 — Business Requirement (BR)

Mục tiêu kinh doanh phải **đo lường được**. Định nghĩa **In Scope** quan trọng nhất là để chặn AI suy diễn tràn lan.

```markdown
## BR-007: Tăng tỷ lệ checkout thành công

### Goal
Tăng conversion_rate khách mới từ 62% lên 88% trong Q3.

### Success Metric
Đo qua dashboard Analytics, segment `new_customer = true`.

### Out of Scope
- Khách cũ (sẽ làm ở BR-008)
- Thanh toán quốc tế
```

### Tầng 2 — Use Case (UC)

Mô tả kịch bản tương tác: **Actor → Trigger → Main Flow → Exceptions**.

```markdown
## UC-042: Đặt hàng bằng QR Code

### Actor
Khách hàng (đã login)

### Trigger
Khách bấm "Thanh toán QR" ở giỏ hàng

### Exceptions
- E1. QR hết hạn (>15 phút): hiển thị lỗi, refund tự động.
- E2. Số tiền không khớp: refund tự động, log incident.
```

### Tầng 3 — Entity Model & Ngôn ngữ chung (Ubiquitous Language)

Chia theo **Bounded Context**. Một `Order` ở Checkout Context khác `Order` ở Shipping Context — tên gọi và trạng thái phải được khai báo rõ để AI không đoán mò.

```markdown
# Entity Model — Checkout Context

## Order
- Đại diện: một lần khách đặt hàng
- Trạng thái: draft, awaiting_payment, paid, cancelled

## Payment
- Phương thức: qr_code, credit_card
- Trạng thái: pending, success, failed

## QRSession
- Hết hạn sau 15 phút
```

### Tầng 4 — Acceptance Criteria (AC) — Cầu nối Spec ↔ Test

Viết theo **Given-When-Then**: mỗi AC trở thành 1 test case có thể kiểm chứng.

```markdown
## AC-1: Tạo QR thành công
Given: Khách có giỏ hàng > 0 đồng, đã chọn QR
When : Khách bấm "Thanh toán QR"
Then : Tạo QRSession với expiry = 15 phút
And  : Order chuyển trạng thái `awaiting_payment`

## AC-2: Thanh toán đúng tiền, đúng hạn
Given: QRSession còn thời gian
When : Webhook trả về `success`
Then : Order chuyển trạng thái `paid`

## AC-3: Xử lý QR hết hạn
Given: QRSession đã quá 15 phút
When : Webhook trả về bất kỳ kết quả nào
Then : Refund tự động, đóng phiên Order
```

> 💡 **Quy ước test naming:** `test_UC_<id>_AC_<number>_<scenario>`
> Ví dụ: `test_UC_042_AC_3_qr_expired_refund`.

---

## 5. Ứng dụng #1 — Dự án mới (Greenfield)

Từ 0 đến MVP trong **5 ngày** (case study: Internal HR Tool).

| Ngày | Bước | Sản phẩm |
|------|------|----------|
| 1 | Định hình | Chốt BR vs Goal và Out of Scope. Loại bỏ tính năng ngoài. |
| 2 | Mô hình hóa | Use Case Diagram + Entity Model. Ngôn ngữ chung được chuẩn hóa. |
| 3 | Đặc tả | Viết AC chuẩn Given-When-Then cho từng UC. |
| 4 | Thực thi | AI sinh code, DB, sinh test bám sát AC. |
| 5 | Phản hồi | UAT với stakeholder. Sửa Spec, regenerate phần lệch. |

### Checklist khởi động Greenfield

- [ ] BR (1 trang) chốt rõ **Goal** và **Out of Scope**.
- [ ] Danh sách 5–10 Use Cases cốt lõi (ID định danh rõ ràng).
- [ ] Entity Model sử dụng ngôn ngữ nghiệp vụ chuẩn (Ubiquitous Language).
- [ ] Quy ước commit message: chứa UC ID. VD: `feat(UC-004)`.
- [ ] Quy ước test name: map 1-1 với AC ID.
- [ ] Có ít nhất **1 người duyệt Spec từ góc nhìn nghiệp vụ** (PO/BA).

---

## 6. Ứng dụng #2 — Hệ thống cũ (Brownfield)

5 năm legacy. Không document. Hệ thống vẫn chạy và đang tạo ra doanh thu.

### Hai sai lầm cần tránh

1. **Big Bang Rewrite với AI** — ném 2000 dòng AI viết lại từ đầu nhưng AI không hiểu "luật ngầm" nghiệp vụ → toang nguyên hệ.
2. **Dùng AI sinh Spec từ Code và tin ngay** — AI hợp thức hóa cả bug và hành vi sai chiều lá vì không ai đối chiếu được.

### Chiến lược 4 bước (Vòng lặp Khám phá & Tái cấu trúc)

```
   ┌──▶ 1. Reverse-Engineer ──┐
   │    (Code → Spec nháp)     │
   │                           ▼
   4. Refactor             2. Verify
   (Viết lại sạch)         (Đối chiếu với người
   │                        hiểu nghiệp vụ)
   │                           │
   └─── 3. Characterization Test ──┘
        (Đóng băng hành vi bằng test
         dựa trên Data Production thật)
```

1. **Reverse-Engineer** — Dùng AI trích xuất Entity từ Database & Use Case từ Controller/Log.
2. **Verify** — Đối chiếu Spec nháp với người hiểu nghiệp vụ. AI không tự xác nhận.
3. **Characterization Test** — Bọc test bám hiện trạng bằng data production thật, bảo tồn cả những hành vi “lạ nhưng đúng”.
4. **Refactor** — Viết lại code dưới sự bảo vệ của Test. Đổi hành vi = viết Spec mới trước.

---

## 7. Traceability trong Vận hành

Kịch bản: **Sự cố Production lúc 3h sáng.**

```
1. Lỗi Production            2. Code/Commit            3. Test Case          4. Đặc tả (Spec)
   ─ Alert email gửi    →     ─ Commit log hiển     →   ─ Test bao phủ   →   ─ Mở lại UC-042.md
     "Order #123 fail"          "fix(UC-042)"            AC-2 (đã pass)        Tìm quy định
                                                         nhưng prod fail        nghiệp vụ liên quan
```

Kết quả: Giảm ~90% thời gian đoán mò. Team cập nhật Spec, bổ sung AC-5 (Retry Logic), regenerate code & AI sửa code chuẩn theo Spec.

---

## 8. Tổng hợp: Greenfield vs Brownfield

| | **Dự án mới (Greenfield)** | **Hệ thống cũ (Brownfield)** |
|---|---|---|
| **Điểm xuất phát** | Từ Business Requirement (Goal rõ ràng) | Từ Database Schema & Controller hiện hữu |
| **Mục tiêu Spec** | Thiết kế hệ thống tương lai | Diễn giải sự thật hiện tại (Baseline) & đề xuất đổi mới |
| **Chiến lược Test** | Viết Test dựa trên AC mới hoàn toàn | Characterization Test bảo vệ hiện trạng, mở rộng sau |
| **Phạm vi áp dụng** | 100% Coverage toàn bộ MVP | Chỉ tập trung vào Module Nóng, có tần suất thay đổi cao |

---

## 9. Chuyển dịch Vai trò trong Kỷ nguyên AI

| Vai trò | Trọng tâm mới |
|---------|---------------|
| **Developer** | Không còn đo lường bằng tốc độ gõ phím. Giá trị nằm ở năng lực hiểu Domain, thiết kế ranh giới an toàn và duyệt code do AI sinh ra. |
| **AI Conductor** | Vai trò mới: chỉ huy dàn AI. |
| **Tech Lead / Spec Architect** | Quyết định cấu trúc thư mục, ranh giới Bounded Context, chính sách Traceability. |
| **PO / BA** | Tham gia trực tiếp vào vòng đời Spec: review Use Case và thiết kế cả Edge Cases. |

---

## 10. Khi nào KHÔNG nên áp dụng SDD?

- Dự án Prototype dùng thử **1 lần (dưới 1 tuần)**.
- Spike kỹ thuật (thử nghiệm công nghệ mới).
- Script nội bộ chạy 1 lần duy nhất.
- Side project cá nhân, 1 người làm.

> *“Nếu chi phí bảo trì và chi phí onboard người mới trong tương lai LỚN HƠN thời gian viết Spec ban đầu, Spec-Driven Development là sự đầu tư bắt buộc.”*

---

## 📁 Cấu trúc thư mục

```
project-root/
├── README.md                          ← File này (overview SDD)
├── CLAUDE.md                          ← Hướng dẫn cho AI agent
├── docs/                              ← Hướng dẫn SDD chi tiết
│   ├── README.md                      ← Quick start & template
│   ├── Spec-Driven-Development.md     ← 6 nguyên tắc + 4 tầng
│   ├── Business-Requirement.md        ← Tầng 1: BR
│   ├── Use-Case.md                    ← Tầng 2: UC
│   ├── Entity-Model.md                ← Tầng 3: Entity
│   └── Acceptance-Criteria.md         ← Tầng 4: AC
├── spec/                              ← Spec thực tế của dự án
└── app/                               ← Source code
```

## 🚀 Bắt đầu

1. Đọc tổng quan tại file này.
2. Xem chi tiết 6 nguyên tắc và 4 tầng: [docs/Spec-Driven-Development.md](./docs/Spec-Driven-Development.md).
3. Làm theo quick start: [docs/README.md](./docs/README.md).
4. Khi phát triển feature mới, tạo 4 file Spec trong `specs/features/`:
   - `BR-XXX-<tên>.md`
   - `UC-XXX-<tên>.md`
   - `EM-XXX-<domain>.md`
   - `AC-XXX-<tên>.md`

---

**Last updated:** 2026-06-15
