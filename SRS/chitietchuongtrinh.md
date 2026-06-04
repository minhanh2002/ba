# Xem danh sách và tìm kiếm chương trình đánh giá – Thông tin chức năng & Luồng xử lý


## 1. Thông tin chức năng (Bảng 2 cột)


| Thuộc tính | Nội dung |
|---|---|
| **Tên chức năng** | Xem danh sách và tìm kiếm chương trình đánh giá |
| **Mã chức năng** | `EVALUATION_PROGRAM_MANAGEMENT` |
| **Mã thao tác** | - Tìm kiếm / Xem danh sách: `SEARCH`<br>- Thêm mới: `CREATE`<br>- Chỉnh sửa cấu hình: `EDIT`<br>- Xóa chương trình: `DELETE`<br>- Xem chi tiết: `VIEW`<br>- Bắt đầu đánh giá: `START`<br>- Hủy đánh giá: `CANCEL`<br>- Xử lý lỗi: `RESOLVE_ERROR`<br>- Hoàn thành đánh giá: `COMPLETE` |
| **Mô tả** | Cung cấp giao diện trung tâm hiển thị danh sách các chương trình đánh giá của hệ thống. Cho phép người dùng tìm kiếm, lọc theo trạng thái (qua các tab-menu), lọc nâng cao và truy cập nhanh các hành động (chỉnh sửa, xóa, xem chi tiết, bắt đầu đánh giá, hủy đánh giá, xử lý lỗi, hoàn thành đánh giá, đánh giá đơn vị, cung cấp sở cứ) dựa trên vai trò của tài khoản hiện tại. |
| **Tác nhân** | Quản trị hệ thống (Admin), Đầu mối điều phối, Chuyên gia đánh giá (Đầu mối đánh giá), Đầu mối đơn vị |
| **Tiền điều kiện** | - Người dùng đã đăng nhập hệ thống.<br>- Người dùng có quyền truy cập màn hình Danh sách chương trình đánh giá (kiểm tra quyền bằng mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `SEARCH`). |
| **Hậu điều kiện** | - Hệ thống hiển thị danh sách chương trình kèm các chỉ số tiến độ, trạng thái và nút hành động thích hợp theo vai trò người dùng.<br>- Cho phép chuyển hướng chính xác đến các chức năng nghiệp vụ tiếp theo (Chỉnh sửa, Xem chi tiết, Cung cấp sở cứ, Đánh giá đơn vị...). |
| **Ngoại lệ** | - Người dùng không có quyền truy cập hệ thống (không có quyền `EVALUATION_PROGRAM_MANAGEMENT` - `SEARCH`) → Hệ thống chặn và chuyển hướng về trang đăng nhập hoặc hiển thị lỗi truy cập. |
| **Yêu cầu nghiệp vụ** | - Phân trang danh sách chương trình (mặc định hiển thị 10 bản ghi/trang).<br>- Tự động lọc danh sách và hiển thị số lượng bản ghi tương ứng trên các Tab-menu (*Tất cả*, *Chờ duyệt*, *Chờ sở cứ*) theo vai trò người dùng.<br>- Hiển thị động các nút hành động trong cột "Thao tác" (cả nút nhanh và dropdown menu) theo vai trò và trạng thái thực tế của chương trình/đơn vị. |


---


## 2. Luồng xử lý chi tiết (các bước)


### Bước 1: Người dùng truy cập chức năng
* Khi người dùng truy cập màn hình Danh sách chương trình đánh giá, hệ thống kiểm tra quyền truy cập qua mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `SEARCH`.
* Sau khi xác thực quyền thành công, hệ thống lấy ID tài khoản hiện tại (`currentUserId`) và danh sách các vai trò (Role) của người dùng đó để thực hiện truy vấn và kết xuất giao diện.


### Bước 2: Hiển thị danh sách chương trình & Phân trang
* Hệ thống tải danh sách chương trình đánh giá từ bảng `evaluation_program`.
* Mỗi chương trình đánh giá hiển thị các thông tin:
  * **Tên chương trình**: `evaluation_program.name`.
  * **Thời gian đánh giá**: `evaluation_program.start_date` - `evaluation_program.end_date`.
  * **Đầu mối điều phối**: Lấy email điều phối viên liên kết từ bảng người dùng (`user.email`).
  * **Số đơn vị đánh giá**: Đếm số đơn vị tham gia chương trình trong bảng `program_department_mapping`.
  * **Tiến độ**: Tính bằng tỷ lệ % số lượng luật đã có kết quả đánh giá (trong bảng `rule_run` mới nhất đối với luật tự động và `rule_manual_review_mapping` mới nhất đối với luật thủ công) trên tổng số luật của tất cả các đơn vị trong chương trình.
  * **Đơn vị tạo**: Đơn vị của đầu mối điều phối chính (lấy thông tin đơn vị từ bảng `user` liên kết điều phối viên).
  * **Trạng thái**: Nhãn hiển thị trạng thái của chương trình (lấy từ `evaluation_program.status` gồm các trạng thái: 0: Chưa đánh giá, 1: Đang đánh giá, 2: Hoàn thành đánh giá, 3: Hủy đánh giá).
  * **Người cập nhật** & **Ngày cập nhật**: `evaluation_program.updated_by` và `evaluation_program.updated_at`.
* **Phân trang**: Hệ thống chia danh sách thành nhiều trang. Người dùng có thể chọn số bản ghi/trang (10, 20, 50) và click các nút chuyển trang (Trước, Sau, Đầu, Cuối).


### Bước 3: Tìm kiếm và Lọc nâng cao
* **Tìm kiếm theo tên**: Người dùng nhập từ khóa vào ô "Tên chương trình" và nhấn Enter hoặc dừng gõ 500ms → Hệ thống gửi yêu cầu tải lại danh sách với điều kiện lọc tương ứng (`evaluation_program.name LIKE %keyword%`).
* **Lọc nâng cao**: Click vào icon filter để mở popup cấu hình các trường lọc bổ sung (thời gian, đơn vị tạo, trạng thái...).


### Bước 4: Chuyển đổi giữa các tab-menu
Hệ thống hiển thị các tab-menu đi kèm số lượng bản ghi tương ứng. Dữ liệu được lọc theo vai trò của người dùng:
* **Tab "Tất cả"**: Hiển thị toàn bộ chương trình mà người dùng được quyền xem (lọc theo đơn vị phân bổ hoặc quyền hạn chung).
* **Tab "Chờ duyệt"**:
  * *Đầu mối điều phối*: Chương trình ở trạng thái Đang đánh giá (`status = 1`) cần hoàn thành hoặc có lỗi tự động.
  * *Đầu mối đánh giá*: Chương trình có đơn vị ở trạng thái chờ đánh giá/đánh giá lại (`manual_review_status IN (1, 4)`).
* **Tab "Chờ sở cứ"**:
  * *Đầu mối đơn vị*: Chương trình ở trạng thái Đang đánh giá (`status = 1`) và đơn vị của mình đang ở trạng thái chưa nộp sở cứ hoặc bị từ chối nộp lại (`manual_review_status IN (0, 3)`).


### Bước 5: Kiểm tra và hiển thị các nút thao tác động
Với mỗi bản ghi trong danh sách chương trình, hệ thống kiểm tra vai trò người dùng hiện tại, trạng thái chương trình và phân quyền chức năng để quyết định hiển thị các nút thao tác tương ứng (Xem bảng ma trận hiển thị nút ở Mục 3). Các nút thao tác gồm:
* Nút icon nhanh:
  * **Xem chi tiết** (icon hình mắt 👁️).
  * **Chỉnh sửa** (icon hình bút chì ✏️): Chỉ hiển thị khi trạng thái chương trình là `0` và tài khoản người dùng có quyền chỉnh sửa (kiểm tra mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `EDIT`).
* Menu dropdown ba chấm (⋮) chứa các tùy chọn hành động khác như: *Bắt đầu đánh giá*, *Hủy đánh giá*, *Hoàn thành chương trình*, *Cung cấp sở cứ*, *Đánh giá đơn vị*, *Xóa*, *Xử lý lỗi*.
* Nút **+ Thêm mới** trên giao diện header: Chỉ hiển thị khi tài khoản người dùng có quyền thêm mới (kiểm tra mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `CREATE`).


### Bước 6: Xử lý nghiệp vụ khi click các nút thao tác
* **Xem chi tiết**: Điều hướng người dùng sang màn hình **Xem chi tiết chương trình đánh giá** (truyền ID chương trình).
* **Chỉnh sửa**: Điều hướng sang màn hình cấu hình thông tin chương trình đánh giá.
* **Xóa**: Hiển thị popup xác nhận xóa. Nếu đồng ý, thực hiện xóa chương trình trong DB và làm mới danh sách.
* **Bắt đầu đánh giá**: Cập nhật trạng thái chương trình thành `status = 1` (Đang đánh giá).
* **Hủy**: Cập nhật trạng thái chương trình thành `status = 3` (Hủy đánh giá).
* **Xử lý lỗi**: Mở giao diện xử lý các lỗi đánh giá tự động.
* **Hoàn thành đánh giá**: Cập nhật trạng thái chương trình thành `status = 2` (Hoàn thành đánh giá).
* **Đánh giá đơn vị**: Chuyển hướng sang màn hình Đánh giá đơn vị của chuyên gia.
* **Cung cấp sở cứ**: Chuyển hướng sang màn hình Cung cấp sở cứ (Primary hoặc Bổ sung tùy thuộc trạng thái đơn vị).


---


## 3. Ma trận hiển thị nút thao tác theo vai trò người dùng


| Vai trò | Nút hiển thị | Điều kiện hiển thị | Ý nghĩa hành động | Hiển thị theo tab-menu |
|---|---|---|---|---|
| **Admin** | Chỉnh sửa | (currentUserId = admin OR có quyền `EDIT` thuộc `EVALUATION_PROGRAM_MANAGEMENT`) AND `evaluation_program.status = 0` | Chỉnh sửa cấu hình chương trình đánh giá | Tất cả |
| **Admin** | Xóa | (currentUserId = admin OR có quyền `DELETE` thuộc `EVALUATION_PROGRAM_MANAGEMENT`) AND `evaluation_program.status = 0` | Xóa chương trình đánh giá | Tất cả |
| **Admin** | Xem chi tiết | currentUserId = admin OR có quyền `VIEW` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Xem thông tin cấu hình và kết quả đánh giá | Tất cả |
| **Đầu mối điều phối** | Bắt đầu đánh giá | `evaluation_program.program_auditor = currentUserId` AND `evaluation_program.status = 0` AND có quyền `START` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Bắt đầu thực hiện chương trình đánh giá | Tất cả, Chờ duyệt |
| **Đầu mối điều phối** | Hủy | `evaluation_program.program_auditor = currentUserId` AND `evaluation_program.status = 1` AND có quyền `CANCEL` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Hủy chương trình đánh giá | Tất cả |
| **Đầu mối điều phối** | Xử lý lỗi | `evaluation_program.program_auditor = currentUserId` AND `evaluation_program.status = 1` AND tồn tại đơn vị có `auto_review_status = 3` AND có quyền `RESOLVE_ERROR` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Thực hiện xử lý các đánh giá tự động bị lỗi | Tất cả, Chờ duyệt |
| **Đầu mối điều phối** | Hoàn thành đánh giá | `evaluation_program.program_auditor = currentUserId` AND `evaluation_program.status = 1` AND tất cả đơn vị có `manual_review_status = 2` AND `auto_review_status = 2` AND có quyền `COMPLETE` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Hoàn thành chương trình đánh giá | Tất cả, Chờ duyệt |
| **Đầu mối điều phối** | Xem chi tiết | `evaluation_program.program_auditor = currentUserId` AND `evaluation_program.status IN (1, 2, 3)` AND có quyền `VIEW` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Xem thông tin cấu hình và tiến độ/kết quả đánh giá | Tất cả |
| **Đầu mối đánh giá** | Đánh giá đơn vị | `department_reviewer_mapping.representative_id = currentUserId` AND `evaluation_program.status = 1` AND `manual_review_status IN (1, 4)` AND có quyền `REVIEW` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Thực hiện đánh giá hoặc đánh giá lại đơn vị | Tất cả, Chờ duyệt |
| **Đầu mối đánh giá** | Xem chi tiết | `department_reviewer_mapping.representative_id = currentUserId` AND có quyền `VIEW` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Xem thông tin cấu hình và kết quả đánh giá | Tất cả |
| **Đầu mối đơn vị** | Cung cấp sở cứ | `department_representative_mapping.representative_id = currentUserId` AND `evaluation_program.status = 1` AND `manual_review_status IN (0, 3)` AND có quyền `SEND` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Cung cấp hoặc bổ sung sở cứ đánh giá | Tất cả, Chờ sở cứ |
| **Đầu mối đơn vị** | Xem chi tiết | `department_representative_mapping.representative_id = currentUserId` AND có quyền `VIEW` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Xem thông tin cấu hình và kết quả đánh giá | Tất cả |


---


## 4. Thông tin chi tiết thành phần màn hình


| STT | Tên phần tử giao diện | Kiểu dữ liệu | I/O | Giá trị khởi tạo | Mô tả chi tiết (Mapping CSDL & Thao tác Button) |
|---|---|---|---|---|---|
| 1 | Tiêu đề trang | string | Output | "Chương trình đánh giá" | Tiêu đề màn hình hiển thị. |
| 2 | Tab-menu "Tất cả" | tab button | Input/Output | Active | Khi click, hiển thị tất cả chương trình thuộc phạm vi được quyền xem. |
| 3 | Tab-menu "Chờ duyệt" | tab button | Input/Output | Hiển thị số lượng | Hiển thị số lượng chương trình cần xử lý của Điều phối viên hoặc Chuyên gia đánh giá.<br>- **Thao tác**: Click $\rightarrow$ Lọc danh sách chương trình đang cần đánh giá hoặc đang gặp lỗi. |
| 4 | Tab-menu "Chờ sở cứ" | tab button | Input/Output | Hiển thị số lượng | Hiển thị số lượng chương trình cần nộp sở cứ của Đầu mối đơn vị.<br>- **Thao tác**: Click $\rightarrow$ Lọc danh sách chương trình đơn vị chưa hoàn thành gửi sở cứ. |
| 5 | Ô tìm kiếm chương trình | input text | Input | "" | Nhập từ khóa để lọc danh sách theo tên chương trình.<br>- **Thao tác**: Nhập dữ liệu và nhấn Enter $\rightarrow$ Hệ thống lọc theo `evaluation_program.name` (yêu cầu quyền mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `SEARCH`). |
| 6 | Nút bộ lọc nâng cao (Icon 🎛️) | icon button | Input | Hiển thị số lượng bộ lọc đang chọn | Click để mở popup các trường lọc phụ (thời gian, trạng thái, người tạo...). |
| 7 | Nút "+ Thêm mới" | button | Input/Output | Active nếu có quyền | Cho phép điều hướng sang màn hình thêm mới chương trình đánh giá.<br>- **Quy tắc hiển thị**: Chỉ hiển thị với vai trò Admin hoặc tài khoản có quyền khởi tạo chương trình (yêu cầu quyền mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `CREATE`). |
| 8 | Bảng danh sách chương trình | table | Output | - | Hiển thị thông tin tổng hợp của các chương trình đánh giá lấy từ DB (các cột thông tin được chi tiết ở Mục 2). |
| 9 | Nút nhanh "Xem chi tiết" (Icon 👁️) | icon button | Input | - | Nút thao tác nhanh trên từng dòng.<br>- **Thao tác**: Click $\rightarrow$ Chuyển hướng sang màn hình **Xem chi tiết chương trình đánh giá** (lọc theo điều kiện phân quyền và yêu cầu quyền đọc từ mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `VIEW`). |
| 10 | Nút nhanh "Chỉnh sửa" (Icon ✏️) | icon button | Input | - | Nút thao tác nhanh trên từng dòng.<br>- **Thao tác**: Chỉ hiển thị khi trạng thái chương trình là `0` (Chưa đánh giá) và người dùng có quyền chỉnh sửa (yêu cầu quyền mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `EDIT` hoặc vai trò Admin). Click $\rightarrow$ Chuyển hướng sang màn hình chỉnh sửa cấu hình chương trình. |
| 11 | Menu ba chấm thả xuống (Dropdown actions) | dropdown menu | Input/Output | - | Chứa danh sách các nút hành động phụ.<br>- **Quy tắc hiển thị**: Tự động tính toán các nút con bên trong dựa vào **Ma trận hiển thị nút thao tác (Mục 3)** tương ứng với vai trò của người đăng nhập và trạng thái của chương trình trên dòng đó. |
| 12 | Thanh phân trang | component | Input/Output | 10 bản ghi/trang | Cho phép chọn số lượng bản ghi hiển thị trên trang và điều khiển chuyển trang. |


---


## 5. Bảng phân quyền thao tác hệ thống (System Action Permission Table)


Dưới đây là bảng tổng hợp cấu trúc phân quyền chức năng và quyền thao tác (Feature - Action Permissions) cho toàn hệ thống nhằm đảm bảo tính đồng bộ giữa các tài liệu đặc tả:


| STT | Tên chức năng (Module) | Mã chức năng (Function Code) | Thao tác (Action) | Mã thao tác (Action Code) | Vai trò được phép (Role) | Mô tả chi tiết hành động kiểm tra |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **1** | **Chương trình đánh giá** | `EVALUATION_PROGRAM_MANAGEMENT` | Tìm kiếm / Xem danh sách | `SEARCH` | Admin, Đầu mối điều phối, Đầu mối đánh giá, Đầu mối đơn vị | Xem danh sách chương trình, sử dụng bộ lọc tìm kiếm. |
| | | | Thêm mới | `CREATE` | Admin | Truy cập màn hình thêm mới chương trình đánh giá. |
| | | | Chỉnh sửa cấu hình | `EDIT` | Admin | Truy cập màn hình chỉnh sửa cấu hình chương trình ở trạng thái 0 (Chưa đánh giá). |
| | | | Xóa chương trình | `DELETE` | Admin | Thực hiện hành động xóa chương trình ở trạng thái 0. |
| | | | Xem chi tiết chương trình | `VIEW` | Admin, Đầu mối điều phối, Đầu mối đánh giá, Đầu mối đơn vị | Xem chi tiết cấu hình và tiến độ/kết quả đánh giá của chương trình. |
| | | | Bắt đầu đánh giá | `START` | Đầu mối điều phối | Chuyển trạng thái chương trình thành 1 (Đang đánh giá). |
| | | | Hủy đánh giá | `CANCEL` | Đầu mối điều phối | Chuyển trạng thái chương trình thành 3 (Hủy đánh giá). |
| | | | Xử lý lỗi | `RESOLVE_ERROR` | Đầu mối điều phối | Mở giao diện và thực thi chạy lại các usecase tự động bị lỗi. |
| | | | Hoàn thành đánh giá | `COMPLETE` | Đầu mối điều phối | Chuyển trạng thái chương trình thành 2 (Hoàn thành đánh giá). |
| | | | Cung cấp sở cứ | `SEND` | Đầu mối đơn vị | Truy cập màn hình và nộp sở cứ, sở cứ bổ sung. |
| | | | Đánh giá đơn vị | `REVIEW` | Đầu mối đánh giá (Chuyên gia) | Thực hiện đánh giá, đánh giá bổ sung. |


---
# Xem chi tiết chương trình đánh giá – Thông tin chức năng & Luồng xử lý


## 1. Thông tin chức năng (Bảng 2 cột)


| Thuộc tính | Nội dung |
|---|---|
| **Tên chức năng** | Xem chi tiết chương trình đánh giá |
| **Mã chức năng** | `EVALUATION_PROGRAM_MANAGEMENT` |
| **Mã thao tác** | `VIEW` |
| **Mô tả** | Cho phép người dùng xem thông tin tổng quan của một chương trình đánh giá, kết quả đánh giá chi tiết của từng đơn vị, và trạng thái đạt/không đạt của từng usecase theo mục tiêu và tiêu chí đánh giá. Đồng thời cung cấp các nút chức năng xử lý tương ứng tùy theo quyền của người dùng. |
| **Tác nhân** | Đầu mối điều phối, Chuyên gia đánh giá (Đầu mối đánh giá), Đầu mối đơn vị, Quản trị hệ thống |
| **Tiền điều kiện** | - Người dùng đã đăng nhập hệ thống.<br>- Người dùng có quyền xem chi tiết chương trình đánh giá (mã chức năng `EVALUATION_PROGRAM_MANAGEMENT`, mã thao tác `VIEW`).<br>- Chương trình đánh giá đã được cấu hình và chạy. |
| **Hậu điều kiện** | - Hệ thống hiển thị toàn bộ thông tin chi tiết chương trình đánh giá, kết quả của từng đơn vị thuộc phạm vi được phép truy cập.<br>- Hiển thị các nút chức năng (Cung cấp sở cứ, Đánh giá đơn vị, Xử lý lỗi, Xem chi tiết usecase) dựa trên quyền hạn được cấu hình. |
| **Ngoại lệ** | - Chương trình đánh giá không tồn tại hoặc người dùng không có quyền xem chương trình đó (không có quyền `EVALUATION_PROGRAM_MANAGEMENT` - `VIEW`) → lỗi "Bạn không có quyền truy cập thông tin chương trình đánh giá này." và chặn truy cập. |
| **Yêu cầu nghiệp vụ** | - UI hiển thị phân cấp dạng cây (Accordion) theo Mục tiêu đánh giá -> Tiêu chí -> Danh sách Usecase.<br>- Kiểm tra quyền người dùng để quyết định hiển thị/cho phép thao tác các nút: Cung cấp sở cứ, Đánh giá đơn vị, Xử lý lỗi.<br>- **Quy tắc hiển thị thông tin theo đơn vị phân bổ (QUAN TRỌNG)**:<br>  + Người dùng chỉ được xem thông tin, dữ liệu thống kê, và kết quả tính toán của các đơn vị mà mình được phân bổ.<br>  + **Đầu mối điều phối**: Xem tất cả các đơn vị thuộc chương trình đánh giá.<br>  + **Đầu mối đánh giá (Chuyên gia)** & **Đầu mối đơn vị**: Chỉ xem các đơn vị được phân bổ cho tài khoản của mình.<br>- Hiển thị trạng thái đánh giá thủ công của từng đơn vị được phép truy cập ở Sidebar cột trái.<br>- Cho phép lọc và tìm kiếm nhanh đơn vị. |


## 2. Luồng xử lý chi tiết (các bước)


### Bước 1: Người dùng truy cập chức năng - hệ thống kiểm tra điều kiện
* **Mô tả**: Khi người dùng click chọn xem chi tiết một chương trình đánh giá từ màn hình danh sách chương trình, hệ thống tiến hành kiểm tra các điều kiện truy cập:
  * Tài khoản người dùng phải có quyền xem chi tiết chương trình đánh giá (kiểm tra mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `VIEW`).
  * Kiểm tra sự tồn tại của chương trình đánh giá trong cơ sở dữ liệu (bảng `evaluation_program`).
* **Xác định phạm vi đơn vị được truy cập**:
  * Hệ thống xác định vai trò của tài khoản đối với chương trình đánh giá để lọc dữ liệu hiển thị:
    * **Đầu mối điều phối**: Được phép xem tất cả đơn vị thuộc chương trình (truy vấn toàn bộ bản ghi trong bảng `program_department_mapping` tương ứng với ID chương trình).
    * **Đầu mối đánh giá (Chuyên gia)**: Chỉ được phép xem các đơn vị mình được phân bổ đánh giá (lọc trong bảng `program_department_mapping` theo `reviewer` là ID tài khoản người dùng hiện tại).
    * **Đầu mối đơn vị**: Chỉ được phép xem đơn vị mình được phân bổ (lọc trong bảng `program_department_mapping` theo `sender` hoặc qua liên kết tài khoản đại diện đơn vị).
* **Kết quả kiểm tra**:
  * Nếu không hợp lệ hoặc người dùng không được phân bổ đơn vị nào trong chương trình → Hệ thống hiển thị thông báo lỗi và chặn không cho truy cập vào màn hình.
  * Nếu hợp lệ → Cho phép người dùng truy cập màn hình Xem chi tiết chương trình đánh giá và chỉ hiển thị dữ liệu của các đơn vị nằm trong phạm vi được truy cập.


### Bước 2: Hiển thị giao diện chi tiết chương trình đánh giá theo từng role
* **Khối thông tin chung**:
  * Lấy thông tin Tên chương trình, Trạng thái chương trình, Mô tả, Thời gian từ bảng `evaluation_program`.
  * **Đầu mối điều phối**: Lấy danh sách email đầu mối liên kết từ bảng người dùng thông qua phân quyền chương trình.
  * **Đơn vị tạo**: Hiển thị đơn vị của đầu mối điều phối.
  * **Số đơn vị hoàn thành**: Đếm số bản ghi trong bảng `program_department_mapping` có trạng thái đánh giá thủ công `manual_review_status = 2` (Hoàn thành) trên tổng số đơn vị được cấu hình trong chương trình **mà người dùng được phép truy cập** (theo kết quả lọc ở Bước 1).
  * **Số usecase đạt/không đạt/lỗi/không có kết quả** và **Tỷ lệ hoàn thành**: Tổng hợp dữ liệu kết quả đánh giá từ bảng `rule_run` (đối với usecase tự động, lấy bản ghi mới nhất) và bảng `rule_manual_review_mapping` (đối với usecase thủ công, lấy bản ghi mới nhất) **chỉ của các đơn vị thuộc phạm vi được truy cập của người dùng**.
* **Các nút chức năng chính**:
  * Hệ thống kiểm tra quyền hạn của người dùng hiện tại để hiển thị các nút thao tác tương ứng:
    * **Nút "Cung cấp sở cứ"**: Chỉ hiển thị và cho thao tác khi người dùng có quyền nộp sở cứ (kiểm tra mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `SEND`) và đơn vị đang chọn ở trạng thái hợp lệ (`0` hoặc `3`).
    * **Nút "Đánh giá đơn vị"**: Chỉ hiển thị và cho thao tác khi người dùng có quyền đánh giá (kiểm tra mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `REVIEW`) khi có ít nhất `1` đơn vị được truy cập ở trạng thái hợp lệ chờ đánh giá/ chờ đánh giá lại (`1` hoặc `4`).
    * **Nút "Xử lý lỗi"**: Chỉ hiển thị và cho thao tác khi người dùng có quyền xử lý lỗi (kiểm tra mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `RESOLVE_ERROR`) có ít nhất `1` đơn vị trong chương trình (thuộc phạm vi được truy cập) ở trạng thái đánh giá tự động Lỗi.
* **Khối phạm vi đánh giá**:
  * **Đơn vị được cấu hình**: Đếm tổng số đơn vị được phép truy cập của người dùng tham gia trong bảng `program_department_mapping` lọc theo ID chương trình.
  * **Tổng số usecase, Usecase thủ công, Usecase tự động**: Tổng hợp số lượng Usecase được cấu hình cho các đơn vị được truy cập trong chương trình thông qua bảng liên kết `criteria_rule_mapping` và phân loại dựa trên loại Usecase (bảng `rule.type`).
* **Danh sách đơn vị (cột trái)**:
  * Hiển thị danh sách các đơn vị được phép truy cập được cấu hình trong chương trình (truy vấn từ bảng `program_department_mapping` liên kết với bảng `department`, lọc theo phạm vi được truy cập xác định ở Bước 1).
  * Đơn vị đầu tiên trong danh sách sẽ được chọn mặc định để hiển thị chi tiết ở cột bên phải.
* **Chi tiết đơn vị đang chọn (cột phải)**:
  * Hiển thị Tên đơn vị đang chọn ở tiêu đề.
  * Thông tin đầu mối, đối tượng đánh giá được lấy từ bảng `program_department_mapping`, `department_representative_mapping`, và `department_object_mapping`.
  * Thống kê kết quả của riêng đơn vị đang chọn (Hoàn thành, Đạt, Không đạt, Lỗi) đếm dựa trên kết quả đánh giá các usecase thuộc đơn vị này.
  * **Danh sách Usecase phân cấp**:
    * Phân nhóm lớn nhất theo **Mục tiêu đánh giá** (lấy từ bảng `evaluation_object`).
    * Trong mỗi mục tiêu đánh giá, phân nhóm con theo **Tiêu chí** (lấy từ bảng `evaluation_criteria` thông qua bảng liên kết `object_criteria_mapping`).
    * Trong mỗi tiêu chí, hiển thị bảng danh sách các Usecase tương ứng (truy vấn từ bảng `rule` qua bảng liên kết `criteria_rule_mapping`) với các cột thông tin:
      * **Loại usecase**: Tự động hoặc Thủ công (trường `type` trong bảng `rule`).
      * **Tên usecase**: Tên của usecase (trường `name` trong bảng `rule`).
      * **Mục tiêu đánh giá**: Dạng nhãn hiển thị.
      * **Phiên bản**: Số phiên bản của usecase (trường `version` trong bảng `rule`).
      * **Trạng thái**: Trạng thái đánh giá hiện tại của usecase (đối với usecase tự động lấy từ bảng `rule_run` mới nhất; đối với usecase thủ công lấy từ bảng `rule_manual_review_mapping` mới nhất). Các trạng thái hiển thị: "Hoàn thành", "Lỗi", "Yêu cầu bổ sung".
      * **Kết quả**: "Đạt" hoặc "Không đạt" hoặc "không có" (đối với usecase tự động lấy từ cột `result` của bản ghi `rule_run` mới nhất; đối với usecase thủ công lấy từ cột `result` của bản ghi `rule_manual_review_mapping` mới nhất).
      * **Thao tác**: Hiển thị nút "Xem chi tiết usecase" (icon hình mắt/tài liệu) cho tất cả các usecase.


### Bước 3: Người dùng tương tác trên màn hình
* **Tìm kiếm đơn vị**: Người dùng nhập từ khóa vào ô "Tìm kiếm đơn vị" ở Sidebar cột trái. Frontend tự động lọc danh sách đơn vị hiển thị khớp với từ khóa nhập vào (không gọi lại DB).
* **Click chọn đơn vị**: Người dùng click chọn một đơn vị khác ở danh sách Sidebar. Giao diện cột bên phải tự động chuyển sang hiển thị chi tiết thông tin và danh sách Usecase của đơn vị mới được chọn.
* **Đóng/Mở các nhóm Mục tiêu/Tiêu chí**: Người dùng click vào icon mũi tên bên cạnh tên Mục tiêu đánh giá hoặc Tiêu chí để ẩn/hiện danh sách usecase bên dưới (thao tác giao diện).
* **Click xem chi tiết Usecase**:
  * Người dùng click vào icon xem chi tiết ở cột thao tác của Usecase.
  * Hệ thống hiển thị Modal/Popup chi tiết kết quả đánh giá của Usecase đó:
    * **Đối với Usecase tự động**: Hiển thị Modal/Popup **Chi tiết Usecase** (như mockup thiết kế) theo 2 kịch bản trạng thái:
      * **Kịch bản 1: Khi Usecase tự động có trạng thái "Hoàn thành"** (Mockup 1):
        * **Khối thông tin chung**:
          * **Mã usecase**: Lấy từ `rule.code` (Ví dụ: `UC115`).
          * **Tên usecase**: Lấy từ `rule.name` (Ví dụ: `Usecase 01 - Đánh giá năng lực sử dụng`).
          * **Phiên bản**: Lấy từ `rule.version` (Ví dụ: `1`).
          * **Trạng thái**: Nhãn màu xanh hiển thị `Hoàn thành` (lấy từ `rule_run.status` mới nhất).
          * **Kết quả usecase**: Lấy từ `rule_run.result` mới nhất (Ví dụ: `Đạt`).
        * **Khối kết quả**:
          * **Tên bước**: Tên bước cấu hình của rule (Ví dụ: `step01usecase1`).
          * **Thông tin đơn vị**: Tên đơn vị cấu hình (Ví dụ: `dept`).
          * **Thông tin kết quả**: Tên trường kết quả (Ví dụ: `result`).
          * **Kết quả**: Nhãn kết quả chung (Ví dụ: `Đạt`).
        * **Thao tác điều khiển**: Chỉ hiển thị nút "Đóng" ở góc dưới bên phải hoặc nút X ở góc trên bên phải để tắt Modal.
      * **Kịch bản 2: Khi Usecase tự động có trạng thái "Lỗi"** (Mockup 2):
        * **Khối thông tin chung**:
          * **Mã usecase**: Lấy từ `rule.code` (Ví dụ: `UC115`).
          * **Tên usecase**: Lấy từ `rule.name` (Ví dụ: `Usecase 01 - Đánh giá năng lực sử dụng`).
          * **Phiên bản**: Lấy từ `rule.version` (Ví dụ: `1`).
          * **Trạng thái**: Nhãn màu đỏ hiển thị `Lỗi` (lấy từ `rule_run.status` mới nhất).
          * *(Không hiển thị trường Kết quả usecase trong khối này)*.
        * **Khối kết quả**:
          * **Tên bước**: Tên bước cấu hình của rule (Ví dụ: `step01usecase1`).
          * **Thông tin đơn vị**: Tên đơn vị cấu hình (Ví dụ: `dept`).
          * **Thông tin kết quả**: Tên trường kết quả (Ví dụ: `result`).
          * *(Không hiển thị trường Kết quả chung trong khối này)*.
        * **Khối chi tiết kết quả**:
          * Hiển thị lại các metadata: **Tên bước** (`step01usecase1`), **Thông tin đơn vị** (`dept`), **Thông tin kết quả** (`result`).
          * **Bảng chi tiết dữ liệu lỗi**:
            * Cột `#`: Số thứ tự.
            * Cột `Tên bước`: Tên bước bị lỗi (Ví dụ: `Bước 1`, `Bước 2`, ...).
            * Cột `Thông tin đơn vị`: Đơn vị tương ứng bị lỗi (Ví dụ: `Đơn vị 1`, `Đơn vị 2`, ...).
            * Phần phân trang ở dưới bảng cho phép điều chỉnh số lượng bản ghi hiển thị (mặc định hiển thị 5 dòng), nút chuyển trang, và tổng số bản ghi lỗi (Ví dụ: `Hiển thị 1 - 5 trên 100`).
        * **Thao tác điều khiển**:
          * Nút "Đóng" ở góc dưới bên phải hoặc nút X ở góc trên bên phải để tắt Modal.
          * **Nút "Chạy lại"** (màu đỏ): Kích hoạt chạy lại tiến trình đánh giá tự động cho Usecase này. Chỉ xuất hiện ở trạng thái Lỗi.
    * **Đối với Usecase thủ công**: Hiển thị Modal/Popup **Chi tiết Usecase** (như mockup thiết kế) gồm các phần:
      * **Tiêu đề popup**: "CHI TIẾT USECASE" kèm icon đóng (X) ở góc trên bên phải.
      * **Khối thông tin chung**:
        * **Mã usecase**: Lấy từ `rule.code` (Ví dụ: `UC115`).
        * **Tên usecase**: Lấy từ `rule.name` (Ví dụ: `Usecase 01 - Đánh giá năng lực sử dụng`).
        * **Phiên bản**: Lấy từ `rule.version` (Ví dụ: `1`).
        * **Kết quả usecase**: Lấy trạng thái từ `rule_manual_review_mapping.status` hoặc `result` của bản ghi mới nhất (Ví dụ: `Hoàn thành`).
        * **Người đánh giá**: Tên/Username tài khoản chuyên gia đánh giá từ `rule_manual_review_mapping.reviewer` liên kết bảng `user` (Ví dụ: `hoangnv1710`).
      * **Danh sách sở cứ**:
        * Hiển thị danh sách các file sở cứ đã nộp của Usecase từ bảng `rule_review_file` liên kết với `rule_manual_review_mapping` (qua `rule_review_id`).
        * Mỗi file hiển thị gồm: Icon loại file tương ứng (PDF, DOCX, XLSX, v.v.), Tên file đầy đủ (`file_name`), Dung lượng file và ngày giờ tải lên (`created_at`) (Ví dụ: `1.2 MB - 20/5/2026`), và Icon Tải xuống (Download) ở góc phải dòng file cho phép người dùng click tải file về.
      * **Thao tác điều khiển**:
        * Nút "Đóng" ở góc dưới bên phải hoặc nút X ở góc trên bên phải cho phép thoát Modal quay lại màn hình Xem chi tiết đánh giá.


### Bước 4: Hệ thống xử lý nghiệp vụ khi click nút chức năng khác
* **Click nút "Cung cấp sở cứ"**:
  * **Kiểm tra quyền**: Người dùng phải được gán quyền cung cấp sở cứ (kiểm tra mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `SEND`) mới hiển thị và được tương tác với nút này.
  * **Kiểm tra trạng thái đơn vị**: Hệ thống kiểm tra trạng thái đơn vị đang chọn ở cột phải:
    * Nếu là `0` (Chưa gửi sở cứ) → Chuyển hướng sang màn hình Cung cấp sở cứ (Primary).
    * Nếu là `3` (Yêu cầu bổ sung) → Chuyển hướng sang màn hình Cung cấp sở cứ bổ sung.
* **Click nút "Đánh giá đơn vị"**:
  * **Kiểm tra quyền**: Người dùng phải có vai trò `Chuyên gia` và được gán quyền đánh giá đơn vị (kiểm tra mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `REVIEW`) mới hiển thị và được tương tác với nút này.
  * **Kiểm tra trạng thái đơn vị**: Hệ thống kiểm tra trạng thái đơn vị đang chọn ở cột phải:
    * Nếu là `1` (Chờ đánh giá) hoặc `4` (Chờ đánh giá bổ sung) → Chuyển hướng sang màn hình Đánh giá đơn vị.
* **Click nút "Xử lý lỗi"**:
  * **Kiểm tra quyền**: Người dùng phải có quyền xử lý lỗi (kiểm tra mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `RESOLVE_ERROR`) mới hiển thị và được tương tác với nút này.
  * **Kiểm tra trạng thái đơn vị**: Chỉ hiển thị khi đơn vị đang chọn có ít nhất một Usecase tự động có trạng thái là `Lỗi`.
  * **Hành động**: Khi click, hiển thị popup/giao diện xử lý lỗi cho các usecase bị lỗi của đơn vị.
* **Click nút "Đóng"**:
  * Hệ thống thoát khỏi màn hình Xem chi tiết chương trình đánh giá và điều hướng người dùng quay trở lại màn hình danh sách chương trình đánh giá.


---


## 3. Thông tin chi tiết thành phần màn hình


| STT | Tên phần tử giao diện | Kiểu dữ liệu | I/O | Giá trị khởi tạo | Mô tả chi tiết (Mapping CSDL & Thao tác Button) |
|---|---|---|---|---|---|
| 1 | Tiêu đề màn hình | string | Output | "Xem chi tiết chương trình đánh giá" | Hiển thị tên chương trình đánh giá đang xem.<br>- **Mapping CSDL**: `evaluation_program.name` |
| 2 | Nút "Cung cấp sở cứ" | button | Input/Output | Active khi đơn vị chọn hợp lệ | Cho phép chuyển sang màn hình nộp sở cứ.<br>- **Kiểm tra quyền**: Phải có quyền nộp sở cứ (kiểm tra mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `SEND`). Nếu không thỏa mãn $\rightarrow$ Ẩn nút.<br>- **Thao tác Button**: Khi click, hệ thống kiểm tra trạng thái đơn vị đang chọn ở cột phải:<br>  + Nếu là `0` (Chưa gửi sở cứ) $\rightarrow$ Điều hướng sang màn hình **Cung cấp sở cứ (Primary)**.<br>  + Nếu là `3` (Yêu cầu bổ sung) $\rightarrow$ Điều hướng sang màn hình **Cung cấp sở cứ bổ sung**.<br>  + Các trạng thái khác $\rightarrow$ Vô hiệu hóa (Disable) nút này. |
| 3 | Nút "Đánh giá đơn vị" | button | Input/Output | Active khi đơn vị chọn hợp lệ | Cho phép chuyển sang màn hình chấm điểm của chuyên gia.<br>- **Kiểm tra quyền**: Phải có vai trò `Chuyên gia` và quyền đánh giá (kiểm tra mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `REVIEW`) khi có ít nhất 1 đơn vị được truy cập ở trạng thái hợp lệ chờ đánh giá/chờ đánh giá lại (`1` hoặc `4`). Nếu không thỏa mãn $\rightarrow$ Ẩn nút.<br>- **Thao tác Button**: Khi click, hệ thống kiểm tra trạng thái đơn vị đang chọn ở cột phải:<br>  + Nếu là `1` (Chờ đánh giá) hoặc `4` (Chờ đánh giá bổ sung) $\rightarrow$ Điều hướng sang màn hình **Đánh giá đơn vị**.<br>  + Các trạng thái khác $\rightarrow$ Vô hiệu hóa (Disable) nút này. |
| 4 | Nút "Xử lý lỗi" | button | Input/Output | Active khi đơn vị chọn hợp lệ | Cho phép mở giao diện xử lý lỗi Usecase tự động.<br>- **Kiểm tra quyền**: Phải có quyền xử lý lỗi (kiểm tra mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `RESOLVE_ERROR`) khi có ít nhất 1 đơn vị trong chương trình (thuộc phạm vi được truy cập) ở trạng thái đánh giá tự động Lỗi. Nếu không thỏa mãn $\rightarrow$ Ẩn nút.<br>- **Thao tác Button**: Chỉ cho phép click khi đơn vị đang chọn có ít nhất một Usecase tự động có trạng thái là `Lỗi`. Khi click, hiển thị popup/giao diện xử lý lỗi cho các usecase bị lỗi của đơn vị. |
| 5 | Nút "Đóng" | button | Input | - | Thoát khỏi giao diện chi tiết.<br>- **Thao tác Button**: Khi click, điều hướng người dùng quay trở lại màn hình Danh sách chương trình đánh giá (mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `SEARCH`). |
| 6 | Thông tin chung chương trình | object (labels) | Output | Lấy theo chương trình đang chọn | Hiển thị thông tin cơ bản của chương trình:<br>- **Tên chương trình**: `evaluation_program.name`<br>- **Trạng thái**: `evaluation_program.status` (nhãn hiển thị)<br>- **Mô tả**: `evaluation_program.description`<br>- **Thời gian**: `evaluation_program.start_date` - `evaluation_program.end_date` |
| 7 | Đầu mối điều phối | list of badges | Output | - | Hiển thị danh sách email các điều phối viên.<br>- **Mapping CSDL**: Truy vấn từ bảng liên kết trung gian giữa `evaluation_program` và bảng người dùng `user.email`. |
| 8 | Đơn vị tạo | label | Output | - | Hiển thị đơn vị của đầu mối điều phối chính của chương trình.<br>- **Mapping CSDL**: Lấy thông tin đơn vị từ bảng người dùng (`user`) liên kết với đầu mối điều phối của chương trình. |
| 9 | Số đơn vị hoàn thành | label | Output | "0/0" | Thống kê số đơn vị hoàn thành đánh giá thủ công mà người dùng có quyền truy cập.<br>- **Mapping CSDL**: Đếm số bản ghi trong bảng `program_department_mapping` có `manual_review_status = 2` (Hoàn thành) chia cho tổng số bản ghi đơn vị được cấu hình trong chương trình mà tài khoản hiện tại được phép truy cập (lọc theo quy tắc phân bổ). |
| 10 | Số usecase đạt / không đạt / lỗi / không có kết quả | label | Output | "0", "0", "0", "0" | Thống kê tổng số kết quả usecase của tất cả các đơn vị thuộc chương trình mà người dùng được phép truy cập (lọc theo quy tắc phân bổ).<br>- **Mapping CSDL**: Tổng hợp kết quả từ bản ghi `rule_run.result` mới nhất (đối với UC tự động) và bản ghi `rule_manual_review_mapping.result` mới nhất (đối với UC thủ công). |
| 11 | Tỷ lệ hoàn thành | progress bar | Output | 0% | Thanh tiến trình hoàn thành đánh giá của chương trình.<br>- **Mapping CSDL**: Tính bằng tỷ lệ `%` giữa số lượng Usecase đã có kết quả đánh giá (lấy từ các bản ghi `rule_run` mới nhất đối với UC tự động và `rule_manual_review_mapping` mới nhất đối với UC thủ công) trên tổng số Usecase của các đơn vị thuộc phạm vi truy cập của tài khoản hiện tại. |
| 12 | Ô "Tìm kiếm đơn vị" | input text | Input | "" | Ô nhập từ khóa lọc nhanh danh sách đơn vị.<br>- **Thao tác**: Nhập ký tự $\rightarrow$ Frontend tự động ẩn/hiện các dòng đơn vị ở Sidebar khớp với từ khóa (không gọi lại DB). |
| 13 | Danh sách đơn vị đánh giá (Sidebar cột trái) | list of items | Output | - | Hiển thị danh sách đơn vị được phép truy cập được cấu hình trong chương trình và nhãn trạng thái đánh giá thủ công tương ứng.<br>- **Mapping CSDL**: Lấy tên đơn vị từ `department.name` thông qua `program_department_mapping.dept_id`, **chỉ hiển thị các đơn vị mà người dùng được phân bổ** (ngoại trừ Đầu mối điều phối được xem tất cả). Nhãn trạng thái bên cạnh tên đơn vị lấy từ `program_department_mapping.manual_review_status` (0: Chưa nộp, 1: Chờ đánh giá, 2: Hoàn thành, 3: Yêu cầu bổ sung, 4: Chờ đánh giá bổ sung).<br>- **Thao tác**: Người dùng click chọn một đơn vị $\rightarrow$ Kích hoạt tải dữ liệu chi tiết của đơn vị đó hiển thị ở cột bên phải. |
| 14 | Chi tiết đơn vị đang chọn (Metadata cột phải) | object (labels) | Output | - | Hiển thị thông tin tổng hợp của đơn vị đang chọn:<br>- **Đầu mối đánh giá đơn vị**: `program_department_mapping.reviewer` liên kết `user.name`.<br>- **Đầu mối đơn vị**: `program_department_mapping.sender` liên kết `user.name`.<br>- **Đối tượng đánh giá**: Tên đối tượng từ `evaluation_object.name` (qua `department_object_mapping`).<br>- **Hoàn thành / Đạt / Không đạt / Lỗi / Không có**: Thống kê số lượng usecase tương tự trường số 10, nhưng chỉ lọc riêng cho đơn vị đang chọn. |
| 15 | Accordion phân nhóm Usecase | accordion | Output | - | Cấu trúc cây thu gọn/mở rộng theo phân cấp Mục tiêu và Tiêu chí.<br>- **Mục tiêu đánh giá (Nhóm cha)**: Lấy từ `evaluation_object.name`.<br>- **Tiêu chí đánh giá (Nhóm con)**: Lấy từ `evaluation_criteria.name` (liên kết qua `object_criteria_mapping`).<br>- **Thao tác**: Click vào tiêu đề nhóm để thu gọn/mở rộng danh sách usecase bên dưới. |
| 16 | Danh sách Usecase (Bảng chi tiết) | table | Output | - | Bảng danh sách hiển thị các usecase thuộc tiêu chí của đơn vị được chọn:<br>- **Loại usecase**: `rule.type` (Tự động/Thủ công).<br>- **Tên usecase**: `rule.name`.<br>- **Mục tiêu đánh giá**: badge hiển thị nhãn mục tiêu đánh giá liên kết.<br>- **Phiên bản**: Phiên bản cấu hình.<br>- **Trạng thái**: `rule_run.status` của bản ghi mới nhất (đối với UC tự động) hoặc `rule_manual_review_mapping.status` của bản ghi mới nhất (đối với UC thủ công) - Nhãn hiển thị: "Hoàn thành", "Lỗi", "Yêu cầu bổ sung".<br>- **Kết quả**: `rule_run.result` của bản ghi mới nhất (đối với UC tự động) hoặc `rule_manual_review_mapping.result` của bản ghi mới nhất (đối với UC thủ công) - Nhãn hiển thị: "Đạt", "Không đạt", "không có". |
| 17 | Nút "Xem chi tiết" (Icon 👁️) | icon button | Input | - | Cho phép xem chi tiết lịch sử và sở cứ đánh giá.<br>- **Thao tác Button**: Khi click, hiển thị Modal Popup:<br>  + Đối với UC tự động: Hiển thị Modal "Chi tiết Usecase" chứa **Thông tin chung** (Mã UC, Tên UC, Phiên bản, Trạng thái, Kết quả), **Khối kết quả** (Tên bước, Thông tin đơn vị, Thông tin kết quả, Kết quả). Nếu trạng thái là **Lỗi**, hiển thị thêm **Bảng chi tiết dữ liệu lỗi** (phân trang) và nút **Chạy lại** để chạy lại đánh giá tự động.<br>  + Đối với UC thủ công: Hiển thị Modal "Chi tiết Usecase" chứa **Thông tin chung** (Mã UC, Tên UC, Phiên bản, Kết quả, Người đánh giá) và **Danh sách sở cứ** (các file đính kèm từ `rule_review_file` cùng với nút tải xuống cho mỗi file). |


---


## 4. Thông tin chi tiết thành phần Modal Chi tiết Usecase thủ công


| STT | Tên phần tử giao diện | Kiểu dữ liệu | I/O | Giá trị khởi tạo | Mô tả chi tiết (Mapping CSDL & Thao tác Button) |
|---|---|---|---|---|---|
| 1 | Tiêu đề popup | string | Output | "CHI TIẾT USECASE" | Tiêu đề của Modal Popup. |
| 2 | Nút "X" (Đóng) | icon button | Input | - | Cho phép đóng modal.<br>- **Thao tác**: Click $\rightarrow$ Đóng Modal quay lại màn hình Xem chi tiết. |
| 3 | Mã usecase | string | Output | Theo Usecase đang chọn | Hiển thị mã usecase.<br>- **Mapping CSDL**: `rule.code` |
| 4 | Tên usecase | string | Output | Theo Usecase đang chọn | Hiển thị tên đầy đủ của usecase.<br>- **Mapping CSDL**: `rule.name` |
| 5 | Phiên bản | string | Output | Theo Usecase đang chọn | Phiên bản cấu hình.<br>- **Mapping CSDL**: `rule.version` |
| 6 | Kết quả usecase | string (badge) | Output | Theo Usecase đang chọn | Trạng thái/Kết quả đánh giá thủ công hiện tại.<br>- **Mapping CSDL**: `rule_manual_review_mapping.status` hoặc `result` của bản ghi mới nhất. |
| 7 | Người đánh giá | string | Output | Theo Usecase đang chọn | Username chuyên gia chấm điểm.<br>- **Mapping CSDL**: `rule_manual_review_mapping.reviewer` liên kết `user.username` (hoặc `user.name`). |
| 8 | Danh sách sở cứ | list of items | Output | - | Hiển thị danh sách các file tài liệu đính kèm đã nộp.<br>- **Mapping CSDL**: Danh sách các file trong `rule_review_file` liên kết với `rule_manual_review_mapping` thông qua `rule_review_id` của bản ghi mới nhất. Mỗi file hiển thị: Tên file (`file_name`), Dung lượng file và ngày tải lên (`created_at`). |
| 9 | Nút "Tải xuống" (Icon 📥) | icon button | Input | - | Nút tải xuống nằm ở góc phải mỗi hàng file sở cứ.<br>- **Thao tác**: Click $\rightarrow$ Kích hoạt tải file sở cứ về máy local. |
| 10 | Nút "Đóng" | button | Input | - | Nút đóng popup nằm ở góc dưới bên phải.<br>- **Thao tác**: Click $\rightarrow$ Đóng Modal quay lại màn hình Xem chi tiết. |


---


## 5. Thông tin chi tiết thành phần Modal Chi tiết Usecase tự động


| STT | Tên phần tử giao diện | Kiểu dữ liệu | I/O | Giá trị khởi tạo | Mô tả chi tiết (Mapping CSDL & Thao tác Button) |
|---|---|---|---|---|---|
| 1 | Tiêu đề popup | string | Output | "CHI TIẾT USECASE" | Tiêu đề của Modal Popup. |
| 2 | Nút "X" (Đóng) | icon button | Input | - | Cho phép đóng modal.<br>- **Thao tác**: Click $\rightarrow$ Đóng Modal quay lại màn hình Xem chi tiết. |
| 3 | Mã usecase | string | Output | Theo Usecase đang chọn | Hiển thị mã usecase.<br>- **Mapping CSDL**: `rule.code` |
| 4 | Tên usecase | string | Output | Theo Usecase đang chọn | Hiển thị tên đầy đủ của usecase.<br>- **Mapping CSDL**: `rule.name` |
| 5 | Phiên bản | string | Output | Theo Usecase đang chọn | Phiên bản cấu hình.<br>- **Mapping CSDL**: `rule.version` |
| 6 | Trạng thái | string (badge) | Output | Theo Usecase đang chọn | Trạng thái chạy của rule tự động.<br>- **Mapping CSDL**: `rule_run.status` của bản ghi mới nhất (Hoàn thành / Lỗi). |
| 7 | Kết quả usecase | string | Output | Theo Usecase đang chọn | Kết quả đánh giá tự động.<br>- **Mapping CSDL**: `rule_run.result` của bản ghi mới nhất (Đạt / Không đạt).<br>- **Quy tắc hiển thị**: Chỉ hiển thị trong khối *Thông tin chung* khi Trạng thái là "Hoàn thành". |
| 8 | Tên bước (Khối Kết quả) | string | Output | - | Tên bước cấu hình trong rule.<br>- **Mapping CSDL**: Cấu hình bước từ rule tự động. |
| 9 | Thông tin đơn vị (Khối Kết quả) | string | Output | - | Tên trường đơn vị cấu hình trong rule.<br>- **Mapping CSDL**: Cấu hình rule. |
| 10 | Thông tin kết quả (Khối Kết quả) | string | Output | - | Tên trường kết quả cấu hình trong rule.<br>- **Mapping CSDL**: Cấu hình rule. |
| 11 | Kết quả (Khối Kết quả) | string | Output | Theo Usecase đang chọn | Kết quả chung của Usecase.<br>- **Mapping CSDL**: `rule_run.result` mới nhất (Đạt / Không đạt).<br>- **Quy tắc hiển thị**: Chỉ hiển thị trong khối *Kết quả* khi Trạng thái là "Hoàn thành". |
| 12 | Bảng chi tiết kết quả lỗi | table | Output | - | Bảng danh sách chi tiết các bước lỗi.<br>- **Quy tắc hiển thị**: Chỉ hiển thị khi Trạng thái là "Lỗi".<br>- **Thành phần bảng**: <br>  + Cột `#` (STT)<br>  + Cột `Tên bước` (ví dụ: `Bước 1`, `Bước 2`...)<br>  + Cột `Thông tin đơn vị` (tên đơn vị lỗi)<br>  + Phân trang dưới bảng (Số bản ghi/trang, Chuyển trang, Hiển thị dòng/Tổng số). |
| 13 | Nút "Đóng" | button | Input | - | Nút đóng popup nằm ở góc dưới bên phải.<br>- **Thao tác**: Click $\rightarrow$ Đóng Modal quay lại màn hình Xem chi tiết. |
| 14 | Nút "Chạy lại" | button | Input | - | Nút chạy lại tiến trình đánh giá tự động, màu đỏ, nằm cạnh nút Đóng.<br>- **Quy tắc hiển thị**: Chỉ hiển thị khi Trạng thái là "Lỗi" và người dùng có quyền tương tác.<br>- **Thao tác**: Click $\rightarrow$ Hệ thống kích hoạt chạy lại đánh giá tự động cho Usecase này. |


---


*End of Document*




*End of Document*



