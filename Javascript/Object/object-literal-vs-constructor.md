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

## Defining Methods And Properties

  Các objects trong JavaScript có các methods và properties, cho dù chúng được
xây dựng bằng constructor function hoặc với literal notation. Hãy xem cách định
nghĩa chúng:

```javascript
// constructor function
function Website() {
    this.url = 'http://www.google.com';
    this.printUrl = function() {
        console.log(this.url);
    };
};

// literal notation
var Website = {
    'url': 'http://www.google.com',
    'printUrl': function() {
        console.log(this.url);
    }
};
```

## Using The Objects

  Bên cạnh syntax, hai objects khác nhau về cách bạn sử dụng chúng. Nếu object đã
được tạo bằng constructor, bạn phải khởi tạo nó trước. Mặt khác, một
literal-notated đã sẵn sàng để sử dụng:

```js
// constructor function
var InternalPointers = new Website();
InternalPointers.printUrl();

// literal notation
Website.printUrl();
```

## Where's the constructor of literal notation?

  Rõ ràng là với literal notation, bạn không thể có constructor, cụ thể là bạn
không thể khởi tạo object của mình trừ khi bạn thêm custom `init()` function.
Thay vào đó, với constructor, bạn có thể truyền các parameters bổ sung cho object
thông qua built-in constructor, như thế:

```js
// constructor function
function Website(protocol) {
    this.protocol = protocol;
    this.url = this.protocol + '://www.google.com';
    this.printUrl = function() {
        console.log(this.url);
    };
};

var InternalPointersHttp  = new Website('http');
var InternalPointersHttps = new Website('https');
InternalPointersHttp.printUrl();   // http://www.google.com
InternalPointersHttps.printUrl();  // https://www.google.com
```

## Instantiation Versus Singleton Approach

Đoạn code trước cho thấy một property quan trọng khác: với constructor, bạn có
thể khởi tạo bao nhiêu objects bạn muốn và tất cả chúng đều là unique. Với ký
literal notation, bạn luôn luôn xử lý object ban đầu (singleton), Ngay cả khi
bạn xác định một variable mới với object như value:

```js
// literal notation
var original = {
    'property' : 'original'
}

console.log(original.property); // 'original'

var clone = original;
clone.property = 'clone';

console.log(original.property); // 'clone' >:(
```

## What About Private And Public Stuff

 Có các private members với literal notation là một điều khá khó khăn, vì vậy hay
để nó lại. Chỉ cần nghĩ về các objects literal-notated là các lightweight
containers, nơi tất cả các thành viên đều public. Đối với các constructor
functions, mọi thứ dễ dàng hơn, nhờ vào khái niệm [closure](http://javascriptissexy.com/understand-javascript-closures-with-ease/).
Đây là cách bạn thiết lập constructor với một vài private members:

```js
// constructor function
function Website() {

    // private members
    var privateUrl = 'http://www.internalpointers.com';
    var privatePrint = function() {
        console.log(privateUrl);
    };

    // public members
    this.printUrl = function() {
        privatePrint();
    };
};

var InternalPointers = new Website();
InternalPointers.printUrl();               // 'http://www.google.com'
InternalPointers.privatePrint();           // TypeError: InternalPointers.privatePrint is not a function
console.log(InternalPointers.privateUrl);  // undefined
```
