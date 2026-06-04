# HƯỚNG DẪN QUẢN LÝ THIẾT KẾ PHÁC THẢO (WIREFRAMES & MOCKUPS)

Thư mục này là nơi lưu trữ các hình ảnh phác thảo giao diện (Wireframes), sơ đồ luồng người dùng (User Flows) hoặc các đường dẫn (Links) đến các công cụ thiết kế trực tuyến như Figma, Adobe XD, Balsamiq.

## 1. Lưu trữ các Link Thiết kế trực tuyến
Khi làm việc với UI/UX Designer hoặc tự phác thảo, bạn nên lưu lại các link dự án tại đây để dễ dàng chia sẻ cho Dev/Tester:

* **Link Figma (Thiết kế chi tiết - UI/UX Mockups):** `[Dán link Figma của bạn vào đây]`
* **Link Balsamiq / Draw.io (Phác thảo cấu trúc - Wireframes):** `[Dán link vào đây]`

## 2. Quy tắc Đặt tên File Ảnh (nếu lưu trực tiếp)
Nếu bạn lưu file hình ảnh phác thảo trực tiếp trong thư mục này, hãy đặt tên theo quy tắc sau để tránh lộn xộn:
* Cấu trúc: `[Mã tính năng]_[Tên màn hình]_[Phiên bản].png`
* Ví dụ:
  - `US001_Homepage_Desktop_v1.png` (Trang chủ phiên bản Desktop v1)
  - `US001_Homepage_Mobile_v1.png` (Trang chủ phiên bản Mobile v1)
  - `US002_Cart_Detail_v2.png` (Trang chi tiết giỏ hàng phiên bản v2)

## 3. Mô tả Tương tác Giao diện (UI Interactions Specification)
Bên dưới là bảng ví dụ cách BA mô tả các tương tác cụ thể của từng phần tử trên màn hình để Developer lập trình đúng logic:

| Tên Phần tử (UI Element) | Loại phần tử (Type) | Mô tả Hoạt động / Tương tác (Interaction Description) |
| :--- | :--- | :--- |
| **Nút "Mua ngay"** | Button | Khi click: Kiểm tra xem đã chọn size/màu chưa. Nếu rồi, chuyển sang trang Thanh toán. Nếu chưa, rung nhẹ thẻ sản phẩm và báo đỏ vùng chọn size. |
| **Ô tìm kiếm** | Text Input | Gợi ý tự động (Auto-suggestion) sẽ xuất hiện sau khi người dùng nhập từ 3 ký tự trở lên. Thời gian debounce là 300ms. |
| **Banner quảng cáo** | Carousel | Tự động chuyển banner sau mỗi 5 giây. Hỗ trợ thao tác vuốt (swipe) trên thiết bị di động. |
