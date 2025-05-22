# Quá trình thực hiện cấu hình firewall trên windown

## Cấu hình allow và block port

* Truy cập firewall thông qua đường dẫn 

* Chọn new rule -> chọn port -> Chọn giao thức tcp hay udp -> chọn block hoặc allow -> ở profile chọn đối tượng áp dụng -> đặt tên rồi save.

## Cấu hình allow và block IP

* Truy cập firewall thông qua đường dẫn 

* Chọn new rule -> chọn custom -> Ở Scope là nơi bạn điều ip để quản lý -> Action chọn block hoặc allow -> ở profile chọn đối tượng áp dụng -> đặt tên rồi save.


## Cấu hình giới hạn IP và Port truy cập

* Tương tự như bước allow hoặc block ip chỉ thêm port cần đặt rule.

* Truy cập firewall thông qua đường dẫn 

* Chọn new rule -> chọn custom -> Ở protocol and port chọn giao thức và điền port tương ứng vào -> Ở Scope là nơi bạn điều ip để quản lý -> Action chọn block hoặc allow -> ở profile chọn đối tượng áp dụng -> đặt tên rồi save.

## Cài đặt webserver IIS

* Mở Server Manager chọn Add Roles and Features -> Chọn "Role-based or feature-based installation" -> Chọn server của bạn -> Tích chọn "Web Server (IIS)" (tích chọn các mục sau để hỗ trợ chạy php và wordpress) ->  -> Tiếp tục Next và Install.

* Kiểm tra hoạt động của IIS bằng cách truy cập vào localhost.

## Cài đặt Wordpress trên IIS

* Cài đặt PHP để chạy wordpress.

* Cài đặt MySQL hoặc MariaDB để làm CSDL cho wordpress (có thể cài extension để dùng SQLServer nếu bắt buộc nhưng không tối ưu).

* Tải gói cài đặt của wordpress tại trang chủ wordpress https://wordpress.org/latest.zip

* Giải nén vào thư mực wwwroot

* Cấu hình để PHP chạy trên IIS, tạo handler Mapping và FastCGI cho PHP trên IIS:

    * Phải thêm PHP vào FastCGI trước -> vào FastCGI -> chọn Add Application -> Trỏ đường dẫn về file php-cgi.exe -> Chọn Ok.

    * Vào Handler Mappings -> Add Module mapping -> Request path chọn *.php -> Chọn loại module là FastCGIModule -> Excutable trỏ đường dẫn về file php-cgi.exe -> Chọn OK.

* Tạo Website trên IIS Mở IIS Manager -> Chuột phải chọn Sites > Add Website -> Điền tên site (ví dụ: WordPressSite) -> Chọn thư mục vật lý tới thư mục WordPress đã giải nén -> Đặt port (mặc định 80) hoặc domain nếu có (ở bước này có thể add domain rồi chỉnh sửa lại file host trỏ domain về localhost để có thể cài SSL) -> Chọn OK.

* Truy cập domain hoặc localhost để kiểm tra xem wordpress đã cài đặt thành công chưa.

## Cài đặt SSL

* Lưu ý: IIS chỉ nhận file .pfx nên phải chuyển đổi định dang từ key và Cert, nên chuyển đổi ngay trên máy chạy IIS để tránh trường hợp lỗi pass không giải được file .pfx.

* Tạo Self-Signed Certificate -> Mở IIS Manager -> Click vào tên server ở cột bên trái -> Trong phần IIS, chọn Server Certificates -> Bên phải chọn Create Self-Signed Certificate -> Đặt tên và OK.

* Gán SSL cho Website -> Vào Sites > chọn website WordPress -> Ở cột bên phải, chọn Bindings -> Thêm mới binding: chọn loại https, port 443, chọn certificate bạn vừa tạo -> Chọn OK

## Cấu hình DB cho Wordpress

* Lưu ý Wordpress chỉ hỗ trợ chính thức cho MySQL và MariaDB.

* Nếu dùng SQL Server bạn trên Windows IIS -> Tải driver tại:https://learn.microsoft.com/sql/connect/php/download-drivers-php-sql-server -> Giải nén và copy 2 file: php_sqlsrv.dll, php_pdo_sqlsrv.dll -> Vào thư mục ext/ của PHP (C:\PHP\ext) -> Mở file php.ini, thêm: extension=php_sqlsrv.dll, extension=php_pdo_sqlsrv.dll -> Khởi động lại IIS hoặc Apache.

* Tải Wordpress đã qua chỉnh sửa để tương thích với SQL Server từ Project Nami Truy cập:https://github.com/ProjectNami/projectnami -> Chọn download to zip rồi giải nén -> Cấu hình wp-config.php để kết nối SQL Server.

* Nếu dùng MySql -> Tải MySQL Installe -> https://dev.mysql.com/downloads/installer/ -> Chạy trình cài đặt -> Chọn kiểu cài: Developer Default hoặc Server Only
    * Thiết lập:

        Cổng mặc định: 3306

        Tài khoản root và mật khẩu

        Có thể chọn tạo thêm user

        Tự động cấu hình MySQL như dịch vụ Windows

    * Kiểm tra dịch vụ MySQl Đăng nhập bằng MySQL Command Line Client sao đó tạo db, user, rồi add quyền db cho user.

* Cấu hình file wp-config.php -> Tìm file wp-config nếu chưa có đổi tên file wp-config-sample.php thành wp-config.php -> Mở file sửa thông tin database: tên DB, username, password thành thông tin vừa tạo ở trên.

## Cài đặt SQL Server

* Truy cập Microsoft tải file exe của SQL server 2016.

* Chạy file và chọn cách tải basic hoặc custom nên chọn custom để cài một số thứ, ở bước Database Engine Configuration chọn Authentication Mode -> Nhập mật khẩu cho user sa -> Thêm Current User làm Admin -> Nhấn Install

* Có thể tải thêm SSMS hay vào service để kiểm tra xem SQL Server chạy chưa.
