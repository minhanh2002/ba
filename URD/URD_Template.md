# TÀI LIỆU YÊU CẦU NGƯỜI DÙNG (USER REQUIREMENTS DOCUMENT - URD)

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

## 3. Chân dung Người dùng (User Personas)
Mô tả các nhóm đối tượng người dùng cuối sẽ trực tiếp tương tác với hệ thống.

| Nhóm Người dùng (User Persona) | Mô tả Đặc điểm | Nhu cầu chính (Core Needs) | Nỗi đau hiện tại (Pain Points) |
| :--- | :--- | :--- | :--- |
| **Khách hàng cá nhân** | Nhóm người mua sắm online, độ tuổi 18-35, sử dụng smartphone là chủ yếu. | Mua hàng nhanh chóng, giao diện đơn giản, thanh toán an toàn, ưu đãi hấp dẫn. | Mất nhiều thời gian đăng ký tài khoản, giao diện giật lag khi mạng yếu. |
| **Nhân viên vận hành** | Bộ phận kho và chăm sóc khách hàng, sử dụng PC/Laptop tại văn phòng. | Cập nhật trạng thái đơn hàng nhanh chóng, quản lý tồn kho chính xác. | Phải nhập dữ liệu thủ công nhiều lần giữa các hệ thống cũ và mới. |

## 4. Danh sách Yêu cầu của Người dùng (User Requirements List)
Phần này mô tả những mong muốn, yêu cầu thực tế từ góc nhìn của người dùng (User Perspective), tập trung vào mục tiêu của họ thay vì giải pháp kỹ thuật.

| Mã URD (URD ID) | Người dùng (Who) | Muốn làm gì (What) | Để đạt mục đích gì (Why) | Độ ưu tiên (Must/Should/Could/Won't) |
| :--- | :--- | :--- | :--- | :--- |
| UR-001 | Khách hàng | Tôi muốn đăng nhập bằng tài khoản Google hoặc Facebook | Để tôi không phải nhớ thêm một mật khẩu mới và rút ngắn thời gian tạo tài khoản. | Must |
| UR-002 | Khách hàng | Tôi muốn nhận được thông báo đẩy (push notification) trên điện thoại khi đơn hàng đang được giao | Để tôi biết thời gian cụ thể nhận hàng và chuẩn bị tiền mặt. | Should |
| UR-003 | Nhân viên vận hành | Tôi muốn xuất báo cáo doanh thu tuần sang định dạng Excel | Để tôi làm báo cáo gửi sếp mà không cần chụp màn hình hoặc tính toán thủ công. | Must |

## 5. Môi trường sử dụng & Trải nghiệm (User Environment & UX Needs)
* **Môi trường hoạt động:** Người dùng cuối chủ yếu dùng ngoài đường (3G/4G chập chờn) hay dùng tại văn phòng (mạng LAN ổn định)?
* **Yêu cầu về trải nghiệm người dùng:** Giao diện tối giản, cỡ chữ lớn dễ đọc cho người lớn tuổi, hay giao diện hiện đại thời thượng?
* **Hỗ trợ người khuyết tật (Accessibility):** Có yêu cầu đặc biệt nào về đọc màn hình (Screen Reader), tương phản màu sắc không?
