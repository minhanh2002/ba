# BIỂU MẪU USER STORY & TIÊU CHÍ NGHIỆM THU (ACCEPTANCE CRITERIA)

## 1. Thông tin chung
* **Mã User Story (Story ID):** US-[Số thứ tự] *(Ví dụ: US-001)*
* **Tên User Story (Title):** [Tên ngắn gọn, ví dụ: Thêm sản phẩm vào giỏ hàng]
* **Epic liên quan (Epic):** [Tên Epic lớn, ví dụ: Quản lý mua sắm (E-Commerce)]
* **Độ ưu tiên (Priority):** [High / Medium / Low]
* **Điểm ước lượng (Story Points):** [1, 2, 3, 5, 8...]

## 2. Nội dung User Story (User Story Statement)
Cấu trúc chuẩn ba thành phần:
* **As a (Với tư cách là):** [Tác nhân / Vai trò người dùng]
* **I want to (Tôi muốn):** [Thực hiện hành động hoặc có tính năng gì]
* **So that (Để):** [Đạt được lợi ích hoặc giá trị kinh doanh gì]

> *Ví dụ:*
> * **As a** Khách hàng chưa đăng nhập,
> * **I want to** thêm sản phẩm vào giỏ hàng từ trang danh sách sản phẩm nhanh chóng,
> * **So that** tôi có thể tiếp tục mua sắm mà không bị gián đoạn và xem lại các món đồ đã chọn trước khi thanh toán.

## 3. Tiêu chí Nghiệm thu (Acceptance Criteria - AC)
Sử dụng cấu trúc **Given - When - Then** để mô tả các kịch bản nghiệm thu cụ thể và rõ ràng cho kiểm thử viên (Tester) và lập trình viên (Developer).

### Kịch bản 1: Thêm sản phẩm thành công từ trang danh sách (Thành công)
* **Given (Giả sử):** Người dùng đang ở trang Danh sách sản phẩm và giỏ hàng đang trống (0 sản phẩm).
* **When (Khi):** Người dùng nhấn nút "Thêm vào giỏ" trên thẻ sản phẩm A.
* **Then (Thì):**
  - Hệ thống thêm sản phẩm A vào giỏ hàng.
  - Số lượng hiển thị trên biểu tượng giỏ hàng ở góc phải màn hình cập nhật từ `0` thành `1`.
  - Một thông báo nhỏ (Toast message) xuất hiện với nội dung "Thêm sản phẩm thành công!".

### Kịch bản 2: Sản phẩm hết hàng (Thất bại)
* **Given (Giả sử):** Người dùng đang ở trang Danh sách sản phẩm và sản phẩm B đã hết hàng trong kho.
* **When (Khi):** Người dùng xem thẻ sản phẩm B.
* **Then (Thì):**
  - Nút "Thêm vào giỏ" trên thẻ sản phẩm B bị vô hiệu hóa (disabled) và hiển thị chữ "Hết hàng".
  - Người dùng không thể nhấn click vào nút này.

## 4. Ghi chú kỹ thuật & UX (Technical Notes & UX Specifications)
* **Giao diện tham chiếu (Wireframe Link):** Xem file [README.md](../wireframe/README.md) hoặc link Figma tại đây.
* **Ràng buộc cơ sở dữ liệu:** Lưu thông tin sản phẩm trong bảng `Cart` dưới dạng Session nếu người dùng chưa đăng nhập, và đồng bộ vào DB khi người dùng đăng nhập thành công.
