# Ram timing

    RAM là một thành phần quan trọng trong máy tính. Bạn có thể thực hiện ép xung RAM 
để tăng tốc nó. Cả tốc độ xung nhịp và timing hoặc độ trễ đều quyết định đến tốc độ RAM.
Vậy RAM timing là gì và nó có ảnh hưởng như thế nào đến RAM? Hãy cùng khám phá trong 
bài viết sau đây nhé.

## Tìm tốc độ xung nhịp của RAM

Bạn có thể tìm tốc độ của RAM trên hộp hoặc module bằng phần mềm như CPU-Z hoặc trong 
BIOS/UEFI. Tên đầy đủ của module RAM sẽ giống như thế này:

```
DDR4 3200 (PC4 25600)
```

    DDR4 mô tả thế hệ DDR mà chip tương thích. Cùng số đó (2, 3 hoặc 4) xuất hiện trong 
PC cũng chỉ điều tương tự. 

    Số 4 chữ số đầu tiên, 3200 trong ví dụ này, thường là tốc độ xung nhịp của RAM tính 
bằng megahertz. Con số này thực sự chỉ tốc độ dữ liệu, được tính bằng megatransfer mỗi
giây hoặc 106 hoạt động truyền dữ liệu mỗi giây.

    Trong RAM DDR, tốc độ xung nhịp thực tế bằng một nửa tốc độ đọc dữ liệu, 1600 MHz 
trong ví dụ này. Mặc dù tốc độ đó đã được tăng lên từ tốc độ xung nhịp nội bộ 400 MHz 
của RAM qua các bit pre-fetch được nhân lên nhiều lần. Nhưng vì DDR truyền dữ liệu gấp 
đôi tốc độ xung nhịp, tốc độ xung nhịp hiệu quả gấp đôi tốc độ xung nhịp thực tế. Do đó, 
tốc độ dữ liệu có hiệu quả tương đương tốc độ xung nhịp rõ ràng của RAM tính bằng MHz.

    Số PC, 25600 trong ví dụ này biểu thị tốc độ truyền được đo bằng megabyte trên giây 
(MB/s). Bằng cách nhân tốc độ dữ liệu (tính bằng megatransfer) với chiều rộng của I/O 
bus (64 bit trong tất cả các bo mạch chủ hiện đại), chúng ta có thể xác định tốc độ 
truyền tối đa nhất có thể:

    3200 megatransfer trên giây x 64 bit trên transfer/8 bit trên byte = 25600 MB/s

    Mỗi con số đều cho biết cùng một thông tin, tốc độ RAM nhưng khác hình thức.

## RAM timing là gì?

    Timing là một cách khác để đo tốc độ RAM. Timing đo độ trễ giữa các hoạt động 
phổ biến khác nhau trên chip RAM. Độ trễ là thời gian trì hoãn giữa các hoạt động. 
Nó có thể được coi là thời gian chờ. 

    Chúng ta đo RAM timing trong chu kỳ xung nhịp. Các nhà bán lẻ liệt kê timing 
với bốn số được tách nhau bằng dấu gạch ngang như 16-18-18-38. Số càng nhỏ, tốc độ 
càng nhanh. Thứ tự của các con số cho bạn biết ý nghĩa của chúng.

#### Số đầu tiên: Thông số CAS Latency (CL)

```
'16'-18-18-38
```

    Đây là thời gian bộ nhớ cần để phản ứng với CPU. Nhưng CL không được xem xét một 
cách riêng biệt. Công thức này chuyển đổi CL timing thành nano giây, dựa trên tốc độ 
truyền của RAM.

```
(CL/Transfer Rate) x 2000
```

    Kết quả là RAM chậm hơn có độ trễ thực tế thấp hơn nếu có CL ngắn hơn.

#### Số thứ hai: TRCD

```
16-'18'-18-38
```

    Các module RAM sử dụng thiết kế dựa trên lưới để ghi địa chỉ. Giao điểm của 
hàng ngang và cột biểu thị địa chỉ bộ nhớ cụ thể. Thông số TRCD đo độ trễ tối 
thiểu giữa việc nhập một hàng mới trong bộ nhớ và bắt đầu truy cập các cột trong 
đó. Bạn có thể nghĩ nó như thời gian cần thiết để RAM đến được địa chỉ. Thời gian 
cần để nhận bit đầu tiên từ một hàng không hoạt động trước đó là TRCD + CL.

#### Số thứ ba: TRP

```
16-18-'18'-38
```

    Row Precharge Time (TRP) đo độ trễ liên quan đến việc mở một hàng mới trong 
bộ nhớ. Về mặt kỹ thuật, nó đo độ trễ giữa việc phát lệnh precharge để chờ 
(hoặc đóng) một hàng và lệnh kích hoạt để mở một hàng khác, giống với số thứ 
hai. Các yếu tố tương tự ảnh hưởng đến độ trễ của cả hai hoạt động.

#### Số thứ tư: TRAS

```
16-18-18-'38'
```

    Row Active Time (TRAS) đo số chu kỳ tối thiểu mà một hàng cần để duy trì tình 
trạng mở để ghi dữ liệu thích hợp. Về mặt kỹ thuật, nó đo độ trễ giữa các lệnh kích 
hoạt trên một hàng và lệnh precharge trên cũng một hàng đó hoặc thời gian tối 
thiểu giữa lần mở và đóng hàng. Đối với module SDRAM, thực hiện TRCD + CL để tính TRAS.

    Những độ trễ này làm hạn chế tốc độ RAM. Bộ điều khiển bộ nhớ quản lý RAM thực 
thi các timing này, nghĩa là chúng có thể điều chỉnh được (nếu bo mạch chủ cho phép). Bạn có thể tăng hiệu suất RAM bằng cách ép xung và thiết chặt timing trong một vài 
chu kỳ.

Ép xung RAM là kỹ thuật ép xung phần cứng phổ biến nhất, đòi hỏi phải thử nghiệm nhiều
nhất. Nhưng RAM nhanh hơn rút ngắn thời gian xử lý khối lượng công việc sử dụng RAM, 
cải thiện tốc độ kết xuất và khả năng đáp ứng của máy ảo.