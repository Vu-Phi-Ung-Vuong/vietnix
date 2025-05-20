# Nginx Apache Reverse Proxy

## Cấu trúc mô hình

* Nginx sẽ nhận request đầu tiên sau đó gửi request về cho Apache vì:

    * Nginx tốc độ reverse proxy rất nhanh, nhẹ và tối ưu cho xử lý kết nối đồng thời lưu lượng cao. Nginx xử lý các file như (css,js,jpg...) rất hiệu quả giảm tải lớn cho Apache.

    * Apache có lợi thế khi có module PHP trực tiếp, dễ quản lý code PHP.

# Quá trình cấu hình

## Chuẩn bị tài nguyên để khởi chạy hai project gồm:

* Cài đặt Nginx, Apache2, mysql-server, php...

* Move source code, db lên vps.

## Cấu hình MySQL

* Truy cập bằng câu lệnh ``` mysql -u root -p ``` sau đó nhập mật khẩu, tạo mới db cho dự án rồi import file db lên bằng câu lệnh:
    ```
    mysql -u root -p laravel_db < /home/ubuntu/database.sql
    ```

* Cấu hình để MySQL có thể remote: mở file mysqld.cnf tìm dòng bind-address sửa từ localhost thành 0.0.0.0 để cấp phép, tạo user để remote bằng câu lệnh sau:
    ```
    CREATE USER 'remoteuser'@'%' IDENTIFIED BY 'password';
    GRANT ALL PRIVILEGES ON *.* TO 'remoteuser'@'%' WITH GRANT OPTION;
    ```
# Cấu hình Apache

* Tạo vHost Apache cho 2 website, file cấu hình được tạo ở sities-avaible sau được symlink sang sities-enable.

* File cấu hình cho wordpress, laravel:

    * Wordpress:
    ```
    <VirtualHost *:8081>
    ServerName wp.vuvuong.vietnix.tech
    DocumentRoot /var/www/html/wordpress

    <Directory /var/www/html/wordpress>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
        <FilesMatch \.php$>
        SetHandler "proxy:unix:/run/php/php8.1-fpm.sock|fcgi://localhost"
    </FilesMatch>

   ErrorLog ${APACHE_LOG_DIR}/wordpress_error.log
    CustomLog ${APACHE_LOG_DIR}/wordpress_access.log combined
    </VirtualHost>

    <VirtualHost *:8443>
    ServerName wp.vuvuong.vietnix.tech
    DocumentRoot /var/www/html/wordpress

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/certificate_wp.crt
    SSLCertificateKeyFile /etc/ssl/private/private_wp.key

    <Directory /var/www/html/wordpress>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    <FilesMatch \.php$>
        SetHandler "proxy:unix:/run/php/php8.1-fpm.sock|fcgi://localhost"
    </FilesMatch>

    ErrorLog ${APACHE_LOG_DIR}/wordpress_error.log
    CustomLog ${APACHE_LOG_DIR}/wordpress_access.log combined
    </VirtualHost>
    ```
    * Laravel
    ```
    <VirtualHost *:8080>
    ServerName laravel.vuvuong.vietnix.tech
    DocumentRoot /var/www/html/laravel/public

    <Directory /var/www/html/laravel/public>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    <FilesMatch \.php$>
        SetHandler "proxy:unix:/run/php/php8.1-fpm.sock|fcgi://localhost"
    </FilesMatch>

    ErrorLog ${APACHE_LOG_DIR}/wordpress_error.log
    CustomLog ${APACHE_LOG_DIR}/wordpress_access.log combined
    </VirtualHost>

    <VirtualHost *:8444>
    ServerName laravel.vuvuong.vietnix.tech
    DocumentRoot /var/www/html/laravel/public

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/certificate_laravel.crt
    SSLCertificateKeyFile /etc/ssl/private/private_laravel.key

    <Directory /var/www/html/laravel/public>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    <FilesMatch \.php$>
        SetHandler "proxy:unix:/run/php/php8.1-fpm.sock|fcgi://localhost"
    </FilesMatch>

    </VirtualHost>

* Đảm bảo các port cấu hình phải có trong ```/etc/apache2/ports.conf``` và phải được mở firewall ```sudo ufw allow```

* Chạy lệnh ```sudo a2ensite wordpress.conf``` để kích hoạt

* Sau đó khởi động lại Apache để áp dụng cấu hình ```sudo systemctl restart apache2```.

# Cấu hình Nginx

* Default
```              
server {
    listen 80 default_server;
    server_name _;

    return 404;
}
```

* Wordpress
```
server {
    listen 80;
    server_name wp.vuvuong.vietnix.tech;
     location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot|otf)$ {
        root /var/www/html/wordpress;
        expires max;
        access_log off;
    }   
    location / {
        proxy_pass http://103.90.226.252:8081;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Port $server_port;
    }

}

server {
    listen 443 ssl http2;
    server_name wp.vuvuong.vietnix.tech;

    ssl_certificate /etc/ssl/certs/certificate_wp.crt;
    ssl_certificate_key /etc/ssl/private/private_wp.key;

     location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot|otf)$ {
        root /var/www/html/wordpress;
        expires max;
        access_log off;
    }

    location / {
        proxy_pass https://103.90.226.252:8443;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Port $server_port;
    }
}
```

* Laravel
```
server {
    listen 80;
    server_name laravel.vuvuong.vietnix.tech;
     location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot|otf)$ {
        root /var/www/html/laravel/public;
        expires max;
        access_log off;
    }

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

server {
    listen 443 ssl http2;
    server_name laravel.vuvuong.vietnix.tech;

    ssl_certificate /etc/ssl/certs/certificate_laravel.crt;
    ssl_certificate_key /etc/ssl/private/private_laravel.key;
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot|otf)$ {
        root /var/www/html/laravel/public;
        expires max;
        access_log off;
    }

    location / {
        proxy_pass https://127.0.0.1:8444;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Port $server_port;
    }
}
```
# Cấu hình xong phải khởi động bằng câu lệnh

* Symlink từ avaible sang enabled
```
sudo ln -s /etc/nginx/sites-available/reverse-proxy.conf /etc/nginx/sites-enabled/
```

* Chạy cấu hình
```
sudo nginx -t
sudo systemctl restart nginx
```
# Cài phpmyadmim

* Đầu tiên tải về bằng lệnh sau ```sudo apt install phpmyadmin -y```

* Cấu hình phpmyadmin vào Nginx
```
server {
    listen 80;
    server_name 103.90.226.252;

    location /phpmyadmin {
        proxy_pass http://103.90.226.252:8080/phpmyadmin;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

# Điều chỉnh option_value của siteurl và home để website không chạy mặc định https

