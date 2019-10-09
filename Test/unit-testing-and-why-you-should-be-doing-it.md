# Unit Testing và tại sao nên viết test

<h1 aglin="center">
<img src="/Test/img/photo-1518349619113-03114f06ac3a.jpeg"/>
</h1>

Tất cả đều biết rằng testing cho application là rất quan trọng, Vì nó đảm bảo
security, sự hài lòng của khách hàng và tiết kiệm tiền trong thời gian dài.
Đôi khi nó cũng có thể cứu mạng: Máy bay của China Airlines bị rơi do lỗi
phần mềm vào ngày 26 tháng 4 năm 1994, mất 264 mạng. Trong software testing,
unit testing là cấp độ testing đầu tiên trong đó hầu hết các vấn đề có thể
được khắc phục, giúp tiết kiệm thời gian. Hãy hiểu các Unit testing là gì.

<h1 aglin="center">
<img src="/Test/img/level-of-testing.png"/>
</h1>

## Unit Testing là gì

  Unit testing là testing of code để đảm bảo rằng nó thực hiện nhiệm vụ chích sác.
Nó test code ở mức rất thấp nhất có thể - các thành phần riêng lẻ của một software
application được tested trong giai đoạn development. Unit testing thường được thực
hiện bởi các developers thay vì testers. Trong unit testing, một chức năng của một
phần của code như class, được test để xác minh tính chính xác của nó bằng trình
điều khiển, Unit testing frameworks, mock objects, và stubs

  Nó là chìa khóa để viết code clean hơn, code có thể bảo trì dễ dang hơn.

  Nhưng, đôi khi có những câu hỏi về định nghĩa của các từ khoá khi nói đến unit
testing. Chẳng hạn, chính xác thì unit là gì? mock có nghĩa là gì Làm thế nào để
biết liệu có thực sự làm unit testing không?

## Unit là gì?

   Câu hỏi đầu tiên xuất hiện khi thảo luận về unit testing là, unit là gì? Bạn có 
thể thực hiện unit testing mà không cần biết unit  là gì.

   Khi nói đến unit testing, một unit là phần có thể test nhỏ nhất của bất kỳ 
software nào. Nó thường có một hoặc một vài inputs và thường là một single output.
Trong procedural programming, một unit có thể là một ndividual program, function, 
procedure,... Trong object-oriented programming, unit nhỏ nhất là một method,
mà có thể thuộc về một base/ super class, abstract class hoặc derived/ child class.
(Một số coi module của an application là một unit. Điều này không được khuyến khích
vì có thể sẽ có nhiều unit riêng lẻ trong module đó.)  Unit testing frameworks, 
drivers, stubs, and mock/ fake objects được sử dụng để hỗ trợ unit testing.

## Một Isolation Framework là gì?

   Thông thường, các developer đã sử dụng mocking framework để mô tả code, nó cung cấp 
faking services để cho phép các class được test một cách cô lập.

   Tuy nhiên, một mock là một loại cụ thể của fake class, cùng với các stub. Vì vậy,
có lẽ chính xác hơn khi sử dụng isolation framework thay vì mocking framework.

   Một isolation framework hữu ích sẽ cho phép tạo dễ dàng cả hai loại mock - stubs
   
   Fakes cho phép bạn kiểm tra một class một cách cô lập bằng cách cung cấp các 
triển khai của các dependencies mà không yêu cầu các dependencies thực sự.

   Định nghĩa: Một isolation framework là một collection of code cho phép tạo ra 
fake dễ dàng.

   Định nghĩa: Một fake class là bất kỳ class nào cung cấp đủ chức năng để giả vờ 
rằng đó là một dependency cần thiết cho một bài test. Có hai loại fake: stubs và mocks.

# Stubs

   Một Stub là một class mà nó thực hiện mức tối thiểu tuyệt đối, là một phụ thuộc thực 
tế cho test. Nó không cung cấp chức năng theo yêu cầu của test, ngoại trừ việc xuất 
hiện để thực hiện một interface nhất định từ một base class nhất định.

   Khi Class Under Test(CUT) gọi nó, một stub thường không làm gì cả. Stub hoàn toàn 
ngoại vi để kiểm tra Class Under Test(CUT) và tồn tại hoàn toàn để cho phép Class 
Under Test(CUT) chạy.

   Một ví dụ điển hình là một stub cung cấp logging services. Class Under Test(CUT)
có thể cần triển khai interface ILogger để thực thi, nhưng không có test case nào 
quan tâm đến việc logging.

   Bạn đặc biệt không muốn Class Under Test(CUT) logging bất cứ điều gì. Do đó, 
Stub giả vờ là logging service bằng cách thực hiện interface, nhưng việc thực hiện đó
thực sự không làm gì cả.

   Hơn nữa, trong khi một stub có thể trả về data để giữ cho Class Under Test(CUT) chạy,
nó không bao giờ có thể thực hiện bất kỳ hành động nào sẽ thất trong một test case.
Nếu có, thì nó không còn là stub, và nó trở thành một mock.

   Định nghĩa: Một Stub là một fake không có ảnh hưởng đến việc vượt qua hoặc thất bại 
của test case và nó tồn tại hoàn toàn để cho phép test case chạy.

## Mocks

   Mocks phức tạp hơn một chút. Mocks làm những gì stub làm, trong
đó chúng cung cấp một triển khai fake của một dependency cần thiết của Class Under Test(CUT).

   Tuy nhiên, chúng vượt xa chỉ là một sơ khai bằng cách ghi lại sự tương tác giữa chính nó và CẮT. Một bản giả giữ một bản ghi tất cả các tương tác với CUT và báo cáo lại, vượt qua bài kiểm tra nếu CUT hành xử đúng và không thực hiện bài kiểm tra nếu không.
Do đó, nó là giả, chứ không phải chính CUT, sẽ xác định xem thử nghiệm có vượt qua hay không.
Đây là một ví dụ.
Giả sử bạn có một lớp được gọi làWidgetProcessor. Nó có hai phụ thuộc, ILogger và IVerifier. Để kiểm tra WidgetProcessor, bạn cần giả mạo cả hai phụ thuộc đó.
Tuy nhiên, để kiểm tra WidgetProcessor, bạn sẽ muốn thực hiện hai thử nghiệm - một trong đó bạn khai thác ILogger và kiểm tra sự tương tác với IVerifier, và một thử nghiệm khác khi bạn khai thác IVerifier và kiểm tra sự tương tác với ILogger.
Cả hai đều yêu cầu hàng giả, nhưng trong mỗi trường hợp, bạn sẽ cung cấp một lớp sơ khai cho lớp này và lớp giả cho lớp kia.
Chúng ta hãy xem xét kỹ hơn một chút về kịch bản đầu tiên, trong đó chúng ta sẽ loại bỏ ILogger và sử dụng một bản giả cho IVerifier. Sơ khai chúng tôi đã thảo luận; Bạn có thể viết một triển khai ILogger trống hoặc bạn sử dụng khung cách ly để triển khai giao diện để không làm gì cả.
Tuy nhiên, giả mạo cho IVerifier trở nên thú vị hơn một chút - nó cần một lớp giả. Nói quá trình xác minh một widget mất hai bước. Đầu tiên, bộ xử lý cần xem liệu widget có trong hệ thống hay không, và sau đó, nếu có, bộ xử lý cần kiểm tra xem widget có được cấu hình đúng không.
Do đó, nếu bạn đang kiểm traWidgetProcessor, bạn cần chạy thử nghiệm để kiểm tra xem WidgetProcessor có thực hiện cuộc gọi thứ hai hay không nếu nó được trả lại đúng từ cuộc gọi đầu tiên.
Bài kiểm tra này sẽ yêu cầu lớp giả để làm hai việc. Đầu tiên, nó cần trả về True từ cuộc gọi đầu tiên và sau đó nó cần theo dõi xem cuộc gọi cấu hình kết quả có được thực hiện hay không.
Sau đó, nó trở thành công việc của lớp giả để cung cấp thông tin vượt qua / thất bại. Nếu cuộc gọi thứ hai được thực hiện sau cuộc gọi đầu tiên trả về True, thì thử nghiệm sẽ vượt qua; Nếu không, thử nghiệm thất bại.
Đây là những gì làm cho lớp giả này trở thành một giả: Bản thân giả có chứa thông tin cần được kiểm tra cho các tiêu chí đạt / không đạt.
Định nghĩa: Giả là một giả mạo theo dõi hành vi của lớp được kiểm tra và vượt qua hoặc thất bại bài kiểm tra dựa trên hành vi đó.
Hầu hết các khung cô lập bao gồm khả năng theo dõi rộng rãi và tinh vi chính xác những gì xảy ra bên trong một lớp giả.
Chẳng hạn, các giả lập không chỉ có thể biết liệu một phương thức đã cho có được gọi hay không, mà chúng còn có thể theo dõi số lần các phương thức đã cho được gọi và các tham số được truyền cho các cuộc gọi đó.
Họ có thể xác định và quyết định xem có thứ gì đó được gọi là isn mệnh hay không, hoặc nếu thứ gì đó được gọi là được cho là như vậy. Là một phần của thiết lập thử nghiệm, bạn có thể cho mock biết chính xác những gì sẽ xảy ra và sẽ thất bại nếu chuỗi sự kiện và tham số chính xác đó không được thực hiện theo kế hoạch.
Sơ khai tương đối đơn giản để xây dựng và sử dụng, nhưng giả có thể khá phức tạp.
Nhưng một lần nữa, toàn bộ vấn đề ở đây là kiểm tra các lớp của bạn một cách cô lập; Bạn muốn CUT của bạn có thể thực hiện nhiệm vụ của mình mà không cần phụ thuộc bên ngoài, thực tế, bên ngoài.
Do đó, một định nghĩa cuối cùng:
Định nghĩa: Kiểm thử đơn vị là kiểm tra một thực thể mã đơn lẻ khi tách biệt hoàn toàn với các phụ thuộc của nó.

## Tại sao phải kiểm tra đơn vị?

Tôi thấy rằng có rất nhiều sự kháng cự khi làm thử nghiệm đơn vị. Nhiều nhà phát triển dường như xem nó là một sự lãng phí thời gian hoặc nó sẽ chỉ trì hoãn việc hoàn thành một dự án theo thời hạn. Họ cảm thấy rằng họ có thể nhận được bất kỳ lợi ích nào từ nó.
Tôi không thể đồng ý nhiều hơn. Đây là lý do tại sao.

### Kiểm thử đơn vị sẽ tìm thấy lỗi

Cho dù bạn thực hiện phát triển dựa trên kiểm tra và viết bài kiểm tra của mình trước, hãy viết bài kiểm tra của bạn khi bạn thực hiện hoặc viết bài kiểm tra sau khi mã được viết, kiểm tra đơn vị sẽ tìm thấy lỗi.
Khi bạn viết một bộ đầy đủ các bài kiểm tra xác định hành vi dự kiến ​​là gì đối với một lớp nhất định, mọi thứ trong lớp đó không phải là hành vi như mong đợi sẽ được tiết lộ.

### Kiểm tra đơn vị sẽ tránh lỗi

Một bộ kiểm tra đơn vị đầy đủ và kỹ lưỡng sẽ giúp đảm bảo rằng bất kỳ lỗi nào xâm nhập vào mã của bạn sẽ được tiết lộ ngay lập tức.
Thực hiện một thay đổi giới thiệu một lỗi và các bài kiểm tra của bạn có thể tiết lộ nó vào lần tiếp theo bạn chạy thử nghiệm. Nếu bạn tìm thấy một lỗi nằm ngoài địa hạt của bộ kiểm thử đơn vị, bạn có thể viết một bài kiểm tra cho nó để đảm bảo rằng lỗi không bao giờ quay trở lại.

### Kiểm tra đơn vị tiết kiệm thời gian

Việc kiểm tra đơn vị có tiết kiệm thời gian hay không là khái niệm gây tranh cãi nhất về kiểm thử đơn vị. Hầu hết các nhà phát triển tin rằng viết bài kiểm tra đơn vị mất nhiều thời gian hơn nó tiết kiệm. Tôi không nghĩ rằng điều này - thực tế, tôi lập luận ngược lại.
Viết bài kiểm tra đơn vị giúp đảm bảo rằng mã của bạn hoạt động như thiết kế, ngay từ đầu. Các bài kiểm tra đơn vị xác định những gì mã của bạn nên làm, và do đó bạn đã giành được thời gian viết mã làm những việc mà nó không nên làm.
Mỗi bài kiểm tra đơn vị trở thành một bài kiểm tra hồi quy, đảm bảo rằng mọi thứ tiếp tục hoạt động như được thiết kế trong khi bạn phát triển. Họ giúp đảm bảo rằng những thay đổi tiếp theo don don phá vỡ mọi thứ.
Họ giúp đảm bảo rằng những gì bạn viết lần đầu tiên là điều đúng đắn. Tất cả những lợi ích này tiết kiệm thời gian cả trong ngắn hạn và dài hạn.
Và nếu bạn nghĩ về nó, bạn đã kiểm tra mã của mình trong khi bạn đang viết nó. Có thể bạn viết một ứng dụng giao diện điều khiển đơn giản. Ít nhất, bạn biên dịch và thấy nó chạy.
Không ai kiểm tra mã mà họ không tin là có tác dụng, và bạn phải làm gì đó để khiến bản thân nghĩ rằng nó hoạt động. Dành thời gian đó để viết các bài kiểm tra đơn vị, và bạn sẽ tách rời mã hóa làm việc với một bộ các bài kiểm tra hồi quy.

### Kiểm tra đơn vị mang lại sự yên tâm

Có một bộ kiểm tra đầy đủ, đầy đủ và kỹ lưỡng bao gồm các chức năng hoàn chỉnh của mã của bạn có thể khó đạt được, nhưng có nó sẽ giúp bạn yên tâm.
Bạn có thể chạy tất cả các thử nghiệm đó và biết rằng mã của bạn hoạt động như mong muốn. Bạn có thể cấu trúc lại và thay đổi mã, biết rằng nếu bạn phá vỡ bất cứ điều gì, bạn sẽ biết ngay lập tức.
Biết trạng thái của mã của bạn, rằng nó hoạt động và bạn có thể cập nhật và cải thiện nó mà không sợ là một điều tuyệt vời.
Tất cả các độ tuổi mã, nhưng bạn có thể giữ mã của mình thực sự trở thành mã kế thừa. Có một số cách để xác định mã kế thừa, nhưng có một cách là: Mã Code bạn sợ chạm vào. Nếu mã của bạn có các bài kiểm tra đơn vị, thật khó để nó trở thành mã kế thừa.
Nhiều người trong chúng ta có đoạn mã mà chúng ta sợ chạm vào, nhưng với thử nghiệm đơn vị, bạn sẽ không có loại mã đó. Kiểm thử đơn vị loại bỏ nỗi sợ chạm vào mã.
Trong cuốn sách Làm việc hiệu quả với Mã kế thừa, Michael Feathers đi xa đến mức định nghĩa mã kế thừa là bất kỳ mã nào không có các bài kiểm tra đơn vị. Bạn muốn tránh mã của bạn trở thành mã kế thừa? Viết bài kiểm tra đơn vị cho nó

### Tài liệu kiểm tra đơn vị sử dụng đúng cách của một lớp

Một trong những lợi ích của kiểm thử đơn vị là các kiểm tra của bạn có thể xác định cho các nhà phát triển tiếp theo cách sử dụng lớp.
Các bài kiểm tra đơn vị trở thành các ví dụ đơn giản về cách mã của bạn hoạt động, những gì nó được dự kiến ​​sẽ làm và cách sử dụng mã phù hợp. Người tiêu dùng mã của bạn có thể tìm đến các bài kiểm tra đơn vị của bạn để biết thông tin về cách thức phù hợp để làm cho mã của bạn thực hiện những gì nó phải làm.

## Cách thực hiện Unit testing

  Unit testing thường được tự động, nhưng đôi khi có thể được thực hiện thủ công
với sự trợ giúp của tài liệu hướng dẫn trên tất cả các loại mobile và web
applications.

  Trong unit testing tự động, developer viết code trong app để kiểm tra chức năng
hoặc quy trình. Khi ứng dụng được triển khai, code đó có thể được gỡ bỏ. Chức
năng có thể được cách ly để kiểm tra app một cách chặt chẽ và nó cho thấy sự phụ
thuộc giữa code được test và các unit khác. Sau đó, các dependencies có thể được
loại bỏ. Hầu hết các developer sử dụng unit test automated framework để ghi lại
các trường hợp test thất bại

<h1 aglin="center">
<img src="/Test/img/unit-test-life-cycle.png"/>
</h1>

## Làm thế nào để cải thiện unit testing

  Có một số điểm cần lưu ý trong khi thực hiện unit testing. Sử dụng các quy ước
đặt tên nhất quán và test một code tại một thời điểm. Đảm bảo rằng có một trường
hợp unit testing tương ứng cho một module nếu có bất kỳ thay đổi nào trong code.
Tất cả các lỗi phải được sửa trước khi chuyển sang giai đoạn tiếp theo. Tập trung
nhiều hơn vào các bài test ảnh hưởng đến hành vi, hoạt động của hệ thống.

## Ưu điểm và nhược điểm của unit testing

Với unit testing, tốc độ phát triển sẽ nhanh hơn. Nếu bạn thực hiện kiểm tra nhà phát triển thay vì kiểm tra đơn vị, thì bạn cần đặt điểm dừng, kích hoạt GUI và cung cấp đầu vào. Nhưng nếu bạn làm bài kiểm tra đơn vị, bạn viết mã, viết bài kiểm tra, rồi chạy kiểm tra. Bạn không cần phải cung cấp các đầu vào hoặc kích hoạt GUI. Cuối cùng, bạn có một mã đáng tin cậy hơn. Mất ít thời gian hơn để tìm và sửa các lỗi trong quá trình kiểm tra đơn vị so với kiểm tra hệ thống hoặc Chấp nhận.

Mã tái sử dụng cũng làm giảm nỗ lực và tiết kiệm thời gian vì mã cần phải được mô đun hóa trong thử nghiệm đơn vị. Bạn kết thúc với mã đáng tin cậy hơn vì các lỗi đã được sửa trong giai đoạn ban đầu.

Các thử nghiệm đơn vị dễ dàng gỡ lỗi hơn vì chỉ những thay đổi mới nhất cần được gỡ lỗi nếu thử nghiệm thất bại, trong khi nếu bạn thử nghiệm ở cấp độ cao hơn, thì bạn sẽ phải quét các thay đổi được thực hiện trong vòng vài tuần hoặc vài tháng. Ngoài ra, chúng tôi có thể kiểm tra một phần của dự án mà không cần chờ đợi phần khác hoàn thành do tính chất mô-đun của thử nghiệm đơn vị.

Nhược điểm duy nhất với kiểm thử đơn vị là nó không thể kiểm tra tất cả các đường dẫn thực thi và nó không thể bắt lỗi hệ thống rộng hoặc lỗi tích hợp.

## kết luận

  Được rồi, nếu tất cả những điều đó không thuyết phục bạn làm unit testing, tôi
cũng không biết làm gì.

  Tôi hy vọng rằng sau khi đọc song, bạn sẽ có một ý nghĩ tốt hơn về việc unit
testing là gì và tại sao nên làm điều đó. Tôi nghĩ rằng bạn khó có thể tìm thấy
bất cứ ai đã hối tiếc về thời gian viết unit testing. Vì vậy, nếu bạn không có
unit testing, hãy cân nhắc bắt đầu ngay.
