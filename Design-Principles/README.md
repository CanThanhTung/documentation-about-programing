<center> <h1>Design Principles</h1> </center>

## What are Software Design Principles?

Software design principles nết quan trọng, cần thiết. nó đại diện một tập hợp
hướng dẫn giúp tránh có một bad design. Design priciples là liên kết với
Robert Martin, người đã tập hợp chúng trong "Agile Software Development: Principles
, Patterns, and Practices". Theo Robert Martin, có 3 đặc điểm quan trọng của một
bad design cần tránh:

- Rigidity: Thật khó để thay đổi vì mọi thay đổi đều ảnh hưởng đến quá nhiều
  phần khác của hệ thống.
- Fragility - Khi bạn thực hiện thay đổi, các phần không mong muốn của hệ thống
  sẽ bị hỏng.
- Immobility - Thật khó để sử dụng lại trong một ứng dụng khác bởi vì nó không
  thể được gỡ bỏ khỏi ứng dụng hiện tại.

> Design principles sẽ giúp bạn tạo ra một thiết kế modular sạch sẽ, dễ dàng test,
> debug và maintain trong tương lai.

Các Object-Oriented design principles là core của Object-Oriented Programming,
nhưng các developers hiện chú trọng nhiều hơn vào các design patterns như Singleton,
Observer pattern ... mà không chú ý vào Object-Oriented design principles.

Trong bài viết này tôi đã ghi lại tất cả các object-oriented design principles
quan trọng và đưa nó vào đây để cùng thảo luận về chúng. Những điều này ít nhất
sẽ cung cấp cho mọi người một số ý tưởng về những gì chúng đang có và những lợi
ích chúng cung cấp.

## 10 Object - Oriented and SOLID Desgin principles

### DRY (don’t repeat yourself)

  DRY là viết tắt cho "Don't Repeat Yourself" là một principle căn bản của
software development nhằm mục đích giảm sự lặp lại của thông tin. DRY principle
được tuyên bố là 'Mỗi phần piece of knowledge hoặc logic phải có một biểu diễn
duy nhất, rõ ràng trong một hệ thống.'

#### Violations of DRY

  'We enjoy typing' (hoặc 'Wasting everyone's time.'): 'We enjoy typing', nghĩa
là viết đi viết lại cùng một đoạn code hoặc logic. Sẽ rất khó để quản lý mã và
nếu logic thay đổi, thì chúng ta phải thay đổi ở tất cả những nơi chúng ta đã
code, do đó làm lãng phí thời gian của mọi người.

#### How to Achieve DRY

  Để tránh vi phạm DRY, hãy chia system của bạn thành nhiều phần. Chia code và
logic của bạn thành các đơn vị tái sử dụng nhỏ hơn và sử dụng code đó bằng cách
gọi nó ở nơi bạn muốn. Đừng viết các phương thức dài, nhưng phân chia logic và
cố gắng sử dụng phần hiện có trong method của bạn.

#### DRY Benefits

  Less code is good: Nó tiết kiệm thời gian và công sức, dễ maintain và cũng làm
giảm bug.

### Encapsulate What May Change

  Đầu tiên có một vài câu hỏi đơn giản - bạn có cần mua điện thoại mới mỗi lần
bạn muốn tải xuống và dùng thử ứng dụng mới không? Hoặc bạn có phải đi mua antenna
mới cho TV khi bạn muốn thay đổi gói kênh đã đăng ký không? Hoặc bạn cần phải nối
lại nguồn cung cấp điện trong nhà khi bạn mua một thiết bị mới? Không. Tại sao?
Bởi vì mặc dù mỗi thiết bị mới có thể có Voltage và Ampere khác nhau, nhưng thiết
bị đi kèm với bộ chuyển đổi điện chịu trách nhiệm kết nối thiết bị với nguồn điện
tiêu chuẩn tại nhà bạn. Nói cách khác, những thay đổi xảy ra trong yêu cầu năng
lượng của các thiết bị gia dụng được gói gọn từ nguồn cung cấp điện tiêu chuẩn
tại nhà của bạn, do đó đảm bảo rằng. Những gì vẫn giữ nguyên được cách ly với
những gì thay đổi thường xuyên.
