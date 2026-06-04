# Xem danh sách và tìm kiếm chương trình đánh giá – Thông tin chức năng & Luồng xử lý


## 1. Thông tin chức năng (Bảng 2 cột)


| Thuộc tính | Nội dung |
|---|---|
| **Tên chức năng** | Xem danh sách và tìm kiếm chương trình đánh giá |
| **Mã chức năng** | `EVALUATION_PROGRAM_MANAGEMENT` |
| **Mã thao tác** | - Tìm kiếm / Xem danh sách: `SEARCH`<br>- Thêm mới: `CREATE`<br>- Chỉnh sửa cấu hình: `EDIT`<br>- Xóa chương trình: `DELETE`<br>- Xem chi tiết: `VIEW`<br>- Bắt đầu đánh giá: `START`<br>- Hủy đánh giá: `CANCEL`<br>- Xử lý lỗi: `RESOLVE_ERROR`<br>- Hoàn thành đánh giá: `COMPLETE` |
| **Mô tả** | Cung cấp giao diện trung tâm hiển thị danh sách các chương trình đánh giá của hệ thống. Cho phép người dùng tìm kiếm, lọc theo trạng thái (qua các tab-menu), lọc nâng cao và truy cập nhanh các hành động (chỉnh sửa, xóa, xem chi tiết, bắt đầu đánh giá, hủy đánh giá, xử lý lỗi, hoàn thành đánh giá, đánh giá đơn vị, cung cấp sở cứ) dựa trên vai trò của tài khoản hiện tại. |
| **Tác nhân** | Quản trị hệ thống (Admin), đầu mối điều phối, đầu mối đánh giá, đầu mối đơn vị |
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
  * *đầu mối điều phối*: Chương trình ở trạng thái Đang đánh giá (`status = 1`) cần hoàn thành hoặc có lỗi tự động.
  * *đầu mối đánh giá*: Chương trình có đơn vị ở trạng thái chờ đánh giá/đánh giá lại (`manual_review_status IN (1, 4)`).
* **Tab "Chờ sở cứ"**:
  * *đầu mối đơn vị*: Chương trình ở trạng thái Đang đánh giá (`status = 1`) và đơn vị của mình đang ở trạng thái chưa nộp sở cứ hoặc bị từ chối nộp lại (`manual_review_status IN (0, 3)`).


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
| **đầu mối điều phối** | Bắt đầu đánh giá | `evaluation_program.program_auditor = currentUserId` AND `evaluation_program.status = 0` AND có quyền `START` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Bắt đầu thực hiện chương trình đánh giá | Tất cả, Chờ duyệt |
| **đầu mối điều phối** | Hủy | `evaluation_program.program_auditor = currentUserId` AND `evaluation_program.status = 1` AND có quyền `CANCEL` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Hủy chương trình đánh giá | Tất cả |
| **đầu mối điều phối** | Xử lý lỗi | `evaluation_program.program_auditor = currentUserId` AND `evaluation_program.status = 1` AND tồn tại đơn vị có `auto_review_status = 3` AND có quyền `RESOLVE_ERROR` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Thực hiện xử lý các đánh giá tự động bị lỗi | Tất cả, Chờ duyệt |
| **đầu mối điều phối** | Hoàn thành đánh giá | `evaluation_program.program_auditor = currentUserId` AND `evaluation_program.status = 1` AND tất cả đơn vị có `manual_review_status = 2` AND `auto_review_status = 2` AND có quyền `COMPLETE` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Hoàn thành chương trình đánh giá | Tất cả, Chờ duyệt |
| **đầu mối điều phối** | Xem chi tiết | `evaluation_program.program_auditor = currentUserId` AND `evaluation_program.status IN (1, 2, 3)` AND có quyền `VIEW` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Xem thông tin cấu hình và tiến độ/kết quả đánh giá | Tất cả |
| **đầu mối đánh giá** | Đánh giá đơn vị | `department_reviewer_mapping.representative_id = currentUserId` AND `evaluation_program.status = 1` AND `manual_review_status IN (1, 4)` AND có quyền `REVIEW` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Thực hiện đánh giá hoặc đánh giá lại đơn vị | Tất cả, Chờ duyệt |
| **đầu mối đánh giá** | Xem chi tiết | `department_reviewer_mapping.representative_id = currentUserId` AND có quyền `VIEW` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Xem thông tin cấu hình và kết quả đánh giá | Tất cả |
| **đầu mối đơn vị** | Cung cấp sở cứ | `department_representative_mapping.representative_id = currentUserId` AND `evaluation_program.status = 1` AND `manual_review_status IN (0, 3)` AND có quyền `SEND` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Cung cấp hoặc bổ sung sở cứ đánh giá | Tất cả, Chờ sở cứ |
| **đầu mối đơn vị** | Xem chi tiết | `department_representative_mapping.representative_id = currentUserId` AND có quyền `VIEW` thuộc `EVALUATION_PROGRAM_MANAGEMENT` | Xem thông tin cấu hình và kết quả đánh giá | Tất cả |


---


## 4. Thông tin chi tiết thành phần màn hình


| STT | Tên phần tử giao diện | Kiểu dữ liệu | I/O | Giá trị khởi tạo | Mô tả chi tiết (Mapping CSDL & Thao tác Button) |
|---|---|---|---|---|---|
| 1 | Tiêu đề trang | string | Output | "Chương trình đánh giá" | Tiêu đề màn hình hiển thị. |
| 2 | Tab-menu "Tất cả" | tab button | Input/Output | Active | Khi click, hiển thị tất cả chương trình thuộc phạm vi được quyền xem. |
| 3 | Tab-menu "Chờ duyệt" | tab button | Input/Output | Hiển thị số lượng | Hiển thị số lượng chương trình cần xử lý của đầu mối điều phối hoặc đầu mối đánh giá.<br>- **Thao tác**: Click $\rightarrow$ Lọc danh sách chương trình đang cần đánh giá hoặc đang gặp lỗi. |
| 4 | Tab-menu "Chờ sở cứ" | tab button | Input/Output | Hiển thị số lượng | Hiển thị số lượng chương trình cần nộp sở cứ của đầu mối đơn vị.<br>- **Thao tác**: Click $\rightarrow$ Lọc danh sách chương trình đơn vị chưa hoàn thành gửi sở cứ. |
| 5 | Ô tìm kiếm chương trình | input text | Input | "" | Nhập từ khóa để lọc danh sách theo tên chương trình.<br>- **Thao tác**: Nhập dữ liệu và nhấn Enter $\rightarrow$ Hệ thống lọc theo `evaluation_program.name` (yêu cầu quyền mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `SEARCH`). |
| 6 | Nút bộ lọc nâng cao (Icon 🎛️) | icon button | Input | Hiển thị số lượng bộ lọc đang chọn | Click để mở popup các trường lọc phụ (thời gian, trạng thái, người tạo...). |
| 7 | Nút "+ Thêm mới" | button | Input/Output | Active nếu có quyền | Cho phép điều hướng sang màn hình thêm mới chương trình đánh giá.<br>- **Quy tắc hiển thị**: Chỉ hiển thị với vai trò Admin hoặc tài khoản có quyền khởi tạo chương trình (yêu cầu quyền mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `CREATE`). |
| 8 | Bảng danh sách chương trình | table | Output | - | Hiển thị thông tin tổng hợp của các chương trình đánh giá lấy từ DB (các cột thông tin được chi tiết ở Mục 2). |
| 9 | Nút nhanh "Xem chi tiết" (Icon 👁️) | icon button | Input | - | Nút thao tác nhanh trên từng dòng.<br>- **Thao tác**: Click $\rightarrow$ Chuyển hướng sang màn hình **Xem chi tiết chương trình đánh giá** (lọc theo điều kiện phân quyền và yêu cầu quyền đọc từ mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `VIEW`). |
| 10 | Nút nhanh "Chỉnh sửa" (Icon ✏️) | icon button | Input | - | Nút thao tác nhanh trên từng dòng.<br>- **Thao tác**: Chỉ hiển thị khi trạng thái chương trình là `0` (Chưa đánh giá) và người dùng có quyền chỉnh sửa (yêu cầu quyền mã chức năng `EVALUATION_PROGRAM_MANAGEMENT` và mã thao tác `EDIT` hoặc vai trò Admin). Click $\rightarrow$ Chuyển hướng sang màn hình chỉnh sửa cấu hình chương trình. |
| 11 | Menu ba chấm thả xuống (Dropdown actions) | dropdown menu | Input/Output | - | Chứa danh sách các nút hành động phụ.<br>- **Quy tắc hiển thị**: Tự động tính toán các nút con bên trong (các mục từ 11.1 đến 11.7 bên dưới) dựa vào **Ma trận hiển thị nút thao tác (Mục 3)** tương ứng với vai trò của người đăng nhập và trạng thái của chương trình trên dòng đó. |
| 11.1 | - Nút "Xóa" | menu item | Input | - | Cho phép xóa chương trình ở trạng thái `0` (Chưa đánh giá).<br>- **Thao tác**: Click $\rightarrow$ Hiển thị popup xác nhận xóa. Nếu chọn Đồng ý, thực hiện xóa bản ghi trong CSDL và làm mới danh sách (yêu cầu quyền `DELETE` thuộc `EVALUATION_PROGRAM_MANAGEMENT`). |
| 11.2 | - Nút "Bắt đầu đánh giá" | menu item | Input | - | Cho phép bắt đầu chạy chương trình đánh giá ở trạng thái `0` (Chưa đánh giá).<br>- **Thao tác**: Click $\rightarrow$ Hệ thống cập nhật `evaluation_program.status = 1` (Đang đánh giá) và chạy tự động các Usecase tự động (yêu cầu quyền `START` thuộc `EVALUATION_PROGRAM_MANAGEMENT` và tác nhân là đầu mối điều phối). |
| 11.3 | - Nút "Hủy" | menu item | Input | - | Cho phép hủy chương trình đang đánh giá (trạng thái `1`).<br>- **Thao tác**: Click $\rightarrow$ Hiển thị popup xác nhận hủy. Nếu chọn Đồng ý, hệ thống cập nhật `evaluation_program.status = 3` (Hủy đánh giá) (yêu cầu quyền `CANCEL` thuộc `EVALUATION_PROGRAM_MANAGEMENT` và tác nhân là đầu mối điều phối). |
| 11.4 | - Nút "Xử lý lỗi" | menu item | Input | - | Cho phép điều hướng sang giao diện xử lý các Usecase tự động bị lỗi.<br>- **Thao tác**: Click $\rightarrow$ Chuyển hướng sang màn hình xử lý lỗi (yêu cầu quyền `RESOLVE_ERROR` thuộc `EVALUATION_PROGRAM_MANAGEMENT` và tác nhân là đầu mối điều phối). |
| 11.5 | - Nút "Hoàn thành đánh giá" | menu item | Input | - | Cho phép xác nhận hoàn thành toàn bộ chương trình đánh giá.<br>- **Thao tác**: Click $\rightarrow$ Cập nhật trạng thái chương trình thành `status = 2` (Hoàn thành đánh giá) (yêu cầu quyền `COMPLETE` thuộc `EVALUATION_PROGRAM_MANAGEMENT` và tác nhân là đầu mối điều phối). |
| 11.6 | - Nút "Đánh giá đơn vị" | menu item | Input | - | Cho phép chuyên gia đánh giá thực hiện chấm điểm cho đơn vị được phân bổ.<br>- **Thao tác**: Click $\rightarrow$ Điều hướng sang màn hình **Đánh giá đơn vị** (yêu cầu quyền `REVIEW` thuộc `EVALUATION_PROGRAM_MANAGEMENT` và tác nhân là đầu mối đánh giá). |
| 11.7 | - Nút "Cung cấp sở cứ" | menu item | Input | - | Cho phép đại diện đơn vị nộp tài liệu minh chứng.<br>- **Thao tác**: Click $\rightarrow$ Điều hướng sang màn hình **Cung cấp sở cứ** (yêu cầu quyền `SEND` thuộc `EVALUATION_PROGRAM_MANAGEMENT` và tác nhân là đầu mối đơn vị). |
| 12 | Thanh phân trang | component | Input/Output | 10 bản ghi/trang | Cho phép chọn số lượng bản ghi hiển thị trên trang và điều khiển chuyển trang. |


---


## 5. Bảng phân quyền thao tác hệ thống (System Action Permission Table)


Dưới đây là bảng tổng hợp cấu trúc phân quyền chức năng và quyền thao tác (Feature - Action Permissions) cho toàn hệ thống nhằm đảm bảo tính đồng bộ giữa các tài liệu đặc tả:


| STT | Tên chức năng (Module) | Mã chức năng (Function Code) | Thao tác (Action) | Mã thao tác (Action Code) | Vai trò được phép (Role) | Mô tả chi tiết hành động kiểm tra |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **1** | **Chương trình đánh giá** | `EVALUATION_PROGRAM_MANAGEMENT` | Tìm kiếm / Xem danh sách | `SEARCH` | Admin, đầu mối điều phối, đầu mối đánh giá, đầu mối đơn vị | Xem danh sách chương trình, sử dụng bộ lọc tìm kiếm. |
| | | | Thêm mới | `CREATE` | Admin | Truy cập màn hình thêm mới chương trình đánh giá. |
| | | | Chỉnh sửa cấu hình | `EDIT` | Admin | Truy cập màn hình chỉnh sửa cấu hình chương trình ở trạng thái 0 (Chưa đánh giá). |
| | | | Xóa chương trình | `DELETE` | Admin | Thực hiện hành động xóa chương trình ở trạng thái 0. |
| | | | Xem chi tiết chương trình | `VIEW` | Admin, đầu mối điều phối, đầu mối đánh giá, đầu mối đơn vị | Xem chi tiết cấu hình và tiến độ/kết quả đánh giá của chương trình. |
| | | | Bắt đầu đánh giá | `START` | đầu mối điều phối | Chuyển trạng thái chương trình thành 1 (Đang đánh giá). |
| | | | Hủy đánh giá | `CANCEL` | đầu mối điều phối | Chuyển trạng thái chương trình thành 3 (Hủy đánh giá). |
| | | | Xử lý lỗi | `RESOLVE_ERROR` | đầu mối điều phối | Mở giao diện và thực thi chạy lại các usecase tự động bị lỗi. |
| | | | Hoàn thành đánh giá | `COMPLETE` | đầu mối điều phối | Chuyển trạng thái chương trình thành 2 (Hoàn thành đánh giá). |
| | | | Cung cấp sở cứ | `SEND` | đầu mối đơn vị | Truy cập màn hình và nộp sở cứ, sở cứ bổ sung. |
| | | | Đánh giá đơn vị | `REVIEW` | đầu mối đánh giá | Thực hiện đánh giá, đánh giá bổ sung. |


---


*End of Document*
