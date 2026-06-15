# Entity Model — <Tên Context, vd: Checkout Context>

## Order (Tên Thực thể 1)
- Đại diện: Một lần khách đặt hàng
- Trạng thái: draft, awaiting_payment, paid, cancelled
- Có nhiều: OrderItem
- Có một: Payment (sau khi paid)

## OrderItem (Tên Thực thể 2)
- Một dòng trong đơn hàng
- Thuộc: Order
- Tham chiếu: Product

## Payment (Tên Thực thể 3)
- Một giao dịch thanh toán
- Thuộc: Order
- Phương thức: qr_code, credit_card
- Trạng thái: pending, success, failed