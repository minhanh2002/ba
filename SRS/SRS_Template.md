# ĐẶC TẢ YÊU CẦU PHẦN MỀM (SOFTWARE REQUIREMENTS SPECIFICATION - SRS)

## 1. Thông tin chung về Tài liệu
* **Tên Dự án (Project Name):** [Nhập tên dự án]
* **Người thực hiện (Author):** [Tên của bạn - BA]
* **Ngày tạo (Created Date):** [Ngày tạo tài liệu]
* **Trạng thái (Status):** [Draft / Under Review / Approved]
* **Phiên bản (Version):** [Ví dụ: 1.0]

## 2. Nhật ký Thay đổi (Document Change History)
| Phiên bản | Ngày thay đổi | Người thay đổi | Lý do thay đổi / Nội dung cập nhật |
| :--- | :--- | :--- | :--- |
| 0.1 | [Ngày] | [Tên] | Khởi tạo bản nháp đầu tiên (Draft) |

## 3. Giới thiệu hệ thống (System Introduction)
* **Mục đích (Purpose):** Tài liệu này mô tả chi tiết các yêu cầu kỹ thuật và chức năng của phần mềm để đội ngũ lập trình (Developer) và kiểm thử (Tester) thực hiện.
* **Phạm vi hệ thống (System Scope):** Mô tả phần mềm sẽ làm gì và không làm gì ở mức độ hệ thống.

## 4. Kiến trúc & Sơ đồ Hệ thống (System Architecture & Diagrams)
> *Mẹo cho BA: Hãy chèn sơ đồ luồng dữ liệu (DFD), sơ đồ ca sử dụng tổng quan (Use Case Diagram) hoặc sơ đồ kiến trúc hệ thống tại đây.*
* **Sơ đồ luồng (Data Flow Diagram - DFD):** [Đường dẫn ảnh hoặc mô tả]
* **Sơ đồ ca sử dụng (Use Case Diagram):** [Đường dẫn ảnh hoặc mô tả]

## 5. Yêu cầu Chức năng chi tiết (Functional Requirements)
Phần này mô tả chi tiết từng tính năng của hệ thống.

### 5.1. [Tên Tính năng 1 - Ví dụ: Đăng nhập]
* **Mô tả (Description):** Mô tả tóm tắt tính năng làm gì.
* **Tác nhân (Actor):** Khách hàng, Admin...
* **Luồng xử lý chính (Main Flow):**
  1. Người dùng truy cập trang Đăng nhập.
  2. Hệ thống hiển thị form nhập Email và Mật khẩu.
  3. Người dùng điền thông tin và nhấn nút "Đăng nhập".
  4. Hệ thống kiểm tra thông tin dưới Cơ sở dữ liệu.
  5. Hệ thống xác thực thành công và chuyển hướng người dùng vào trang chủ.
* **Luồng thay thế / Luồng lỗi (Alternative/Exception Flows):**
  - *Mật khẩu sai:* Hệ thống hiển thị thông báo lỗi "Mật khẩu không chính xác" và giữ nguyên trang đăng nhập.
  - *Tài khoản bị khóa:* Hệ thống hiển thị thông báo lỗi "Tài khoản của bạn đã bị khóa".
* **Quy tắc Nghiệp vụ (Business Rules):**
  - Mật khẩu phải chứa tối thiểu 8 ký tự, có ít nhất 1 chữ hoa, 1 chữ thường và 1 số.
  - Cho phép đăng nhập sai tối đa 5 lần liên tiếp.

### 5.2. [Tên Tính năng 2...]

## 6. Yêu cầu Phi chức năng (Non-Functional Requirements)
* **Hiệu năng (Performance):** Thời gian phản hồi trang dưới 2 giây. Hệ thống hỗ trợ tối thiểu 10,000 người dùng truy cập đồng thời.
* **Bảo mật (Security):** Mật khẩu người dùng phải được mã hóa một chiều bằng thuật toán bcrypt trước khi lưu vào DB. Toàn bộ đường truyền phải qua HTTPS.
* **Khả năng mở rộng & Bảo trì (Maintainability/Scalability):** Hệ thống được thiết kế theo dạng Microservices để dễ dàng nâng cấp.
* **Tính tương thích (Compatibility):** Hỗ trợ trên các trình duyệt phổ biến Chrome, Safari, Firefox, Edge và hiển thị tốt trên thiết bị di động (Responsive).

## 7. Yêu cầu về Giao diện người dùng (User Interface Requirements)
* Phác thảo bố cục (Layout Wireframes) được đính kèm ở thư mục `/wireframe`.
* Quy chuẩn thiết kế (Design System): Font chữ chủ đạo Inter, màu sắc thương hiệu lục/xanh dương.
