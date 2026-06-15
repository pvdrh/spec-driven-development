## Acceptance Criteria (Đặt ở phần cuối của file Use Case)

### AC-1: <Tên kịch bản nghiệm thu 1 - vd: Submit thành công>
Given: <Trạng thái bối cảnh ban đầu, vd: Khách có giỏ hàng > 0 đồng>
When: <Sự kiện xảy ra, vd: Khách bấm Thanh toán>
Then: <Hệ quả chính, vd: Hệ thống tạo đơn hàng trạng thái pending>
And: <Hệ quả phụ, vd: Gửi email xác nhận>

### AC-2: <Tên kịch bản nghiệm thu 2 - vd: Xử lý ngoại lệ>
Given: <Trạng thái bối cảnh, vd: Mã QR đã hết hạn>
When: <Sự kiện, vd: Hệ thống nhận được webhook thanh toán>
Then: <Hệ quả, vd: Tự động hoàn tiền (Refund)>
And: <Hệ quả phụ, vd: Báo lỗi trên màn hình>