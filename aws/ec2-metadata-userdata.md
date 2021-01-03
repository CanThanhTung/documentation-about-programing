# AWS EC2 Instance Metadata với Userdata

 * Instance **metadata** và **user-data** được sử dụng cho việc cấu hình, thông tin và quản lý 
   EC2 instance đó.
 * Instance **metadata** và **user-data** có thể được truy cập từ trính trong EC2 instance.
 * **metadata** và **user-data** không được bảo vệ bởi các phương pháp authentication hoặc
   cryptographic nào. Bất kỳ ai có thể truy cập vào instance cũng có thể xem **metadata** và
   **user-data** của instance đó vì vậy không được sử dụng **metadata** và **user-data** để 
   lưu bất kỳ thông tin nhạy cảm nào như mật khẩu, dữ liệu của user
 * Cả **metadata** và **user-data** đều có sẵn ở địa chỉ IP 169.254.169.254
 * **metadata** và **user-data** có thể truy xuất đơn giản bằng **CRUL** or **GET** command
   những request này không mất phí.

## Instance Metadata
 * Instance metadata là dữ liệu chứa thông tin về instance và cho phép bạn biết các thông tin
   về instance mà bạn cần.
 * Được chia thành hai loại
    * Instance metadata
        * Bao gồm **metadata** của instance như instance id, AMI id, hostname, ipaddress, role ...
    * Dynamic data
        * Được tạo khi instance launch như instance identity documents, instance monitoring ...
        * có thể lấy được từ http://169.254.169.254/latest/dynamic/
 * Có thể sử dụng cho quản lý và cấu hình lúc instance running
 * cho phép truy xuất tới **user-data** được chỉ định khi launch instance

 ## User Data

 * **user-data** có thể sử dụng cho bootstrap (launching commands khi machine start) 
   EC2 instance 
 * Được cung cấp khi EC2 instance khởi động và được thưc thi tại thời gian boot
 * có thể ở dạng parameters hoặc script do người dùng xác định được thực thi khi instance được
   khởi chạy
 * Có thể được sử dụng để build các AMI
 * có thể được truy xuất từ ​​http://169.254.169.254/latest/user-data
 * Theo mặc định, các **user-data** script và cloud-init directives chỉ chạy trong boot cycle
   khi một EC2 instance khởi chạy.
 * Nếu stop một instance, sửa đổi **user-data** và start instance, **user-data** mới sẽ không
   được thực thi tự động.
 * Tuy nhiên, **user-data** script và cloud-init directives có thể được cấu hình bằng 
   **mime multi-part file**. **Mime multi-part file** cho phép script ghi đè **user-data** được
    thực thi trong  cloud-init package.
 * Được coi là dữ liệu không rõ ràng và được trả về nguyên trạng.
 * được giới hạn ở 16 KB. Giới hạn này áp dụng cho dữ liệu ở dạng raw, không phải ở dạng được 
   mã hóa base64.
 * Phải được mã hóa base64 trước khi được gửi tới API. EC2 command line tools thực hiện mã hóa
   base64. Dữ liệu được giải mã trước khi được đưa ra cho instance.

 ## Cloud-Init & EC2Config

 * Cloud-Init và EC2Config cung cấp khả năng phân tích cú pháp kịch bản dữ liệu người dùng trên phiên bản và chạy các hướng dẫn
 * Cloud-Init
    * Amazon Linux AMI hỗ trợ Cloud-Init, là một ứng dụng mã nguồn mở được xây dựng bởi Canonical.
    * được cài đặt trên Amazon Linux, Ubuntu và RHEL AMIs
    * cho phép sử dụng tham số Dữ liệu người dùng EC2 để chỉ định các hành động để chạy trên phiên bản tại thời điểm khởi động
    * Dữ liệu người dùng được thực thi trong lần khởi động đầu tiên bằng Cloud-Init, nếu dữ liệu người dùng bắt đầu bằng #!
 * EC2Config
    * EC2Config được cài đặt trên Windows Server AMIs
    * Dữ liệu người dùng được thực thi trong lần khởi động đầu tiên bằng Cloud-Init (về mặt kỹ thuật EC2Config phân tích cú pháp hướng dẫn) nếu dữ liệu người dùng bắt đầu bằng script hoặc powershell
    * Dịch vụ EC2Config được khởi động khi phiên bản được khởi động. Nó thực hiện các tác vụ trong quá trình khởi động phiên bản ban đầu (một lần) và mỗi lần bạn dừng và khởi động phiên bản.
    * Nó cũng có thể thực hiện các nhiệm vụ theo yêu cầu. Một số tác vụ này được bật tự động, trong khi những tác vụ khác phải được bật theo cách thủ công.
    * sử dụng các tệp cài đặt để kiểm soát hoạt động của nó
    * dịch vụ chạy Sysprep, một công cụ của Microsoft cho phép tạo AMI Windows tùy chỉnh có thể được sử dụng lại.
    * Khi EC2Config gọi Sysprep, nó sẽ sử dụng các tệp cài đặt trong EC2ConfigService Settings để xác định các thao tác cần thực hiện.