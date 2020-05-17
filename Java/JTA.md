# Java Transaction API

  Java Transaction API thường được gọi là JTA là Java Enterprise Edition (Java EE) APIs.
một API cho quản lý transactions trong Java cho phép các ứng dụng thực hiện các
distributed transactions, nghĩa là các transactions access và update data trên
hai hoặc nhiều resources được kết nối với nhau qua network. JTA chỉ định các
Java interfaces tiêu chuẩn giữa transaction manager và các phần liên quan đến
distributed transaction system

  Một transaction xác định một công việc hoàn toàn thành công hoặc không tạo ra
thay đổi nào cả. Một distributed transaction chỉ đơn giản là một transaction acess
và update data trên hai hoặc nhiều resources được nối mạng và do đó phải được phối
hợp giữa các resources đó. Trong tài liệu này, chúng ta quan tâm chủ yếu đến các
transaction liên quan đến các hệ thống RDBMS.

  Trong các phần sau, chúng ta mô tả các thành phần này và mối quan hệ của chúng
với JTA và acess vào DATABASE.

## Accessing Databases

  Tốt nhất nên nghĩ về các thành phần liên quan đến giao dịch phân tán như các
quy trình độc lập, thay vì về vị trí trên một máy tính cụ thể. Một số thành phần
có thể nằm trên một máy hoặc chúng có thể được trải đều giữa một số máy. Các sơ
đồ trong các ví dụ sau có thể hiển thị một thành phần trên một máy tính cụ thể,
nhưng mối quan hệ giữa các quy trình là sự cân nhắc chính.
