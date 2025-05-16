# LAB1
## SSL LÀ GÌ ?

SSL là một giao thức bảo mật dùng để mã hóa dữ liệu truyền tải giữa trình duyệt và máy chủ web

## Có bao nhiêu cách để xác thực SSL?

* Có ba cách xác thực SSL

    * 1. Domain Validation (DV) xác thực domain.

    * 2. Xác thực qua email.

    * 3. Xác thực qua bản ghi DNS.

## CSR file dùng để làm gì trong quá trình tạo SSL?

* CSR là một đoạn văn bản yêu cầu chứa thông tin đã được mã hóa được gửi đến nhà cung cấp dịch vụ SSL để xác nhận.

## Gen file CSR và cách gửi yêu cầu xác thực
```
    openssl req -new -newkey rsa:2048 -nodes -keyout tech.training.vietnix.tech.key -out tech.training.vietnix.tech.csr
```
## File PEM là gì?

* File PEM (Privacy Enhanced Mail) là một định dạng tệp được sử dụng để lưu trữ và truyền tải các chứng chỉ, khóa riêng tư, và các dữ liệu bảo mật khác trong các hệ thống mã hóa.

## Private key ssl là gì ?

* Private Key SSL đóng vai trò là chìa khóa để máy chủ có thể giải mã được thông tin từ client và xác thực để bắt đầu một phiên kết nối SSL.

## PFX file là gì ? Cách chuyển từ file crt file sang PFX file.

* File PFX là một tệp chứa chứng chỉ số và khóa riêng tư được mã hóa bằng mật khẩu. Định dạng tệp PFX (hay còn gọi là PKCS#12) được sử dụng để trao đổi chứng chỉ số và khóa riêng tư giữa các hệ thống hoặc để cài đặt chúng trên một máy tính hoặc một máy chủ web.

* Có hai cách chuyển:

    * Cách 1: Sử dụng lệnh OpenSSL
    ```
    openssl pkcs12 -export \ -out domain.pfx \ -inkey domain.key \ -in domain.crt \ -certfile ca-bundle.crt
    ```
    | Thành phần  | Ý nghĩa                                      |
    |-------------|----------------------------------------------|
    | `-export`   | Xuất ra định dạng `.pfx` (PKCS#12)           |
    | `-out`      | Tên file đầu ra `.pfx`                       |
    | `-inkey`    | File chứa **private key** (`.key`)           |
    | `-in`       | File chứng chỉ chính (`.crt`)                |
    | `-certfile` | Chuỗi chứng chỉ trung gian (intermediate CA) |

    *  Cách 2: Sử dụng công cụ Online để converter

## Domain là gì?

* Domain là địa chỉ bằng chữ dễ nhớ dùng để truy cập website thay thế cho địa chỉ ip.

## Các trạng thái của domain?

| **Trạng thái tại Đơn vị Cấp phát tên miền (Registry)** | **Ý nghĩa** | **Nên làm gì khi tên miền ở trong trạng thái này** |
|--------------------------------------------------------|-------------|---------------------------------------------------|
| OK                                                    | Tên miền hoạt động bình thường sau khi đăng ký. | Nên yêu cầu nhà đăng ký thực hiện các trạng thái hạn chế nhằm ngăn chặn chuyển đổi, xóa hoặc cập nhật trái phép vào tên miền như: `clientTransferProhibited`, `clientDeleteProhibited`, `clientUpdateProhibited`. |
| addPeriod                                             | Trạng thái sau khi vừa đăng ký tên miền. | Không có vấn đề phát sinh nào với tên miền. Đây là trạng thái được đặt sau vài ngày đầu đã đăng ký tên miền. |
| autoRenewPeriod                                       | Thời gian tự động gia hạn tên miền. | Sau khi đăng ký tự động gia hạn tên miền, trạng thái này sẽ được đặt trong một khoảng thời gian giới hạn. Nếu không muốn trả phí gia hạn, chỉ cần liên hệ với nhà đăng ký để được hỗ trợ. |
| inactive                                              | Tên miền đã được đăng ký nhưng Name Server chưa thể liên kết với tên miền. | Khi tên miền ở trong trạng thái này trong vài ngày, hãy liên hệ với nhà đăng ký để yêu cầu xử lý về sự chậm trễ trong quá trình đưa tên miền vào hoạt động. |
| pendingCreate                                         | Tên miền đang chờ được đăng ký. | Yêu cầu tạo tên miền đã được xác nhận và trong quá trình xử lý. |
| pendingDelete                                         | Tên miền hết hạn đăng ký và chuẩn bị xóa. | Chờ tên miền trở về trạng thái tự do, sau đó có thể đăng ký lại theo chính sách của cơ quan đăng ký. |
| pendingRenew                                          | Tên miền đang chờ gia hạn. | Yêu cầu gia hạn tên miền của bạn đã được tiếp nhận và đang trong quá trình xử lý. |
| pendingRestore                                        | Tên miền đã hết hạn đang chờ về trạng thái khôi phục (Active). | Theo dõi tên miền trong 7 ngày để xác minh nhà đăng ký đã thực hiện yêu cầu khôi phục tên miền. Nếu tên miền chuyển về trạng thái `redemptionPeriod`, hãy liên hệ ngay với nhà đăng ký để được hỗ trợ. |
| pendingTransfer                                       | Tên miền chờ chuyển đổi nhà đăng ký. | Nếu không chuyển đổi tên miền, hãy liên hệ với nhà đăng ký để yêu cầu họ từ chối yêu cầu chuyển tên miền. Sau đó, đưa về trạng thái `clientTransferProhibited`. |
| pendingUpdate                                         | Tên miền đang chờ cập nhật. | Nếu bạn không yêu cầu cập nhật thêm thông tin, hãy liên hệ với nhà đăng ký để được giúp đỡ. |
| redemptionPeriod                                      | Tên miền đã hết hạn và cần thanh toán phí gia hạn nếu muốn tiếp tục sử dụng. | Nếu muốn giữ tên miền, bạn cần liên hệ ngay với nhà đăng ký trước khi tên miền bị xóa. Thông thường, thời gian này kéo dài 30 ngày. |
| renewPeriod                                           | Tên miền được gia hạn. | Trạng thái này được đặt trong một khoảng thời gian giới hạn cho việc xác nhận gia hạn tên miền với nhà đăng ký. |
| serverDeleteProhibited                                | Ngăn tên miền bị xóa. | Trạng thái này thường được ban hành trong các trường hợp tranh chấp pháp lý hoặc redemptionPeriod. Liên hệ nhà đăng ký để gỡ bỏ. |
| serverHold                                            | Tên miền không được kích hoạt trong DNS. | Để kiểm tra thông tin, bạn phải liên hệ với nhà đăng ký. |
| serverRenewProhibited                                 | Tên miền không thể được gia hạn. | Liên hệ nhà đăng ký để yêu cầu gỡ bỏ trạng thái này. |
| serverTransferProhibited                              | Không cho phép chuyển đổi nhà đăng ký. | Liên hệ nhà đăng ký để gỡ bỏ trạng thái này. |
| serverUpdateProhibited                                | Không cho phép cập nhật tên miền. | Liên hệ nhà đăng ký để gỡ bỏ trạng thái này. |
| transferPeriod                                        | Trạng thái sau khi chuyển đổi nhà đăng ký. | Trạng thái này sẽ được đặt trong một khoảng thời gian giới hạn sau khi bạn chuyển tên miền sang nhà đăng ký mới. |

| **Trạng thái tại Nhà đăng ký tên miền (Registrar)** | **Ý nghĩa** | **Bạn nên làm gì khi tên miền trong trạng thái này** |
|-----------------------------------------------------|-------------|-----------------------------------------------------|
| clientDeleteProhibited                              | Cấm hủy domain/ Không cho phép xóa tên miền. | Trạng thái thể hiện không thể xóa tên miền, giúp ngăn chặn việc xóa trái phép do chiếm quyền điều khiển hay gian lận. |
| clientHold                                          | Tạm ngừng tên miền (suspend tên miền). | Với trạng thái này, DNS tên miền của bạn sẽ không hoạt động. Liên hệ nhà cung cấp để được trợ giúp. |
| clientRenewProhibited                               | Cấm gia hạn tên miền. | Trạng thái không phổ biến, thường được ban hành khi có tranh chấp pháp lý. Liên hệ nhà đăng ký để gia hạn hoặc xóa trạng thái này. |
| clientTransferProhibited                            | Không cho phép chuyển đổi nhà đăng ký. | Trạng thái này giúp ngăn chặn trái phép khi bị chiếm quyền điều khiển hoặc lừa đảo. |
| clientUpdateProhibited                              | Cấm cập nhật thông tin. | Trạng thái này giúp ngăn chặn các cập nhật tên miền trái phép từ kẻ xấu. |


    

## Subdomain là gì?

* Là tên miền phụ phần mở rộng của tên miền chính.

## Virtual host là gì?

* Virtual Hosts là một tính năng trong web server và cũng là một phương thức lưu trữ, cho phép nhiều trang web hoặc tên miền hoạt động trên cùng một máy chủ vật lý hoặc một địa chỉ IP duy nhất.

## Mail Server

* MX Record là gì?
    * MX (Mail Exchange) Record là bản ghi DNS chỉ định server nào quản lý các dịch vụ Email của tên miền đó.

* DKIM, SPF, PTR là gì
    * DKIM (DomainKeys Identified Mail) – Sử dụng chữ ký số để xác thực rằng nội dung email không bị chỉnh sửa, và được gửi từ domain hợp lệ.

    * SPF (Sender Policy Framework) – Dùng IP để xác minh máy chủ được phép gửi email thay mặt domain.

    * PTR (Reverse DNS) – Bản ghi ánh xạ IP ngược về hostname, giúp xác thực server gửi email.

## DNS là gì?

* Đây hệ thống phân giải tên miền thành địa chỉ IP.

## Các loại recored DNS

* CNAME Record: Là một bản ghi tên quy chuẩn (Canonical Name Record). Đây là một dạng bản ghi tài nguyên trong hệ thống tên miền.

* A Record: Dùng để trỏ tên miền website tới một địa chỉ IPv4 cụ thể. Đây được xem là bản ghi DNS đơn giản nhất.

* MX Record: Bản ghi này bạn có thể sử dụng để trỏ tên miền đến mail server. MX Record chỉ định server nào quản lý các dịch vụ Email của tên miền đó.

* AAAA Record: Dùng để trỏ tên miền đến địa chỉ IPv6 và cho phép thêm host mới, TTL và IPv6.

* TXT Record: Dùng để thêm giá trị TXT, Host mới, TTL và Point To để chứa các thông tin định dạng văn bản domain.

* SRV Record: Đây là bản ghi DNS đặc biệt, dùng để xác định chính xác dịch vụ nào đang chạy Port nào. Và thông qua bản ghi này bạn có thể thêm Priority, Port, Weight, TTL, Point to Point.

* NS Record: Bản ghi này có thể chỉ định Name Server cho từng tên miền phụ và bên cạnh đó có thể tạo tên Name Server, TTL hay host mới.

* PTR (Reverse DNS) – Bản ghi ánh xạ IP ngược về hostname, giúp xác thực server gửi email.

## Nguyên tắc làm việc của DNS

* DNS hoạt động theo mô hình cây phân cấp.

* Sẽ dịch tên miền từ phải sang trái.

* DNS có hai vai trò chính:

    * Một là biên dịch các trang web.
    * Hai là trả lời các DNS khác khi truy vấn đúng tên miền mà bản thân nó quản lý.

## Cách phân giải địa chỉ của DNS

* Người dùng nhập domain vào trình duyệt.

* Máy chủ DNS cục bộ sẽ kiểm tra cơ sở dữ liệu của mình xem có chứa địa chỉ IP của website hay không. Nếu có, sẽ trả về địa chỉ IP cho máy tính của người dùng.

* Nếu máy chủ DNS cục bộ không có địa chỉ IP của website, nó sẽ hỏi máy chủ DNS gốc. Máy chủ DNS gốc sẽ trả về địa chỉ IP của máy chủ DNS cấp cao nhất cho website.

* Máy chủ DNS cấp cao nhất sẽ trả về địa chỉ IP của máy chủ DNS quản lý website. Máy chủ DNS quản lý sẽ trả về địa chỉ IP của trang web cho máy chủ DNS cục bộ.

* Cuối cùng, máy chủ DNS cục bộ sẽ trả về địa chỉ IP của trang web cho máy tính của người dùng. Máy tính của người dùng sẽ sử dụng địa chỉ IP này để kết nối với website.

# LAB 2

## TTL là gì?

* Time to live (TTL) đề cập đến lượng thời gian hoặc “hops” mà một packet được thiết lập để tồn tại trong mạng trước khi bị router loại bỏ

## Time là gì?

* Thời gian (ms) từ khi gửi gói tin đến khi nhận phản hồi.

## SSH

* Dùng password
```
    ssh vuong@192.168.1.99
    vuong@192.168.1.99's password:
```
* Dùng private key
```
    ssh -i /path/key.pem vuong@192.168.1.99
```
* Dùng port custom
```
    ssh -p 1990 vuong@192.168.1.99 
```
## SCP

* FILE
```
    scp file.txt vuong@192.168.1.99:/home
```
* Folder
```
    scp -r folder/ vuong@192.168.1.99:/home
```
## Rsync

* FILE
```
    rsync -avz /home/vuong/file.txt vuong@192.168.1.99:/home/bt/
```
* Folder
```
    rsync -avz /home/vuong/folder/ vuong@192.168.1.99:/home/bt/
```
* rsync incremental (chỉ chuyển các file mới hoặc đã thay đổi)
```
    rsync -avz --progress /home/vuong/ vuong@192.168.1.99:/home/bt/
```
## Cat nội dung 1 file
```
    cat /home/kingvuong28/Desktop/bai_tap/lap1.md
```
## Cat dòng thứ n trong một file
* Một mình lệnh cat không thể lấy một dòng cố định phải kết hợp với lệnh khác
```
    cat -n filename | grep -w '^ *5'
```
## Cat nhiều dòng vào 1 file
```
    cat <<EOF >> /home/kingvuong28/Desktop/bai_tap/lap1.md
    Hi.
    New.
    EOF
```
## Dùng echo để chèn thêm 1 dòng vào cuối file
```
    echo "Thêm mới một dòng" >> /home/kingvuong28/Desktop/bai_tap/lap1.md
```
## Dùng echo để overwrite nội dung trong file
```
    echo "Ghi đè hết file" > /home/kingvuong28/Desktop/bai_tap/lap1.md
```
## Head (hiển thị văn bản theo dòng từ trên xuống)
```
    tail /home/kingvuong28/Desktop/bai_tap/lap1.md
```
## Tail (hiển thị văn bản theo dòng từ dưới lên)
```
    tail /home/kingvuong28/Desktop/bai_tap/lap1.md
```
## Tailf (theo dõi real-time hiện thị dòng vừa thêm)
```
    tailf /home/kingvuong28/Desktop/bai_tap/lap1.md
    tail -f /home/kingvuong28/Desktop/bai_tap/lap1.md
```
## Dùng sed để find and replace một string trong file
```
    sed -i 's/Hi/Hello/' /home/kingvuong28/Desktop/bai_tap/lap1.md
```
## Traceroute
![anh](/ass/traceroute.png)

* Traceroute to vietnix.com (103.200.23.49): traceroute đang cố gắng tìm đường đi từ máy tính của bạn đến máy chủ có địa chỉ IP là 103.200.23.49 (địa chỉ IP của vietnix.com).

* 30 hops max: Trình traceroute sẽ thử tối đa 30 bước (hop) để truy vết đường đi. Nếu không thể tìm thấy đích đến trong 30 bước, quá trình sẽ kết thúc.

* 60 byte packets: Mỗi gói tin mà traceroute gửi đi có kích thước là 60 byte.

* Với dòng đầu tiên hiển thị gateway đi ra, thông số ms là thời gian phản hồi của gói tin

* Với các dòng phía sau thể hiện gói tin đã đi qua các địa chỉ IP nào để đến đích

* Với dấu chấm * là gói tin gửi đi nhưng không nhận phản hồi

## Netstat
* Hiển thị các socket đang listen
```
    sudo netstat -l
```
* Don't resolve hostname
```
    sudo netstat -n
```
* Don't resolve portname
```
    sudo netstat -n
```
* Display process name/PID
```
    sudo netstat -p
```
* Only show tcp socket
```
    sudo netstat -t
```
* Only show udp socket
```
    sudo netstat -u
```
## Sort command
* Sort theo thứ tự tăng dần (mặc định lệnh sort sẽ tăng dần)
```
    sort /home/kingvuong28/Desktop/bai_tap/lap1.md
```
* Sort theo thứ tự giảm dần
```
    sort -r /home/kingvuong28/Desktop/bai_tap/lap1.md
```
* Sort theo column
```
    sort -k<n> /home/kingvuong28/Desktop/bai_tap/lap1.md
```
## Uniq
* Lọc ra các dòng lặp lại trong một file
```
    uniq /home/kingvuong28/Desktop/bai_tap/lap1.md
```
* Lọc ra các dòng lặp lại trong file và đếm số lượng các dòng lặp lại
```
    uniq -c /home/kingvuong28/Desktop/bai_tap/lap1.md
```
## WC (word count)
* Đếm số dòng trong file
```
    wc -l /home/kingvuong28/Desktop/bai_tap/lap1.md
```
* Đếm số kí tự trong file
```
    wc -m /home/kingvuong28/Desktop/bai_tap/lap1.md
```
## Chmod, Chown, Chattr
* Phân quyền bằng số 
```
    chmod 744 /home/kingvuong28/Desktop/bai_tap/lap1.md
```
* Phân quyền bằng chữ
```
    chmod u+rwx,g+rx,o+r /home/kingvuong28/Desktop/bai_tap/lap1.md
```
* Đổi owner user/group
```
    chown vuong /home/kingvuong28/Desktop/bai_tap/lap1.md
```
* Đổi owner group
```
    chown :developers /home/kingvuong28/Desktop/bai_tap/lap1.md
```
* Set Immutable Attribute
```
    chattr +i /home/kingvuong28/Desktop/bai_tap/lap1.md
    chattr -i /home/kingvuong28/Desktop/bai_tap/lap1.md
```
## Find
* Find các file có đuôi .log
```
    find /home/kingvuong28/Desktop/bai_tap -type f -name "*.log"
```
* Find các folder có tên abc
```
    find /home/kingvuong28/Desktop/bai_tap -type d -name "abc"
```
* Find các file có tên abc
```
    find /home/kingvuong28/Desktop/bai_tap -type f -name "abc"
```
* Find các file có tên abc và thực hiện phần quyền read only cho file
```
    find /home/kingvuong28/Desktop/bai_tap -type f -name "abc" -exec chmod 444 {} \;
```
## CP
* CP file
```
    cp /home/kingvuong28/Desktop/bai_tap/lap1.md /home/kingvuong28/Desktop/bai_tap/lap1_backup.md
```
* CP folder
```
    cp -r /home/kingvuong28/Desktop/bai_tap /home/kingvuong28/Desktop/bai_tap_backup
```
## MV (move)
* MV file
```
    mv /home/kingvuong28/Desktop/bai_tap/lap1.md /home/kingvuong28/Desktop/lap1.md
```
* MV folder
```
    mv /home/kingvuong28/Desktop/bai_tap /home/kingvuong28/Desktop/backup_bai_tap
```
## Cut
* Cut kí tự thứ <n> trong string
```
    cut -c<n>
```
* Cut từ kí tự thứ <n> trở về sau
```
    cut -c<n>-
```
* Cut từ kí tự thứ <n> trở về trước
```
    cut -c-<n>
```
## Dig
* Dùng Dig command để kiểm tra resolv record A
```
    dig example.com A
```
* Dùng Dig command để kiểm tra resolv record MX
```
    dig example.com MX
```
* Dùng Dig command để kiểm tra resolv record NS
```
    dig example.com NS
```
* Dùng Dig command để kiểm tra resolv record A, MX, NS với custom DNS
```
    dig @<custom_dns_server> example.com A/MX/NS
```
## Tar/zip/unzip
* Nén file tar.gz 
```
    tar -czvf backup.tar.gz /home/kingvuong28/Desktop/bai_tap
```
* Giải nén file tar.gz 
```
    tar -xzvf backup.tar.gz
```
* Nén file tar.gz 
```
    zip backup.zip /home/kingvuong28/Desktop/bai_tap
```
* Giải nén file tar.gz 
```
    unzip backup.zip
```
## Mount/Umount
* Add thêm một ổ cứng sdb ~ 5gb
```
    sudo fdisk /dev/sdb
    * Bấm phím n để tạo phân vùng mới.
    sudo mkfs.ext4 /dev/sdb1
    * format lại ổ đĩa
```
* Kiểm tra được có bao nhiêu ổ cứng trên máy chủ
```
    sudo fdisk -l
```
* Mount ổ cứng vào /mnt/test
```
    sudo mount /dev/sdb1 /test
```
* Umount /mnt/test
```
    sudo umount /test
```
## Symbolic Links, Hard Links
* Định nghĩ Sym Link
    * Sym Link là một fiel đặc biệt trỏ đến một file hoặc thư mục khác tương tự shortcut trên win, nếu file gốc bị xóa sẽ trở thành broken link và không hoạt động.
* Định nghĩ Hard Link
    * Hard link tham chiếu trực tiếp đến dữ liệu trên ổ đĩa cũng với file gốc đều trỏ về inode(dữ liệu thực), nên file gốc có bị xóa thì hard link vẫn hoạt động bình thường.(không hỗ trợ Cross filesystem và chỉ dùng cho file)
* Ví dụ về Sym Link
```
    ln -s /home/kingvuong28/Desktop/bai_tap/lap1.md /home/kingvuong28/Desktop/lap1_symlink.md
```
* Ví dụ về Hard Link
```
    ln /home/kingvuong28/Desktop/bai_tap/lap1.md /home/kingvuong28/Desktop/lap1_hardlink.md
```

## LS

* Liệt kê danh sách file/thư mục
```
    ls /home/kingvuong28/Desktop/bai_tap
    ls /home/kingvuong28/Desktop/bai_tap/lap1.md
```
* Liệt kê danh sách file/thư mục và thuộc tính
```
    ls -l /home/kingvuong28/Desktop/bai_tap
    ls -l /home/kingvuong28/Desktop/bai_tap/lap1.md
```
* Show file ẩn
```
    ls -a /home/kingvuong28/Desktop/bai_tap
```

## PS
* Show tiến trình
```
    ps
```
* Hiển thị đầy đủ thông tin của tất cả tiến trình
```
    ps aux
```
* Tìm tiến trình theo tên
```
    ps aux | grep <tên>
```
## Kill tiến trình
```
    kill [PID]
    * PID process ID tìm bằng cách ps
    * Có thể thể kill theo tên bằng cú pháp:
    pkill <tên_process>
    killall <tên_process>
```

## Top command

* Kiểm tra tài nguyên cpu đang sử dụng của một vài process đang chạy
    ![anh](/ass/top.png)

* Load average
    * Đại diện cho số tiến trình đang chờ CPU trong 1, 5 và 15 phút gần nhất. Nếu bạn có 1 CPU, load > 1 nghĩa là hệ thống bắt đầu quá tải.

* Giải thích ý nghĩa của us, sy, ni, id, wa, hi, si, st

    | Viết tắt | Nghĩa        | Giải thích                                              |
    |----------|--------------|---------------------------------------------------------|
    | `us`     | user         | % CPU dùng cho tiến trình người dùng                    |
    | `sy`     | system       | % CPU cho kernel/system                                 |
    | `ni`     | nice         | % CPU cho tiến trình `nice` (ưu tiên thấp)              |
    | `id`     | idle         | % CPU không sử dụng (nhàn rỗi)                          |
    | `wa`     | wait         | % CPU chờ I/O (đọc/ghi ổ đĩa)                           |
    | `hi`     | hardware IRQ | % CPU xử lý ngắt cứng                                   |
    | `si`     | software IRQ | % CPU xử lý ngắt mềm                                    |
    | `st`     | steal time   | % CPU bị "mượn" bởi máy ảo khác nếu dùng virtualization |

* Zombie Process là gì?
    * Là tiến trình đã kết thúc, nhưng PID vẫn tồn tại vì tiến trình cha chưa "thu dọn" (dùng wait()). Không sử dụng tài nguyên, nhưng nếu quá nhiều có thể làm đầy bảng tiến trình (PID table).

* Sleeping Process là gì?
    * Là tiến trình đang ngủ, chờ 1 sự kiện (VD: chờ I/O, input người dùng).Rất phổ biến, và thường không gây ảnh hưởng hiệu suất.

## Free
* Dùng -h để dung lượng ở dạng MB và GB dễ đọc hơn

* Giải thích ram used, total, free, shared, buff/cahche, available

    | Cột          | Ý nghĩa                                                                  |
    | ------------ | ------------------------------------------------------------------------ |
    | `total`      | Tổng dung lượng RAM (bộ nhớ vật lý)                                      |
    | `used`       | Tổng RAM đang được sử dụng thực tế (bao gồm shared + buff/cache)         |
    | `free`       | RAM hoàn toàn chưa dùng tới                                              |
    | `shared`     | RAM đang được chia sẻ giữa các tiến trình (thường là tmpfs)              |
    | `buff/cache` | RAM dùng cho **buffer (disk I/O)** và **cache (cache hệ thống)**         |
    | `available`  | RAM còn **khả dụng thực sự** cho ứng dụng, **sau khi trừ cache/buffers** |

## DF

* Xem dung lượng disk

    ![anh](/ass/df.png)

* Phân vùng / là gì

    * / gọi là root partition – thư mục gốc của toàn bộ hệ thống Linux. Tất cả thư mục con như /home, /etc, /var, /usr đều nằm trong /. Nếu không chia phân vùng riêng cho /home, /var,... thì tất cả dùng chung dung lượng của /.