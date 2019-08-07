# Object Literal VS Contructors In Javascript

  Thật đáng ngạc nhiên JavaScript là một object-oriented programming language
có thể gây nhầm lẫn khi nói đến việc tạo object. Trong bài viết này tôi muốn sắp
xếp nó cho tốt.

## Literals And Constructors

  Trong JavaScript, có hai cách để tạo một object - literal notation
và constructor function. Các object được tạo bằng cách sử dụng object literals
là singletons. Điều này có nghĩa là khi một thay đổi được thực hiện cho object,
nó sẽ ảnh hưởng đến object đó trên toàn bộ script. Object được xác định với
function constructor cho phép chúng ta có nhiều phiên bản của đối tượng đó.
Điều này có nghĩa là thay đổi được thực hiện cho một instance, sẽ không ảnh hưởng
đến các instances khác.

### Object Literal Notation

  Object literal được khai báo như bên dưới:

  ```javascript
  // literal notation
  let dog = {
    name: 'Max',
    colors: ['Black'],
    age: 5
  };
  ```

  Nếu bạn không có behaviour liên quan đến một đối object (tức là nếu object chỉ
là nơi chứa data / state), chúng ta sẽ sử dụng một object literal. Áp dụng
[KISS principle](https://en.wikipedia.org/wiki/KISS_principle). Nếu bạn không
cần bất cứ thứ gì ngoài một thùng chứa data đơn giản, hãy sử dụng một object literal

### Object Constructor Notation

  Object constructor được khai báo như bên dưới:

```javascript
// constructor function
function Dog(name, color, age) {
  this.name = name;
  this.color = color;
  this.age = age;
  this.verify = function () {
    return this.age > 5;
  }
};

// or:
Dog.prototype.verify = function () {
    return this.age > 5;
};

// init with new keyword
var dog = new Dog('Max', ['Black'], 5);
```

  Nếu bạn muốn thêm behaviour vào object của mình, bạn có thể đi với một
constructor và thêm các methods cho object trong quá trình xây dựng hoặc cung cấp
cho class của bạn một prototype.

  Một class như thế này cũng hoạt động như một schema cho data object của bạn:
Bây giờ bạn có một số loại contract (thông qua constructor) những properties mà
object initializes/contains. Một literal chỉ là một đốm dữ liệu vô định hình.

  Bạn cũng có thể có một function `verify` bên ngoài hoạt động trên data object liệu
đơn giản:

```javascript
// literal notation
let dog = {
  name: 'Max',
  colors: ['Black'],
  age: 5
};

function verify(dog) {
    return dog.age > 5;
}
```

  Tuy nhiên, điều này không thuận lợi đối với việc encapsulation: Lý tưởng nhất
là tất cả data + behaviour  liên quan đến một entity nên sống cùng nhau.
