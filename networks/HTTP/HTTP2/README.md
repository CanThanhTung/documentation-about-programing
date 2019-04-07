# Hypertext Transfer Protocol Version 2 (HTTP2)

HTTP là một giao protocol thành công, tuy nhiên cách mà HTTP/1.1 sử dụng vận chuyển data có một số đặc điểm ảnh hưởng 
chung đến hiệu suất. Cụ thể như 

1. HTTP/1.0 chỉ cho phép một request tại một thời điểm trên một kết nối TCP. HTTP/1.1 đã thêm 
Pipeline, nhưng điều này chỉ giải quyết được phần nào request concurrency và vẫn chịu các thiệt hại từ Head-of-line blocking.
Vì vậy Các client sử dụng HTTP/1.0 và HTTP/1.1 cần thực hiện nhiều request cần sử dụng nhiều connection tới server
để đạt được concurrency và giảm đỗ trễ
1. Các HTTP header field thường lặp lại và dài dòng, khiên tốn lưu lượng mạng không cần thiết cũng như làm cho TCP ban đầu
nhanh tróng bị lấp đầy. Điều này có thể dẫn đến độ trễ quá mức khi nhiều request được thực hiện trên kết nối TCP mới.

HTTP/2 đã giải quyết các vẫn đề này bằng các sác định mapping của các HTTP simantic tới một một connection, nó cho phép 
xen kẽ các request, response message trên cùng một connection mà sư dụng một mã hoá cho các HTTP header field. Nó cũng cho
phéo ưu tiên các request, cho phép các request quan trọng hơn được hoàn thành nhanh hơn, cải thiện hiệu suất hơn nữa. Nó
cũng thân thiện hơn với network vì có thể sự dụng ít kết nối TCP hơn với HTTP/1.x. Điều này cho thấy ít cạnh tranh hơn với 
các flow khác và kết nối lâu hơn, từ đó sử dụng tốt hơn dung lượng mạng.

## HTTP/2 Overview

HTTP/2 cung cấp một tối ưu hoá vận cho chuyển các HTTP semantic. HTTP/2 hỗ trợ tất cả các tính năng cốt lõi của HTTP/1.1
Nhưng hiệu quả hơn theo nhiều cách.

Căn bản của HTTP/2 là frame. Mỗi frame 