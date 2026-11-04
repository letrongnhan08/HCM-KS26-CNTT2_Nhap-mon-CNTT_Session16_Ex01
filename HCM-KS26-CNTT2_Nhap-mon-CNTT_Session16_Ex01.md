**Bài 1 : Sự cố không truy cập được hệ thống quản trị**

**2. Yêu cầu**

* **Chức năng 1 (Phân tích mạng):**
* DNS Lookup: trình duyệt yêu cầu DNS tra cứu IP của trang web phong-dieu-phoi.greenlog.vn
* TCP / TLS Handshake: giúp trình duyệt kết nối thành công tới IP của trang web.
* HTTP Request: trình duyệt sẽ gửi yêu cầu lấy nội dung từ trang web
* Browser Render: trình duyệt nhận dữ liệu tải về, phân tích HTML và CSS để làm cây DOM và CSSOM, sau đó tính toán bố cục Layout và vẽ các ảnh lên màn hình để người dùng nhìn thấy giao diện hoàn chỉnh.
* **Chức năng 2 (Chẩn đoán lỗi):**
* Lỗi DNS: nằm ở phía Client là do cá nhân nhập sai địa chỉ IP hay đường dẫn không tồn tại dẫn đến lỗi 404 Not Found
* Lỗi 500 Internal Server Error: nằm ở phía Server là do chương trình bên trong máy chủ bên kia bị sập, lỗi mạng hoặc quá tải tài nguyên thì gõ đúng địa chỉ IP thì vẫn không vào được trang web.

**3. Ràng buộc**

\-Hậu quả của việc sai lệch tên miền

* Nam gõ nhầm địa chỉ  phong-dieu-phoi.grenlog.vn thiếu 1 chữ e dẫn đến việc DNS không dịch ra đúng với địa chỉ IP của trang web vì trang web không tồn tại dẫn đến lỗi 404 Not Found.

***Phần 1. Phân tích***

**Xác định Input (Tên miền) / Output (IP address) và mô tả sơ đồ luồng dữ liệu của DNS.**

* Input: nhập tên miền  phong-dieu-phoi.grenlog.vn
* DNS nhận được tên miền và dịch ra địa chỉ IP của tên miền so với trang web
* Output:  Tìm ra được IP của máy chủ hoặc trả về thông báo không tìm thấy tên miền, không phân giải được ra IP báo lỗi DNS\_PROBE\_FINISHED\_NXDOMAIN.


 sơ đồ luồng dữ liệu của DNS:

|             DNS Lookup <br />(trình duyệt yêu cầu DNS tra cứu IP của trang web)|
|-|

|

v

| TCP / TLS Handshake<br />(trình duyệt kết nối thành công tới IP của trang web) |
| ------------------------------------------------------------------------------ |

|

v

|HTTP Request<br />(trình duyệt sẽ gửi yêu cầu lấy nội dung từ trang web)|
|-|

|

v

|Browser Render<br />(nhận dữ liệu ,phân tích HTML,CSS làm cây DOM , tính toán và đưa ra giao diện cho người dùng)|
|-|





***Phần 2. Triển khai***

* **Đề xuất quy trình kiểm tra mã trạng thái HTTP Status Code để xác nhận xem yêu cầu đã chạm tới máy chủ hay chưa.**
* Nhấn F12
* Chọn tab NetWork
* Chọn tên miền và sẽ ra khung Headers
* Kéo xuống xem phần Status Code: 200 OK, 404 Not Found, 500 Internal Server Error

***Phần 3. Kiểm thử***

* **Giả định Nam sửa đúng tên miền nhưng vẫn nhận mã 404 Not Found, hệ thống đang gặp vấn đề gì?**
* Do quản trị viên đã xóa đi trang Web nên dù đúng IP thì vẫn hiện lỗi  404 Not Found vì trang web bị xóa không tồn tại
* Nam gõ tên miền thì đúng nhưng có thể gõ sai phần phía sau hoặc thiếu dấu

