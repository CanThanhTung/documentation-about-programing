# Unit Testing và tại sao nên viết test

<h3 aglin="center">
<img src="/Test/img/photo-1518349619113-03114f06ac3a.jpeg"/>
</h3>

Tất cả đều biết rằng testing cho application là rất quan trọng, Vì nó đảm bảo
security, sự hài lòng của khách hàng và tiết kiệm tiền trong thời gian dài.
Đôi khi nó cũng có thể cứu mạng: Máy bay của China Airlines bị rơi do lỗi
phần mềm vào ngày 26 tháng 4 năm 1994, mất 264 mạng. Trong software testing,
unit testing là cấp độ testing đầu tiên trong đó hầu hết các vấn đề có thể
được khắc phục, giúp tiết kiệm thời gian. Hãy hiểu các Unit testing là gì.

## Unit Testing là gì

  Unit testing là testing of code để đảm bảo rằng nó thực hiện nhiệm vụ chích
sác. Nó test code ở mức rất thấp nhất có thể - các thành phần riêng lẻ của một
software application được tested trong giai đoạn development. Unit test
thường được thực hiện bởi các developers thay vì testers. Trong unit test,
một chức năng của một phần của code như class, được test để xác minh tính chính
xác của nó bằng trình điều khiển, Unit testing frameworks, mock objects, và stubs

  Nó là chìa khóa để viết code clean hơn, code có thể bảo trì dễ dàng hơn.

  Nhưng, đôi khi có những câu hỏi về định nghĩa của các từ khoá khi nói đến unit
test. Chẳng hạn, chính xác thì unit là gì? mock có nghĩa là gì, Làm thế nào để
biết liệu có thực sự làm unit test không?

<h3 aglin="center">
<img src="/Test/img/level-of-testing.png"/>
</h3>

## Unit là gì?

   Câu hỏi đầu tiên xuất hiện khi thảo luận về unit testing là, unit là gì? Bạn
có thể thực hiện unit test mà không cần biết unit là gì.

   Khi nói đến unit test, một unit là phần có thể test nhỏ nhất của bất kỳ
software nào. Nó thường có một hoặc một vài inputs và thường là một single
output. Trong procedural programming, một unit có thể là một individual program,
function, procedure,... Trong object-oriented programming, unit nhỏ nhất là một
method, mà có thể thuộc về một base/super class, abstract class hoặc
derived/child class.(Một số coi module của an application là một unit. Điều này
không được khuyến khích, vì có thể sẽ có nhiều unit riêng lẻ trong module đó.)
Unit testing frameworks, drivers, stubs, and mock/fake objects được sử dụng để
hỗ trợ unit testing.

## Một Isolation Framework là gì?

   Thông thường, các developer đã sử dụng mocking framework để mô tả code, nó
cung cấp fake services để cho phép các class được test một cách cô lập.

   Tuy nhiên, một mock là một loại cụ thể của fake class, cùng với các stub.
Vì vậy, có lẽ chính xác hơn khi sử dụng isolation framework thay vì mocking
framework.

   Một isolation framework hữu ích sẽ cho phép tạo ra dễ dàng cả hai loại mock
và stub

   Fake cho phép bạn kiểm tra một class một cách cô lập bằng cách cung cấp các
triển khai của các dependencies mà không yêu cầu các dependencies thực sự.

   Định nghĩa: Một isolation framework là một collection của code cho phép tạo
ra fake dễ dàng. Một fake class là bất kỳ class nào cung cấp đủ các chức năng
để giả lập rằng đó là một dependency cần thiết cho một bài test. Có hai loại fake: stub và mock.

# Stubs

   Một Stub là một class mà nó thực hiện mức tối thiểu tuyệt đối, là một real
dependency. Nó không cung cấp các chức năng theo yêu cầu của test, ngoại trừ
việc xuất hiện để thực hiện một interface nhất định từ một base class nhất định.

   Khi Class Under Test(CUT) gọi nó, một stub thường không làm gì cả. Stub hoàn
toàn ngoại vi để test Class Under Test(CUT) và tồn tại hoàn toàn để cho phép Class Under Test(CUT) chạy.

   Một ví dụ điển hình là một stub cung cấp logging services. Class Under
Test(CUT) có thể cần triển khai interface `ILogger` để thực thi, nhưng không có
test case nào quan tâm đến việc logging.

   Bạn đặc biệt không muốn Class Under Test(CUT) logging bất cứ điều gì. Do đó,
Stub giả vờ làm logging service bằng cách thực hiện interface, nhưng việc thực
hiện đó thực sự không làm gì cả.

   Hơn nữa, trong khi một stub có thể trả về data để giữ cho Class Unde Test(CUT)
chạy, nó không bao giờ có thể thực hiện bất kỳ hành động nào sẽ làm fail một
test case. Nếu có, thì nó không còn là stub, và nó trở thành một mock.

   Định nghĩa: Một Stub là một fake không có ảnh hưởng đến việc pass/fail của
test case và nó tồn tại hoàn toàn để cho phép test case chạy.

## Mocks

   Mocks phức tạp hơn một chút. Mocks làm những gì stub làm, trong đó chúng cung
cấp một triển khai fake của một dependency cần thiết của Class Under Test(CUT).

   Tuy nhiên, chúng vượt xa hơn là stub bằng cách ghi lại sự tương tác giữa
chính nó và Class Under Test(CUT). Một mock giữ một bản ghi tất cả các tương
tác với Class Under Test(CUT) và báo cáo lại, vượt qua test case nếu Class Under
Test(CUT) xử lý đúng và test case faild nếu thực hiện không đúng.

  Do đó, nó là mock, chứ không phải chính Class Under Test(CUT), sẽ xác định xem
test có vượt qua hay không.

  Đây là một ví dụ.

  Giả sử bạn có một class được gọi là `WidgetProcessor`. Nó có hai dependencies,
`ILogger` và `IVerifier`. Để test `WidgetProcessor`, bạn cần fake cả hai
dependencies đó.

  Tuy nhiên, để test `WidgetProcessor`, bạn sẽ muốn thực hiện hai test case -
một trong đó bạn stub `ILogger` và test sự tương tác với `IVerifier`, và một
test case khác khi bạn stub `IVerifier` và test sự tương tác với `ILogger`.

  Cả hai đều yêu cầu fake, nhưng trong mỗi trường hợp, bạn sẽ cung cấp một
stub class cho class này và mock class cho class kia.

  Chúng ta hãy xem xét kỹ hơn một chút về kịch bản đầu tiên, trong đó chúng ta
sẽ loại bỏ `ILogger` và sử dụng một mock cho `IVerifier`. Stub chúng ta đã thảo
luận; Bạn có thể viết một triển khai của `ILogger` trống hoặc bạn sử dụng một
isolation framework để triển khai interface để không làm gì cả.

  Tuy nhiên, fake cho `IVerifier` trở nên thú vị hơn một chút - nó cần một mock
class. Nói quá trình xác minh một **widget** mất hai bước. Đầu tiên, bộ xử lý cần
xem liệu **widget** có trong hệ thống hay không, và sau đó, nếu có, bộ xử lý cần
kiểm tra xem **widget** có được cấu hình đúng không.

  Do đó, nếu bạn đang test `WidgetProcessor`, bạn cần chạy test để kiểm tra xem
`WidgetProcessor` có thực hiện cuộc gọi thứ hai hay không nếu nó được trả lại
**True** từ cuộc gọi đầu tiên.

  Bài kiểm tra này sẽ yêu cầu mock class để làm hai việc. Đầu tiên, nó cần trả
về **True** từ cuộc gọi đầu tiên và sau đó nó cần theo dõi xem cuộc gọi cấu hình
kết quả có được thực hiện hay không.

  Sau đó, nó trở thành công việc của mock class để cung cấp thông tin pass/fail.
Nếu cuộc gọi thứ hai được thực hiện sau cuộc gọi đầu tiên trả về **True**, thì
test case sẽ vượt qua. Nếu không, test case faild.

  Đây là những gì làm cho mock class này trở thành một mock: Bản thân mock có
chứa thông tin cần được test cho các tiêu chí pass/fail.

  Định nghĩa: Mock là một fake theo dõi hành vi của class được test và vượt qua
hoặc thất bại test dựa trên hành vi đó.

  Hầu hết các isolation framework bao gồm khả năng theo dõi rộng rãi và tinh
vi chính xác những gì xảy ra bên trong một mock class.

  Chẳng hạn, các mock không chỉ có thể biết liệu một method đã cho có được gọi
hay không, mà chúng còn có thể theo dõi số lần các method đã cho được gọi và
các parameter được truyền cho các cuộc gọi đó.

  Stub tương đối đơn giản để xây dựng và sử dụng, nhưng mock có thể khá phức tạp.

  Nhưng một lần nữa, toàn bộ vấn đề ở đây là test các class của bạn một cách
cô lập; Bạn muốn Class Under Test(CUT) của bạn có thể thực hiện nhiệm vụ của mình
mà không cần các dependencies bên ngoài.

## Tại sao phải unit testing?

  Có rất nhiều sự kháng cự, không thích khi làm unit testing. Nhiều developer
dường như xem nó là một sự lãng phí thời gian hoặc nó sẽ chỉ trì hoãn việc hoàn
thành một dự án theo thời hạn. Họ cảm thấy rằng họ không thể nhận được bất kỳ
lợi ích nào từ nó.

  Dưới đây là các lý do tại sao cần unit testing.

### Unit testing sẽ tìm thấy bug.

  Hãy viết test của bạn khi bạn thực hiện hoặc viết test sau khi code được viết,
unit test sẽ tìm thấy bug.

  Khi bạn viết một suite đầy đủ các test case xác định hành vi dự kiến, đối
với một class nhất định, mọi thứ trong class đó không phải là hành vi như mong
đợi sẽ được tìm thấy.

### Unit testing sẽ tránh bug.

  Một suite unit test đầy đủ và kỹ lưỡng sẽ giúp đảm bảo rằng bất kỳ bug nào
xâm nhập vào code của bạn sẽ được thấy ngay lập tức.

  Thực hiện một thay đổi code có thể sinh ra bug và các test case của bạn có thể
cho thấy nó. Nếu bạn tìm thấy một bug nằm ngoài các test case, bạn có thể viết
các test case cho nó để đảm bảo rằng bug không bao giờ quay trở lại.

### Unit test tiết kiệm thời gian.

  Việc viết unit test có tiết kiệm thời gian hay không gây tranh cãi nhất về
unit test. Hầu hết các developer tin rằng viết unit test mất nhiều thời gian hơn
là nó tiết kiệm. Tôi không nghĩ rằng là như vậy - thực tế, tôi lập luận ngược
lại.

  Viết unit test giúp đảm bảo rằng code của bạn hoạt động đúng như thiết kế,
ngay từ đầu. Các unit test xác định những gì code của bạn nên làm, và do đó
bạn đã giành được thời gian viết code làm những việc mà nó không nên làm.

  Mỗi unit test trở thành một bài kiểm tra hồi quy, đảm bảo rằng mọi thứ tiếp
tục hoạt động như được thiết kế trong khi bạn phát triển. Chúng giúp đảm bảo
rằng những thay đổi tiếp theo không phá vỡ mọi thứ đã xây dựng.

  Chúng giúp đảm bảo rằng những gì bạn viết lần đầu tiên là điều đúng đắn. Tất
cả những lợi ích này tiết kiệm thời gian cả trong ngắn hạn và dài hạn.

### Unit test mang lại sự yên tâm

  Có một suite test case đầy đủ và kỹ lưỡng bao gồm các chức năng hoàn chỉnh của
code của bạn có thể khó đạt được, nhưng có nó sẽ giúp bạn yên tâm.

  Bạn có thể chạy tất cả các suite test case đó và biết rằng code của bạn hoạt
động như mong muốn. Bạn có thể cấu trúc lại và thay đổi code, biết rằng nếu bạn
phá vỡ bất cứ điều gì, bạn sẽ biết ngay lập tức.

  Biết trạng thái của code của bạn, rằng nó hoạt động và bạn có thể cập nhật và
cải thiện nó mà không sợ một điều gì đó sảy ra.

  Nhiều người trong chúng ta có đoạn code phức tạp mà chúng ta sợ chạm vào,
nhưng với unit test, bạn sẽ không có loại code đó. Unit test loại bỏ nỗi sợ
chạm vào những đoạn code phức tạp.

### Unit test là tài liệu sử dụng đúng cách của một class.

  Một trong những lợi ích của unit test là các test case của bạn có thể xác định
cho các developer tiếp theo cách sử dụng đúng các các hành vi của class đó.

  Các unit test trở thành các ví dụ đơn giản về cách code của bạn hoạt động,
những gì được dự kiến ​​sẽ làm và cách sử dụng code phù hợp.

## kết luận

  Được rồi, nếu tất cả những điều đó không thuyết phục bạn làm unit testing, tôi
cũng không biết làm gì.

  Tôi hy vọng rằng sau khi đọc song, bạn sẽ có một ý nghĩ tốt hơn về việc unit
testing là gì và tại sao nên làm điều đó. Tôi nghĩ rằng bạn khó có thể tìm thấy
bất cứ ai đã hối tiếc về thời gian viết unit testing. Vì vậy, nếu bạn không có
unit testing, hãy cân nhắc bắt đầu ngay.
