# Principles of an API Gateway

  API Gateway là điểm vào để truy cập các capabilities khác nhau của một platform
phức tạp thông qua các API được xác định rõ. Để minh họa khái niệm này, giả sử
bạn xây dựng một trang web cho một cửa hàng điện tử và thiết kế API cho trang
chủ của nó. API này, được lưu trữ trên một single service, trả về danh sách
các thiết bị điện tử trong cửa hàng cộng với các mặt hàng được đề xuất. Bây giờ
hãy tưởng tượng cửa hàng phát triển theo thời gian và bạn quyết định xây dựng
các dịch vụ cụ thể cho người tiêu dùng và điện thoại di động. Ngoài ra, bạn quyết
định xây dựng một service đề xuất dành riêng. Do đó, để hiển thị trang chủ, giờ
đây bạn phải tích hợp với nhiều dịch vụ siêu nhỏ, tăng tổng số vòng tròn được
thực hiện cho phần phụ trợ. Nếu một danh mục điện tử mới được thêm vào, toàn bộ
quá trình tái hòa nhập sẽ lặp lại. API Gateway có thể hấp thụ sự phức tạp này
bằng cách kết nối với trang chủ một lần và sắp xếp các yêu cầu tới càng nhiều
service cần thiết. Thêm vào đó, bạn có thể phát triển độc lập các service siêu
nhỏ của mình mà không phải thay đổi tích hợp client.

  Risk team trong PayPal đã phải đối mặt với một tình huống tương tự - chúng tôi
đã phát triển hệ sinh thái các risk services để tạo ra các trạm kiểm soát tinh
vi, nhưng việc kết hợp chặt chẽ với các client thanh toán PayPal dẫn đến ma sát
trong việc di chuyển. Chúng tôi đã xây dựng Cổng API để tách các services của
mình với client. Điều này dẫn đến các API gọn gàng, được xác định rõ ràng được
lưu trữ trong một single gateway service, trong đó một API có thể tận dụng các
khả năng của Risk platform, chống lại các API riêng lẻ bị phân rã. Là một điểm
liên lạc duy nhất của nền tảng của bạn, bắt buộc phải thiết kế API Gateway service
một cách cẩn thận. Trong bài viết này, tôi sẽ trình bày các nguyên tắc chính của
API Gateway và minh họa cách các nguyên tắc này được sử dụng để thiết kế một
ứng dụng Gateway mạnh mẽ, có thể mở rộng và hoạt động tốt.

<center>
  <img src="/Microservices/img/1_DAJdjBHEq37GXCwQ9GcrtQ.png">
</center>

## 1. Scalability and Performance

  Nếu platform của bạn phục vụ hàng triệu/tỷ yêu cầu mỗi này, sẽ rất hợp lý khi
đầu tư vào công nghệ phù hợp có thể hỗ trợ lưu lượng truy cập cao mà không ảnh
hưởng đến hiệu suất. Asynchronous non-blocking I/O (NIO) dựa trên frameworks là
phù hợp nhất cho các trường hợp như vậy. Một trong những frameworks ưa thích là
Netty và sử dụng nó làm bộ chứa HTTP đã được chứng minh là mang lại hiệu suất
tăng mong muốn. Thông thường, API Gateway thực hiện các cuộc gọi đến nhiều backend services cho một request. Nếu các backend microservices của bạn mất nhiều thời
gian để xử lý, tài nguyên thread trong Gateway của bạn có thể bị cạn kiệt gây ra
tình trạng đói và do đó sẽ ảnh hưởng đến Cổng ATB (Availability to Business). Do
đó, viết API code theo cách khai báo bằng cách sử dụng coding pattern là lựa
chọn phù hợp cho các ứng dụng đó. Ví dụ: người ta có thể sử dụng CompleteableFuture
của JAVA 8 làm reactive abstraction. Để kết luận, đối với một ứng dụng có lưu
lượng truy cập cao như API Gateway, tốt nhất nên sử dụng framework NIO và
reactive programming model để sử dụng tối ưu các tài nguyên mang lại hiệu suất cao. Biểu ồ sau so sánh độ trễ (tính bằng ms) của ứng dụng Gateway sử dụng Tomcat và Netty làm HTTP container.

<center>
  <img src="/Microservices/img/1_fSznVKCVZxS1wVC8K_k6eQ.png">
</center>

## 2. Request orchestration

  Một trong những chức năng chính của gateway application là loại bỏ các
API request tới nhiều backend microservices. Để làm điều này một cách hiệu quả,
người ta cần xem xét các khía cạnh quan trọng sau:

* Protocol Translation: request API gửi đến có thể ở một định dạng cụ thể có thể khác với các downstream services của bạn. Ví dụ: API Gateway nhận các requests
HTTPS RESTful, trong khi một số downstream services. có thể mong đợi Protobuf.
Gateway server cần đảm bảo rằng các connectors và translators được tích hợp.

* Serial/Parallel invocation patterns: API Gateway sẽ cho phép gọi các downstream
services trong serial or parallel patterns. Cùng với đó, hỗ trợ cho conditional
routing  nên có sẵn.

* Configurable routing: Routing patterns sẽ không yêu cầu code mới (hoặc code tối
thiểu) để tạo route mới đến backend microservice. Nói cách khác, service routing
nên được điều khiển bởi các configurations. Điều này đảm bảo rằng cơ sở Gateway
code vẫn duy trì trạng thái tinh gọn khi các API mới được sử dụng.

## 3. Response translation

  API Gateway có thể điều phối các requests tới nhiều downstream services như
chúng tôi đã đề cập trong phần trên. Tuy nhiên, client có thể cần một single
response. Trong các trường hợp như vậy, API Gateway sẽ chịu trách nhiệm hợp nhất các response khác nhau và gửi một response duy nhất, có ý nghĩa. Một cách hiệu
quả để đạt được điều này là một bảng tra cứu. Một bảng tra cứu xác định các
hoán vị của các câu trả lời và đưa ra kết quả cuối cùng cho một sự kết hợp cụ
thể. Ví dụ: giả sử một gateway service gọi n microservice, nó có thể hợp nhất n
phản hồi với một bằng cách tham khảo bảng tra cứu và sau đó gửi kết quả phù hợp
với kết hợp response.

## 4. Enabling Fallback

   Fallback pattern được định nghĩa là routing request đến lightweight version of
service của bạn trong trường hợp lỗi trong primary service của bạn. Routing
pattern này ngăn chặn sự thất bại hoàn toàn của business của bạn và hoạt động
như một tuyến phòng thủ cuối cùng. Nếu việc xây dựng một lightweight service quá
tốn kém, API Gateway có thể chỉ cần route cuộc gọi đến một default function, do
đó cho phép service của bạn cung cấp best-effort response thay vì thất bại hoàn
toàn.

## 5. Rate limiting and Access Control

  Mặc dù một mặt, API Gateway cho phép business của bạn hiển thị các hoạt động
của mình thông qua API, nhưng cũng có trách nhiệm bảo vệ các microservices của
bạn khỏi các vụ nổ lưu lượng truy cập và truy cập trái phép. Xây dựng Rate Limiter
trong Gateway đảm bảo lưu lượng truy cập đột ngột không làm tổn hại đến ứng dụng
của bạn và do đó bảo vệ khỏi các cuộc tấn công DDoS có chủ ý/không chủ ý. Một số
API nhất định trong nền tảng của bạn có thể nhạy cảm và bạn muốn điều chỉnh quyền
truy cập vào chúng. API Gateway có thể hạn chế quyền truy cập API và chỉ cho phép
các ứng dụng được ủy quyền gọi các dịch vụ của bạn

## 6. Monitoring and Analytics

  Đối với bất kỳ nền tảng nào, điều quan trọng là phải có bảng điều khiển giám sát
hiển thị các số liệu quan trọng của hệ thống và ứng dụng. Vì API Gateway là mặt
tiền của nền tảng, nên việc xây dựng các tính năng giám sát và cảnh báo ở lớp này
là điều hợp lý. Một vài ví dụ về các số liệu quan trọng cần xem xét là ATB
(Availability to Business), Độ trễ API và Số lỗi API cung cấp cái nhìn sâu sắc
về sức khỏe của chính API Gateway. Ngoài ra, API Gateway có thể phát ra thông
tin hữu ích từ request và response API đến cửa hàng ngoại tuyến (tốt nhất là
[Hadoop](https://hadoop.apache.org/)), có thể cung cấp các phân tích business
chi tiết về API

## API Gateway for Risk Platform at PayPal

Tóm lại, trong thế giới của nhiều microservices đa dạng, API Gateway là một lựa
chọn tự nhiên để thể hiện các khả năng theo cách được xác định rõ. Các tham số
khác nhau phải được xem xét trong khi thiết kế  API Gateway, bao gồm công nghệ
phù hợp về khả năng mở rộng, các design patterns hỗ trợ
orchestration/consolidation, fallback services, rate limiters, kiểm soát truy
cập vào API và các tính năng phân tích và giám sát rộng rãi. Tại PayPal đã xây
dựng Cổng API theo các nguyên tắc này, đóng vai trò là điểm duy nhất cho tất cả
các API rủi ro trong PayPal. Hiện tại, nó phục vụ nhiều triệu cuộc gọi API trong
một ngày và có độ trễ chỉ 1,4 ms (trung bình) và 3 ms (99% ile). Việc sử dụng CPU
và bộ nhớ thấp ngay cả khi lưu lượng cao nhất và do đó, chúng tôi dễ dàng phục vụ
các giao dịch đang phát triển mà không tăng dung lượng theo chiều ngang.
