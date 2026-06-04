# Chi tiết luồng nghiệp vụ Quản lý chương trình đánh giá (end-to-end)

Dưới đây là bảng mô tả chi tiết luồng nghiệp vụ **Quản lý chương trình đánh giá (end-to-end)** dựa trên sơ đồ quy trình nghiệp vụ:

| STT | Tên bước | Tác nhân | Mô tả công việc | Bước chuyển tiếp theo |
| :--- | :--- | :--- | :--- | :--- |
| **1** | **Tạo danh mục** | User được phân quyền | Thực hiện tạo các danh mục hệ thống bao gồm:<br>- Đối tượng đánh giá<br>- Tiêu chí đánh giá<br>- Các Usecase (UC) tự động và thủ công. | **2. Tạo chương trình đánh giá** |
| **2** | **Tạo chương trình đánh giá** | User được phân quyền | Thiết lập thông tin chương trình đánh giá mới:<br>- Điền thông tin chương trình.<br>- Lựa chọn đầu mối điều phối.<br>- Cấu hình đơn vị đánh giá (đầu mối đánh giá, đầu mối đơn vị, đối tượng, tiêu chí, UC áp dụng). | **3. Chạy chương trình đánh giá** |
| **3** | **Chạy chương trình đánh giá** | Đầu mối điều phối | Thực hiện lệnh chạy/kích hoạt chương trình đánh giá. | **Phân luồng dựa trên loại Usecase:**<br>- Đối với UC thủ công $\rightarrow$ **4. Gửi UC thủ công cho các bên liên quan**<br>- Đối với UC tự động $\rightarrow$ **5. Tự động chạy => Đánh giá UC tự động** |
| **4** | **Gửi UC thủ công cho các bên liên quan** | Hệ thống | Gửi thông báo và phân bổ các Usecase thủ công đến đầu mối đơn vị và đầu mối đánh giá để chuẩn bị thực hiện. | **6. Nộp sở cứ UC theo từng đơn vị** |
| **5** | **Tự động chạy => Đánh giá UC tự động** | Hệ thống | Hệ thống tự động thực hiện quét và đánh giá các Usecase tự động của các đơn vị. | **Đánh giá kết quả chạy tự động:**<br>- Nếu hoàn thành và số lượng UC lỗi = 0 $\rightarrow$ **12. Hoàn thành chương trình đánh giá**<br>- Nếu chạy lỗi (SL UC lỗi > 0) $\rightarrow$ **10. Chạy lại UC** |
| **6** | **Nộp sở cứ UC theo từng đơn vị** | Đầu mối đơn vị | Đầu mối đơn vị tiến hành tải lên (upload) và nộp các tài liệu sở cứ minh chứng cho từng Usecase thủ công của đơn vị mình. | **7. Đánh giá UC theo từng đơn vị** |
| **7** | **Đánh giá UC theo từng đơn vị** | Đầu mối đánh giá | Kiểm tra, thẩm định các sở cứ do đầu mối đơn vị nộp lên để tiến hành đánh giá. | **Kiểm tra tình trạng sở cứ:**<br>- Nếu thiếu sở cứ $\rightarrow$ **9. Bổ sung sở cứ**<br>- Nếu đủ sở cứ $\rightarrow$ **8. Hoàn thành đánh giá đơn vị** |
| **8** | **Hoàn thành đánh giá đơn vị** | Đầu mối đánh giá | Ghi nhận kết quả đánh giá cuối cùng của đầu mối đơn vị sau khi thẩm định xong toàn bộ Usecase. | Sau khi tất cả đầu mối đơn vị hoàn thành đánh giá $\rightarrow$ **12. Hoàn thành chương trình đánh giá** |
| **9** | **Bổ sung sở cứ** | Đầu mối đơn vị | Nhận yêu cầu nộp thêm sở cứ từ đầu mối đánh giá, tiến hành bổ sung tài liệu cần thiết và gửi lại. | **8. Hoàn thành đánh giá đơn vị** |
| **10** | **Chạy lại UC** | Đầu mối điều phối | Đầu mối điều phối thực hiện lệnh kích hoạt chạy lại đối với các Usecase tự động bị lỗi. | **11. Chạy lại UC (Hệ thống)** |
| **11** | **Chạy lại UC** | Hệ thống | Thực thi lại tiến trình đánh giá tự động cho các Usecase bị lỗi. | Quay lại kiểm tra điều kiện của **Bước 5** (Hoàn thành, SL UC lỗi = 0?) |
| **12** | **Hoàn thành chương trình đánh giá** | Đầu mối điều phối | Xác nhận đóng và hoàn thành chương trình đánh giá khi tất cả Usecase tự động (không lỗi) và Usecase thủ công (tất cả đầu mối đơn vị đã được hoàn tất đánh giá) đều hoàn thành. | Kết thúc quy trình (End) |
