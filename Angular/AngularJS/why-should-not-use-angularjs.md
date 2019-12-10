# Tại sao không nên sử dụng AngularJS

<h2 align="center" style = "margin: 80px 0">
  <img src="/Angular/AngularJS/assets/images/AngularJS_logo.svg.png">
</h2>

  Đã có nhiều thời gian kể từ khi AngularJS ra đời. Bây giờ trên internet, có một
số lượng lớn bài viết ca ngợi AngularJS, và những bài phê bình không quá nhiều.
Nhưng những bài viết như vậy đang dần dần bắt đầu xuất hiện. AngularJS tạo ra một
hiệu ứng tuyệt vời, hiệu quả tốt, khi chúng ta nhìn thấy nó lần đầu tiên, tôi đã
viết ng-repeat và thực hiện logic này chỉ bằng các thẻ HTML, và nó tự cập nhật!
thực hiện như một ứng dụng thực sự. Vậy có gì sai với AngularJS? Không có câu trả
lời chắc chắn. Vậy chúng ta hãy tìm hiểu những vấn đề của AngularJS trong bài viết
này.

## 1. Two-way data-binding.

  Có một quy tắc cơ bản trong lập trình, minh bạch luôn luôn tốt hơn không
minh bạch Tất cả những $watch này là một sự mời gọi không minh bạch của những
người xử lý sự kiện trong AngularJS. Trong thực tế, các sự kiện xảy ra khi người
dùng thao tác sự kiện gì đó chứ không phải khi model thay đổi, vì vậy khi chúng
ta code trên AngularJS, bạn cần nghĩ theo cách sau: Người dùng đã click vào buttom
và model đã thay đổi, bây giờ chúng ta cần lắng nghe thay đổi trên model này và
gọi trình xử lý, và không phải là người dùng đã click vào buttom và gọi trình xử
lý đó.

  Ngoài ra two-way data binding có nghĩa là thay đổi bất cứ điều gì trong ứng dụng
của bạn sẽ kích hoạt hàng trăm watcher theo dõi các thay đổi. Nó chậm, và đặc
biệt là tất cả đều tệ trên nền tảng di động và khi bạn thực hiện một cái gì đó
phức tạp. Và nó là một phần cơ bản của AngularJS. AngularJS thậm chí còn áp đặt
các hạn chế về cách bạn có thể thao tác với UI. AngularJS giới thiệu one-way data
binding, để tránh các vấn đề về hiệu suất. Tạo một vấn đề và giải quyết nó bằng
một kiến trúc không rõ ràng. Hãy xem có bao nhiêu người đang vật lộn với hiệu
suât với AngularJS.Ngay cả khi bạn tìm kiếm trên google cả thiện hiệu suất
AngularJS, bạn sẽ ngạc nhiên với số lượng bài viết mô tả cách tăng tốc AngularJS.
Có phải đó là một lý lẽ đủ để ủng hộ rằng AngularJS là chậm?

  Có một điểm khác, đôi khi bạn cần two-way data binding, nhưng UI chậm. Ví dụ:
khi người dùng tải trang, người dùng có thể thấy `{{expressions}}`. Nó không
phải là một lỗi mà nó là một tính năng! Chúng ta cần phải đưa ra một directive,
như vậy sẽ ẩn UI nên người dùng sẽ không thấy. Và với những mục đích này,
AngularJS giới thiệu các directive mới: `ngCloack`, `ngBind`. AngularJS một lần
nữa tạo ra các vấn đề và giải quyết chúng bằng các bản kiến trúc sai lầm.

## 2. Dependency Injection

Ví dụ tiếp theo về Dependency Injection. Trong Angular dependencies được injected
bởi tên của một đối số như bên dưới.

```
function MyController($scope, $window) { // … }
```

Ở đây, AngularJS gọi `.toString()`, lấy tên của các đối số và sau đó tìm kiếm của
chúng trong danh sách tất cả các dependencies hiện có. Tìm kiếm một tên biến
là ok, nhưng vấn đề là nó dừng hoạt động sau khi thu nhỏ code. Khi bạn thu nhỏ
code của mình, nó sẽ ngừng hoạt động, vì các biến được chèn theo tên. Bạn phải
sử dụng cú pháp này:

```
someModule.controller(‘MyController’, [‘$scope’, function($scope) { }]);
```

Hoặc

```
MyController.$inject = [‘$scope’, ‘$window’];
```

nhưng nó không rõ lý do tại sao cần phải giới thiệu một số cách để làm điều tương
tự, nhưng thậm chí còn không rõ tại sao cần phải giới thiệu một cách cố tình bị
phá vỡ. Điều quan trọng tiếp theo là làm thế nào dependency được tuyên bố. Trước khi bạn inject một dependency, nó cần phải được khai báo. Bạn sẽ làm điều này như thế nào:

```
injector.register(name, factoryFn);
```

Trong đó tên là tên của dependency và `FactoryFn` là hàm sẽ khởi tạo dependency.
Bây giờ hãy nhìn vào
[những gì AngularJS cung cấp](https://docs.angularjs.org/guide/providers).
AngularJS giới thiệu 5 entities mới: provider, service, factory, value, constant.
Và mỗi thứ trong số chúng là khác nhau, nhưng về cơ bản chúng đều giống nhau.
Quan trọng nhất, tất cả chúng có thể dễ dàng được thay thế bằng một
**single method** mà tôi đã mô tả ở trên. Nó giống như một architecture không
được tốt.

## 3. Debugging

  Bản thân việc debug rất phức tạp, những đó là chưa đủ, các errors trong binding
không ổn cho tất cả trường hợp ví dụ như trong code bên dưới:

```
<div ng-repeat=”phone in user.phones”> {{ phone }} </div>
```

  Nếu `user` không được định nghĩa, sẽ không có lỗi nào xảy ra. Hơn nữa, bạn
không thể thể đặt một breakpoint bên trong {{ expression }}, vì nó không phải là
JavaScript. Hơn nữa, các errors xảy ra trong JavaScript được bắt bởi angular
interceptor và được browser hiểu là lỗi (mọi thứ xảy ra trong AngularJS,
đều nằm trong AngularJS). Do đó, bạn phải chọn `flag Pause On Caught Exceptions`
(để trình debugger dừng lại trên tất cả các exceptions, Nhưng không chỉ trên các
trường hợp chưa được phát hiện (thường là các exceptions không được chú ý)), đó
là lý do tại sao bạn cần phải vượt qua tất cả các  internal angular exceptions
trong khi nó đang đang khởi tạo và chỉ sau đó bạn mới có được exception thực sự
của mình. Và sau đó bạn sẽ thấy **stacktrace** sau:

<h3 align="center">
  <img src="/Angular/AngularJS/assets/images/1_VmMW21ih1M9laiXt9COleg.png">
</h3>

  error được đưa ra từ chu trình phân loại và điều đó có nghĩa là nó có thể được
gây ra bởi sự thay đổi của bất kỳ biến nào trong toàn bộ ứng dụng. Bạn sẽ không
bao giờ tìm ra error xuất phát từ việc sử dụng trình debugger.

## 4. Scope inheritance

  Đây chắc chắn là lỗi phổ biến nhất mà hoàn toàn mọi developer AngularJS phải
đối mặt. Ví dụ:

```
<div ng-init="phone = 02"> {{ phone }} </div>
<div ng-if="true">
    <button ng-click=”phone = 911">change phone</button>
</div>
```

  Sau click vào buttom, variable **phone** không thể thay đổi được trong khi:

```
<div ng-init="phone = {value: 02}"> {{ phone.value }} </div>
<div ng-if="true">
    <button ng-click=”phone.value = 911">change phone</button>
</div>
```

  Mọi thứ hoạt động như mong đợi Hoặc ví dụ trong code này hành vi của AngularJS
cực kỳ không trực quan. Thậm chí còn có một syntax mới để khai báo các variable
trong controller bằng cách sử dụng ký hiệu để khắc phục vấn đề này. Rõ ràng, hành
vi này hoàn toàn vô dụng và rất nguy hiểm.

  * Nếu bạn căn cứ logic của mình vào scope inheritance, thì việc kiểm tra nó sẽ
    trở nên rất khó khăn (bạn phải khởi tạo state của parent controller).

  * Logic trở nên phức tạp và tiềm ẩn hơn (bạn sử dụng một variable không được khai
    báo trong module hiện tại).

  * Các variable được inherited tương tự như các global variable (từ phạm vi con
    bạn có thể truy cập hoàn toàn bất kỳ variable nào của bất kỳ parent scope
    nào). Và mọi người đều biết rằng global variable là không tốt.

  * Nó làm tăng số lượng lý thuyết bạn cần phải học biết: prototypal inheritance,
    khi **transclude** tạo ra một scope mới và khi nó không tạo ra một scope mới,
    bạn cần biết cách  scope inheritance hoạt động với `ng-repeat`, `ng-if`,
    `ng-switch`, bạn cần biết tại sao một directive có 3 scope type khác nhau và như vậy.

## 5. Directives

  Chúng ta đến phần thú vị nhất, directive là phần quan trọng của angularJS. Mọi
người đều thích chúng, mọi người chỉ thích chúng vì một lý do nào đó! Nhưng tôi
không như vậy. Để hiểu directive và tại sao chúng ta cần nó, bạn thực sự phải
dành rất nhiều thời gian. Điều đáng buồn là sự phức tạp này là vô dụng. Không có
lý do logic để phân tách logic cho 3 method (compile, link, controller), tất
cả điều này có thể được thực hiện dễ dàng trong một method duy nhất. Một lần nữa,
hãy xem có bao nhiêu vấn đề nó gây ra. Cũng tính đến việc một số functions có
scope, một số không, một số chỉ thực hiện một lần, một số lần với chu kỳ của
$digest và mọi directive cũng có mức độ ưu tiên. Ngay cả để tích hợp một số code
trong angularJS, ví dụ như một số plugin jQuery, bạn cần phải bao bọc nó trong
một directive. Bởi vì nó sẽ quá dễ dàng nếu các developers có thể sử dụng các giải
pháp ngay lập tức! Ngoài ra, nó không rõ tại sao controller function trong
directive có cùng tên với controller thông thường, chúng được sử dụng ở những nơi
hoàn toàn khác nhau và cho các mục đích khác nhau, chúng phải có các tên khác
nhau!

## Angular 2.0

  Phiên bản của angular 2 đã xuất hiện, phá vỡ khả năng tương thích ngược. Rõ
ràng, ngay cả các developers của Anglar cũng nhận ra rằng có điều gì đó sai và
quyết định viết lại Angular gần như từ đầu. Họ không có các chức năng mới, họ phá
vỡ khả năng tương thích ngược, họ viết lại hầu hết mọi thứ. Điều đó có nghĩa nếu
bạn bắt đầu một dự án mới với phiên bản hiện tại của AngularJS, trong tương lai
có thể bạn xẽ gặp phải khó khăn trong quá trình phát triển ứng dụng của bạn

## Conclusion

  Hầu hết các vấn đề mà tôi đã mô tả có thể được giải quyết nếu muốn. Tôi chỉ
đề cập đến những vấn đề đáng chú ý nhất, vì vậy nếu bạn sẽ sử AngularJS, hãy sẵn
sàng đối mặt với không chỉ những vấn đề này, mà còn nhiều vấn đề khác, ít chú ý
hơn, nhưng vẫn Các vấn đề.
  * Tài liệu khủng khiếp, bạn phải google rất nhiều thông tin.

  * Vẫn không có đầy đủ các directives cho các sự kiện bạn cần Nó không phải là
    một vấn đề lớn, nhưng nó cho thấy chất lượng của sản phẩm và sự chú ý đến các
    chi tiết.

  * Khi bạn viết code bằng AngularJS, bạn đưa logic của mình vào HTML
    (`ng-repeat`, `ng-show`, `ng-class`, `ng-model`, `ng-init`, `ng-click`,
    `ng-switch`, `ng-if`). Sự tồn tại của logic như vậy không tệ như thực tế là
    không thể test logic này bằng các bài unit-test.

  Điều tốt duy nhất mà AngularJS có là nó buộc các developers chia logic của họ
thành các modules và code trở nên chi tiết hơn. Thay vì AngularJS. Tôi không biết
tại sao AngularJS rất phổ biến, nhưng nó chắc chắn không phải vì nó là một
framework tốt.
