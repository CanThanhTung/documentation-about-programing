# Integration testing

## 1. Integration testing là gì?

  **Integration test** là một cấp độ trong software test trong đó các units
riêng lẻ được kết hợp lại thành một nhóm và thực hiện test. Thông thường trong
một dự án thường có nhiều units và được thực hiện bởi nhiều người khác nhau. Mục
đích của **integration test** là để lộ ra các lỗi, khuyết điểm và thiếu sót
khi kết hợp lại với nhau trong sự tương tác giữa các units.  Test drivers và
test stubs được sử dụng để hỗ trợ integration testing.


 ## 2. Tại sao lại cần integration test?

 Mặc dù mỗi unit của một ứng dụng đã được unit test, nhưng các lỗi vẫn tồn tại
vì nhiều lý do khác nhau do như:

  * Một unit, nói chung, được thực hiện, thiết kế bởi một developer có sự hiểu
    biết và logic có thể khác nhac so với với các developer khác. Integration
    test trở nên cần thiết để xác minh các units hoạt động cùng nhau chính xác.
  * Tại thời điểm phát triển các units, có nhiều thay đổi yêu cầu của khách hàng.
    Các yêu cầu mới này có thể không được unit test và do đó integration test trở
    nên cần thiết.

## 3. Phương pháp

  Các sofware engineers định nghĩa nhiều phương pháp thực hiện integration test.

  1. Big Bang
  2. Incremental
     * Top down
     * Bottom up
     * Sandwich

  Dưới đây là các phương pháp khác nhau, cách chúng được thực hiện và những
hạn chế cũng như những lợi thế của chúng.

### Big Bang

  Phương pháp này ở đây tất cả các units được kết hợp lại một lần. Nó xác minh
nếu hệ thống hoạt động như mong đợi. Nếu bất kỳ vấn đề nào được phát hiện, thì
việc tìm ra unit nào đã gây ra sự cố trở nên khó khăn.

  Phương pháp này là một quá trình tốn thời gian để tìm ra một unit có khiếm
khuyết vì nó sẽ mất thời gian và một khi lỗi được phát hiện, việc sửa lỗi tương
tự sẽ tốn kém vì lỗi được phát hiện ở giai đoạn sau.

  * Ưu điểm:
    - Thuận thiện với các hiện thống nhỏ.

  * Nhược điểm:
    - Rất khó để phát hiện unit gây ra sự cố.
    - Với một số lượng lớn các units có thế có một số units có thể bị bỏ qua.
    - Integration test chỉ có thể bắt đầu khi các units được hoàn thành viêt
    test sẽ có ít thời gian hơn trong giai đoạn test.
    - Vì tất các units được thực hiện test cùng một lúc nên không thể tập chung
    được vào những units quan trọng

### Incremental

  Trong phương pháp này, việc test được thực hiện bằng cách nối hai hoặc nhiều
units có liên quan đến logic với nhau. Sau đó, các units liên quan khác được
thêm vào và test. Quá trình tiếp tục cho đến khi tất cả các units được tham gia
và test thành công.

  Incremental lần dược được thực hiện từ hai phương pháp khác nhau:

  1. Bottom up
  2. Top down

### Bottom-up Integration

  Trong **Bottom-up**, Như tên cho thấy bắt đầu từ unit thấp nhất hoặc unit
trong cùng của ứng dụng, và dần dần di chuyển lên. Integration testing bắt đầu
từ unit thấp nhất và dần dần tiến tới các unit trên của ứng dụng. Tiếp tục cho
đến khi tất cả các unit được tích hợp và toàn bộ ứng dụng được test dưới
dạng một unit. Nó cần sự giúp đỡ của **DRIVERS** để test.

  * Ưu điểm:
    - nếu một lỗi lớn tồn tại ở unit thấp nhất của chương trình, thì việc phát
      hiện nó sẽ dễ dàng hơn và có thể áp dụng các biện pháp khắc phục.

  * Nhược điểm
    - chương trình chính thực sự không tồn tại cho đến khi unit cuối cùng được
      tích hợp và test. Do đó, các lỗi thiết kế cấp cao hơn sẽ chỉ được phát
      hiện ở cuối.

### Top-down Integration

  Trong **Top-down**, Kỹ thuật này bắt đầu từ unit trên cùng và dần dần tiến tới
các unit thấp hơn. Chỉ có unit hàng đầu là unit được test cô lập. Sau này, các
unit thấp hơn được tích hợp từng cái một. Quá trình được lặp lại cho đến khi tất
cả các unit được tích hợp và test. Cần sự giúp đỡ của stub để test.

### Hybrid/Sandwich Integration

  Trong **hybrid/sandwich** là sự kết hợp của **Top-down** và **Bottom-up**. Ở
đây, các top units được test với các lower units đồng thời các lower units được
tích hợp với các top units và được test. Sử dụng Stub cũng như Driver.

### Stub và Driver là gì.

  Incremental được thực hiện bằng cách sử dụng các chương trình giả mạo có tên
là Stub và Driver. Stub và Driver không thực hiện toàn bộ logic của các units mà
chỉ mô phỏng giao tiếp dữ liệu với các units gọi nhau.

  * **Stub**: Là được gọi bới unit đang được test.
  * **Driver**: Gọi unit để test

  Hãy để kết luận một số khác biệt giữa Stub và Driver:


| Stubs     | Driver       |
|:----------|:-------------|
| Được sử dụng trong Top-down | Được sử dụng trong Bottom-up |
| Unit cao nhất phải được test đầu tiên | Unit thấp nhất phải được test đầu tiên |
| Chương trình giả của các thành phần cấp thấp hơn | Chương trình giả cho thành phần cấp cao hơn |
