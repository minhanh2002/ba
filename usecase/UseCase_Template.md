# ĐẶC TẢ CA SỬ DỤNG (USE CASE SPECIFICATION)

## 1. Thông tin chung về Ca sử dụng
* **Mã ca sử dụng (Use Case ID):** UC-[Số thứ tự] *(Ví dụ: UC-001)*
* **Tên ca sử dụng (Use Case Name):** [Tên hành động bắt đầu bằng động từ, ví dụ: Đặt hàng trực tuyến]
* **Tác nhân chính (Primary Actor):** [Ví dụ: Khách hàng]
* **Tác nhân phụ (Secondary Actors):** [Ví dụ: Hệ thống thanh toán bên thứ ba, Nhân viên kho]
* **Người thực hiện (Author):** [Tên của bạn - BA]
* **Ngày tạo (Created Date):** [Ngày tạo tài liệu]
* **Trạng thái (Status):** [Draft / Approved]

## 2. Mô tả Tóm tắt (Brief Description)
Mô tả ngắn gọn về những gì ca sử dụng này thực hiện từ góc nhìn của tác nhân chính.

## 3. Điều kiện Tiên quyết & Kết quả đạt được (Pre-conditions & Post-conditions)
* **Điều kiện tiên quyết (Pre-conditions):** Những điều kiện bắt buộc phải đúng TRƯỚC khi ca sử dụng bắt đầu (Ví dụ: Khách hàng đã đăng nhập tài khoản và có ít nhất một sản phẩm trong giỏ hàng).
* **Kết quả đạt được (Post-conditions):** Những gì hệ thống đạt được SAU khi ca sử dụng kết thúc thành công (Ví dụ: Đơn hàng mới được tạo dưới DB, trừ số lượng tồn kho sản phẩm, gửi email xác nhận cho khách hàng).

## 4. Các Luồng Nghiệp vụ (Flow of Events)

### 4.1. Luồng chính (Basic Flow / Main Success Scenario)
Mô tả các bước diễn ra trong điều kiện lý tưởng (không lỗi).

| Bước | Hành động của Tác nhân (Actor Action) | Phản hồi của Hệ thống (System Response) |
| :--- | :--- | :--- |
| 1 | Khách hàng nhấn nút "Thanh toán" từ màn hình Giỏ hàng. | Hệ thống hiển thị thông tin Đơn hàng và Form nhập địa chỉ nhận hàng, phương thức thanh toán. |
| 2 | Khách hàng điền thông tin giao hàng và chọn phương thức "Thanh toán khi nhận hàng (COD)". | Hệ thống kiểm tra tính hợp lệ của địa chỉ và số điện thoại. |
| 3 | Khách hàng nhấn nút "Xác nhận đặt hàng". | Hệ thống lưu đơn hàng vào DB với trạng thái "Chờ xử lý", gửi email xác nhận đặt hàng thành công, hiển thị trang hoàn tất đơn hàng. |

### 4.2. Luồng thay thế / Luồng phụ (Alternative Flows)
Các nhánh rẽ khác vẫn dẫn đến kết quả thành công.
* **Alt-Flow 1: Khách hàng chọn thanh toán qua thẻ ngân hàng**
  - Tại bước 2 luồng chính, khách hàng chọn phương thức "Thanh toán qua Thẻ/Ví điện tử".
  - Hệ thống chuyển hướng người dùng sang Cổng thanh toán.
  - Sau khi giao dịch thành công, Cổng thanh toán phản hồi và hệ thống tiếp tục bước 3.

### 4.3. Luồng ngoại lệ / Luồng lỗi (Exception Flows)
Các nhánh rẽ dẫn đến lỗi hoặc thất bại.
* **Exc-Flow 1: Số điện thoại không hợp lệ**
  - Tại bước 2, khách hàng nhập số điện thoại sai định dạng (ví dụ: thiếu số, có chữ cái).
  - Hệ thống báo lỗi ngay tại ô nhập và vô hiệu hóa nút "Xác nhận đặt hàng".
  - Ca sử dụng quay lại trạng thái chờ khách hàng sửa thông tin.

## 5. Quy tắc Nghiệp vụ áp dụng (Business Rules)
* Chỉ cho phép đặt hàng khi số tiền tối thiểu của đơn hàng từ 50,000 VND trở lên.
* Phí vận chuyển được tính tự động dựa trên khoảng cách địa lý từ kho gần nhất đến địa chỉ người nhận.
