# POJO class - Bean class

  Trong Java language classes và objects là khái niệm căn bản của Object Oriented
Programming nó xoay quan các entities trong cuộc sống thực, mà những thứ chúng
ta đã quá quen thuộc với chúng và các khái nhiệm của chúng. Trong bài viết này
chúng ta sẽ tìm hiểu nâng cao hơn một chút về sự khác nhau giữa POJO và JavaBean
và cách biến POJO thành JavaBean có thể hữu ích với chúng ta.

## Plain Old Java Objects

  Một **Plain Old Java Objects (POJO)** là một java object thông thường là khái
niệm được đưa ra bởi Martin Fowler, Rebecca Parsons và Josh MacKenzie vào tháng
9 năm 2000. Một POJO class không bị ràng buộc bởi bất kỳ hạn chế nào ngoài những
hạn chế bắt buộc của ngôn ngữ Java và không yêu cầu bất kỳ class path.

  Thuật ngữ **POJO** ban đầu biểu thị một object Java không tuân theo bất kỳ
Java object models, conventions, or frameworks nào của Java. Ngày nay 'POJO' cũng
có thể được sử dụng làm từ viết tắt cho đối tượng JavaScript đơn giản.

  POJO được sử dụng để tăng khả năng đọc và khả năng sử dụng lại của chương trình.
POJO đã được chấp nhận nhiều nhất vì chúng dễ viết và dễ hiểu. Chúng được giới
thiệu trong EJB 3.0 bởi Sun microsystems.

> POJO không nên
> 1. Extend các classes được chỉ định trước, Ví dụ: public class GFG extends
javax.servlet.http.HttpServlet { … } không phải là một lớp POJO.
> 2. Implement các interfaces chỉ định trước, Ví dụ: public class Bar implements
javax.ejb.EntityBean { … } không phải là một lớp POJO.
> 3. Chứa các annotations được chỉ định trước, Ví dụ: @javax.persistence.Entity
public class Baz { … } không phải là một lớp POJO.

  POJO về cơ bản định nghĩa một entity. Giống như trong một chương trình, nếu
bạn muốn có một class Employee thì bạn có thể tạo POJO như sau:

```
// Employee POJO class to represent entity Employee
public class Employee
{
  public String firstName;
  public String lastName;
  private LocalDate startDate;

  public EmployeePojo(String firstName, String lastName, LocalDate startDate) {
      this.firstName = firstName;
      this.lastName = lastName;
      this.startDate = startDate;
  }

  public String name() {
      return this.firstName + " " + this.lastName;
  }

  public LocalDate getStart() {
      return this.startDate;
  }
}
```

  Ví dụ trên là một ví dụ được xác định rõ về POJO. Như bạn có thể thấy, class
này có thể được sử dụng bởi bất kỳ chương trình Java nào vì nó không bị ràng buộc
với bất kỳ framework nào.

  Nhưng, chúng tôi không tuân theo bất kỳ quy ước nào để xây dựng, truy cập hoặc
sửa đổi trạng thái của class. Điều này gây ra vấn đề. Nó có thể giới hạn khả năng
của một framework để ưu tiên quy ước về cấu hình, hiểu cách sử dụng lớp và tăng
cường chức năng của nó. Để hiểu hơn, chúng ta hãy thao tác với Employee bằng cách
sử dụng reflection. Vì vậy, chúng ta sẽ tìm thấy một số hạn chế của nó.

### Reflection với POJO

  Hãy commons-beanutils vào:
```
<dependency>
    <groupId>commons-beanutils</groupId>
    <artifactId>commons-beanutils</artifactId>
    <version>1.9.4</version>
</dependency>
```

Và bây giờ, hãy kiểm tra các thuộc tính của POJO class:

```
List<String> propertyNames =
        (List<String>) Arrays.stream(PropertyUtils.getPropertyDescriptors(Employee.class))
                .map(PropertyDescriptor::getDisplayName)
                .collect(Collectors.toList());
```

Nếu chúng ta print ra propertyNames, chúng ta sẽ chỉ thấy:

```
[start]
```

Ở đây, chúng ta thấy rằng chúng ta chỉ bắt đầu như một property của class.
PropertyUtils không tìm thấy các properties khác.

làm sao chúng ta thấy được những proerties khác, chúng ta sẽ thấy tất cả các properties
của mình: FirstName, lastName và startDate. nhờ nhiều thư viện Java hỗ trợ theo
mặc định một thứ gọi là quy ước đặt tên JavaBean.

## JavaBean

  JavaBean là một POJO có khả năng **serializable**, có no-argument constructor
và cho phép truy cập vào các properties bằng các phương thức getter và setter
tuân theo quy ước đặt tên đơn giản. Do quy ước này, các tham chiếu khai báo đơn
giản có thể được thực hiện cho các properties của JavaBeans tùy ý. Code sử ​​dụng
tham chiếu khai báo như vậy không cần phải biết gì về loại Bean và Bean có thể
được sử dụng với nhiều framework mà không cần các framework này phải biết chính
xác loại bean. JavaBeans chỉ rõ, nếu được triển khai đầy đủ, sẽ phá vỡ một chút
mô hình POJO vì class phải implement Serializable interface để trở thành một
JavaBean thực sự. Nhiều lớp POJO vẫn được gọi là JavaBeans không đáp ứng yêu
cầu này. Vì serializable là một marker interface (không có phương thức), nên đây
không phải là một vấn đề lớn.

  bây giờ, hãy thử chuyển đổi Employee POJO thành JavaBean:

```
public class Employee implements Serializable {

    private static final long serialVersionUID = -3760445487636086034L;
    private String firstName;
    private String lastName;
    private LocalDate startDate;

    public Employee() {
    }

    public Employee(String firstName, String lastName, LocalDate startDate) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.startDate = startDate;
    }

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    //  additional getters/setters

}
```

  Khi chúng ta kiểm tra bean của chúng ta với reflection, bây giờ chúng ta nhận
được danh sách đầy đủ các properties:

```
[firstName, lastName, startDate]
```

## Kết luận

  Trong hướng dẫn này, chúng ta đã so sánh POJO với JavaBeans.

  Chúng ta đã chỉ ra một cách mà JavaBeans tỏ ra hữu ích. Hãy nhớ rằng mọi sự
lựa chọn đều đi kèm với sự đánh đổi.

  Khi chúng ta sử dụng JavaBeans, chúng ta cũng nên chú ý đến một số nhược điểm
tiềm ẩn:

  1. Khả năng tương tác - JavaBeans của chúng ta có thể thay đổi do các phương
thức setter của chúng
  2. Dư thừa - chúng ta phải có getters cho tất cả các properties và setters cho
hầu hết, phần lớn điều này có thể không cần thiết

  3. Trình xây dựng đối số không - chúng ta thường cần các argument trong các
constructor của mình để đảm bảo đối tượng được khởi tạo ở trạng thái hợp lệ,
nhưng tiêu chuẩn JavaBean yêu cầu chúng ta cung cấp constructor không có argument.


  Hãy chọn lựa cái nào phù hợp trong từng tình huống với bạn.
