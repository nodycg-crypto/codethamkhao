# Hệ thống đấu giá trực tuyến - CS2043 16
**Bài tập lớn môn Lập Trình Nâng Cao - Trường Đại học Công nghệ (UET), ĐHQGHN**

Hệ thống đấu giá trực tuyến với mục đích tạo ra một sân chơi công bằng và minh bạch cho người đấu giá cũng như đảm bảo giá trị cho các vật phẩm được đấu giá.

**Sinh viên thực hành:** 
- Mai Văn Đồng (25023464)
- Hoàng Lâm Trí Dũng (25020502)
- Đỗ Hoàng Anh (25023434)

---

## 1. Mô tả ngắn gọn bài toán và phạm vi hệ thống
Hệ thống đấu giá trực tuyến là một ứng dụng phần mềm hoạt động theo mô hình Client - Server, cho phép người dùng tham gia các phiên đấu giá tài sản theo thời gian thực (real-time). 
- **Phạm vi hệ thống:** Ứng dụng cung cấp nền tảng giao dịch cho 3 đối tượng chính: Quản trị viên (Admin - duyệt nạp/rút tiền), Người bán (Seller - đăng tài sản), và Người mua (Bidder - nạp tiền, đặt giá).
- **Điểm nổi bật:** Đồng bộ hóa giá thầu giữa nhiều Client cùng lúc bằng công nghệ Multi-threading và Socket Programming. Hệ thống tích hợp thuật toán xử lý luồng phiên đấu giá (Auction) và xác thực giao dịch đặt giá (BidTransaction) chặt chẽ, cùng luồng Scanner tự động quét và đóng các phiên khi hết hạn.

## 2. Công nghệ sử dụng, môi trường chạy và yêu cầu cài đặt
**Công nghệ và Môi trường:**
- Ngôn ngữ phát triển: Java (Yêu cầu JDK 21 trở lên).
- Giao diện người dùng (GUI): JavaFX.
- Cơ sở dữ liệu: MySQL.
- Thư viện kết nối DB: JDBC Driver (mysql-connector-j).

**Yêu cầu cài đặt:**
- Máy tính đã cài đặt sẵn JDK (Java Development Kit) và đã cấu hình biến môi trường JAVA_HOME.
- Cài đặt localhost Server cho MySQL.
- IDE: VS Code, IntelliJ hoặc Eclipse.

## 3. Cấu trúc thư mục hệ thống
Dự án được tổ chức tách biệt rõ ràng giữa Client và Server:

    📦 project-root
     ┣ 📂 src/main/java
     ┃ ┣ 📂 vnu_daugia_client       # Phía Máy trạm (Giao diện, Xử lý sự kiện)
     ┃ ┃ ┣ 📜 MainClient.java       # Khởi tạo Application JavaFX (Chạy Client)
     ┃ ┃ ┗ 📂 controller            # Các Controller xử lý logic giao diện
     ┃ ┣ 📂 vnu_daugia_server       # Phía Máy chủ (Socket, Multi-threading)
     ┃ ┃ ┣ 📜 MainServer.java       # Server lắng nghe kết nối & luồng Scanner
     ┃ ┃ ┣ 📂 network               # ClientHandler quản lý từng luồng Client
     ┃ ┃ ┗ 📂 util                  # Tiện ích hệ thống (DatabaseUtil)
     ┣ 📂 src/main/resources        # Tệp tĩnh (FXML, CSS, Hình ảnh)
     ┣ 📂 database                  # Script tạo cấu trúc SQL (Binding_db.sql)
     ┣ 📜 pom.xml                   # Cấu hình thư viện Maven
     ┗ 📜 README.md

## 4. Hướng dẫn khởi tạo Cơ sở dữ liệu
Yêu cầu cấu hình Database trước khi chạy hệ thống:
1. Mở XAMPP Control Panel, nhấn Start cho Apache và MySQL.
2. Truy cập http://localhost/phpmyadmin, tạo Database tên là `Binding_db`.
3. Chạy file script SQL (trong thư mục `database`) để tạo bảng và dữ liệu mẫu.
*(Lưu ý: Đảm bảo cấu hình tài khoản MySQL trong file `vnu_daugia_server/util/DatabaseUtil.java` khớp với máy của bạn).*

## 5. Hướng dẫn chạy chương trình (Nhiều cách)

### Cách 1: Chạy trực tiếp trên phần mềm IDE (Dễ nhất - Không cần gõ lệnh)
Cách này áp dụng cho các môi trường như VS Code, IntelliJ IDEA hoặc Eclipse. 

1. **Khởi động Server (Bắt buộc chạy trước):**
   - Tìm đến file `MainServer.java` (thuộc package `vnu_daugia_server`).
   - Bấm nút **Run**. Đợi hệ thống báo Server kết nối.
2. **Khởi động Client (Chạy sau khi Server đã bật):**
   - Tìm đến file `MainClient.java` (thuộc package `vnu_daugia_client`).
   - Bấm nút **Run** để khởi động giao diện người dùng.

### Cách 2: Chạy bằng dòng lệnh CLI với Maven
Nếu máy bạn có cài đặt sẵn Maven, thao tác này sẽ đồng nhất trên mọi hệ điều hành. Cần mở 2 cửa sổ Terminal độc lập (1 cho Server, 1 cho Client).

**Trên Windows (PowerShell/CMD):**
    # Biên dịch dự án
    mvn clean compile
    
    # Chạy Server
    mvn exec:java -Dexec.mainClass="vnu_daugia_server.MainServer"
    
    # Mở Terminal thứ 2, Chạy Client
    mvn exec:java -Dexec.mainClass="vnu_daugia_client.MainClient"

**Trên Linux / MacOS (Terminal):**
    # Biên dịch dự án
    mvn clean compile
    
    # Chạy Server
    mvn exec:java -Dexec.mainClass=vnu_daugia_server.MainServer
    
    # Mở Terminal thứ 2, Chạy Client
    mvn exec:java -Dexec.mainClass=vnu_daugia_client.MainClient

## 6. Danh sách chức năng đã hoàn thành
- Xác thực người dùng: Đăng nhập, nhận diện vai trò (Admin/Seller/Bidder).
- Quản lý phiên đấu giá (Admin): Khởi tạo thời gian, giá khởi điểm.
- Đăng tài sản (Seller): Điền thông tin vật phẩm cần đấu giá.
- Giao dịch tài chính: Giả lập nạp tiền, rút tiền, cập nhật số dư.
- Xử lý BidTransaction: Đặt giá thầu, kiểm tra tính hợp lệ của số dư và bước giá.
- Đồng bộ đa luồng (Real-time): Đẩy dữ liệu giá mới nhất tới toàn bộ Client đang theo dõi.
- Luồng Scanner (Auto-close): Quét DB mỗi 10 giây để chốt kết quả và phân bổ tiền tự động.

## 7. Tài liệu và Demo
- Báo cáo chi tiết (PDF): https://drive.google.com/file/d/1glkfQpF0OnuuyS1mGRDrRdNaKI34pXGQ/view?usp=sharing
- Video Demo Hệ thống: https://drive.google.com/file/d/1gqOrQDQ92xpIEKssUd0Wv2mNoCX05eru/view?usp=drive_link
