# HỆ THỐNG QUẢN LÝ CÔNG VIỆC VÀ TÀI LIỆU BUSINESS ANALYST (BA)

Chào mừng bạn đến với không gian làm việc chuyên nghiệp được thiết kế riêng cho **Business Analyst (BA)**! 
Hệ thống thư mục này được xây dựng chuẩn hóa theo yêu cầu cụ thể của bạn, nhằm tối ưu hóa việc phân loại tài liệu, theo dõi yêu cầu và phối hợp hiệu quả cùng đội ngũ Lập trình (Developer) & Kiểm thử (Tester).

---

## 📂 Hướng dẫn Cấu trúc Thư mục

Mỗi thư mục đại diện cho một mảng tài liệu cốt lõi trong công việc phân tích nghiệp vụ của bạn:

1. **[BRD/](file:///Users/whis/Desktop/Anh/BRD/)**: Lưu trữ **Tài liệu Yêu cầu Nghiệp vụ** (Business Requirements Document). Tập trung vào mục tiêu kinh doanh, phạm vi dự án ở mức cao.
   - 📄 Đã có sẵn mẫu: [BRD_Template.md](file:///Users/whis/Desktop/Anh/BRD/BRD_Template.md)
2. **[SRS/](file:///Users/whis/Desktop/Anh/SRS/)**: Lưu trữ **Tài liệu Đặc tả Chi tiết Phần mềm** (Software Requirements Specification). Dành cho Developer & Tester để hiểu rõ các quy tắc nghiệp vụ và luồng xử lý của hệ thống.
   - 📄 Đã có sẵn mẫu: [SRS_Template.md](file:///Users/whis/Desktop/Anh/SRS/SRS_Template.md)
3. **[URD/](file:///Users/whis/Desktop/Anh/URD/)**: Lưu trữ **Tài liệu Yêu cầu Người dùng** (User Requirements Document). Mô tả các chân dung người dùng (User Personas), mong muốn và các giải pháp giải quyết nỗi đau của họ.
   - 📄 Đã có sẵn mẫu: [URD_Template.md](file:///Users/whis/Desktop/Anh/URD/URD_Template.md)
4. **[ChangeRequest/](file:///Users/whis/Desktop/Anh/ChangeRequest/)**: Quản lý các **Yêu cầu Thay đổi** (Change Requests) phát sinh trong quá trình phát triển dự án, giúp đánh giá tác động về tiến độ, nhân lực và chi phí.
   - 📄 Đã có sẵn mẫu: [Change_Request_Template.md](file:///Users/whis/Desktop/Anh/ChangeRequest/Change_Request_Template.md)
5. **[userstory/](file:///Users/whis/Desktop/Anh/userstory/)**: Dành cho các dự án phát triển theo mô hình **Agile/Scrum**. Lưu trữ các User Story cùng tiêu chí nghiệm thu (Acceptance Criteria) chuẩn Given-When-Then.
   - 📄 Đã có sẵn mẫu: [User_Story_Template.md](file:///Users/whis/Desktop/Anh/userstory/User_Story_Template.md)
6. **[wireframe/](file:///Users/whis/Desktop/Anh/wireframe/)**: Nơi lưu trữ phác thảo giao diện, mockups, user flows hoặc liên kết tới các phần mềm thiết kế như Figma, Balsamiq.
   - 📄 Đã có sẵn hướng dẫn: [README.md](file:///Users/whis/Desktop/Anh/wireframe/README.md)
7. **[usecase/](file:///Users/whis/Desktop/Anh/usecase/)**: Chứa các đặc tả chi tiết về **Ca sử dụng** (Use Case Specifications), mô tả rõ ràng tương tác giữa Tác nhân (Actor) và Hệ thống qua từng bước cụ thể.
   - 📄 Đã có sẵn mẫu: [UseCase_Template.md](file:///Users/whis/Desktop/Anh/usecase/UseCase_Template.md)
8. **[Co_so_du_lieu/](file:///Users/whis/Desktop/Anh/Co_so_du_lieu/)**: Lưu trữ các sơ đồ cơ sở dữ liệu (ERD), Data Dictionary (Từ điển dữ liệu) và các scripts SQL để hỗ trợ việc làm rõ cấu trúc dữ liệu.
   - 📄 Đã có sẵn mẫu: [Database_Design_Template.md](file:///Users/whis/Desktop/Anh/Co_so_du_lieu/Database_Design_Template.md)

---

## 💡 Mẹo nhỏ dành cho BA
* **Tạo phiên bản (Version Control):** Hãy luôn cập nhật nhật ký thay đổi trong tài liệu khi có bất kỳ sự thay đổi nào được duyệt.
* **Tài liệu dạng Markdown (.md):** Định dạng Markdown rất nhẹ, có thể dễ dàng đọc trực tiếp trên các IDE (như VS Code, Cursor), GitHub hoặc xuất ra file PDF/Word rất đẹp mắt.
* **Tận dụng Sơ đồ (Diagrams):** Hãy sử dụng cú pháp `mermaid` được tích hợp sẵn để vẽ sơ đồ trực quan (như sơ đồ ERD, sơ đồ trạng thái, flowchart...) trực tiếp trong tài liệu Markdown mà không cần phần mềm vẽ bên ngoài.

Chúc bạn có những trải nghiệm làm việc tuyệt vời và hiệu suất vượt trội với bộ công cụ quản lý này!
