# What is a Makefile and how does it work 

<h3 align="center">
<image src="./assets/images/1048px-Heckert_GNU_white.svg.png"/>
</h1>

**Makefile** nói một cách đơn giản được sử dụng để tổ chức biên dịch code. Run và Complie ứng dụng của bạn một cách hiệu quả
hơn một cách tự động hoá với **Makefile*.

Nếu bạn muốn **run** hoặc **update** một tác vụ khi một số file được cập nhật, **Make Utility** có thể có ích. 
**Make Utility** yêu cầu một file, **Makefile** (hoặc **makefile**), định nghĩa tập hợp các tác vụ sẽ được thực thi. 
Bạn có thể đã sử dụng **make** để biên dịch một chương trình từ mã nguồn. Hầu hết các dự án mã nguồn mở sử dụng **make**
để biên dịch ra file thực thi binary, sau đó có thể được cài đặt bằng **make install**.

Trong bài viết này, chúng ta sẽ khám phá **make** và **Makefile** bằng các ví dụ cơ bản và nâng cao. Trước khi bạn bắt đầu, 
đảm bảo rằng cài đặt được thực hiện trong hệ thống của bạn.

## Basic examples

Bây giờ chúng ta hãy bắt đầu bằng một ví dụ cổ điển trên terminal. Tạo một thư mục trống và tậo một file có tên makefile
và chứa nội dung như bên dưới 

```makefile
say_hello: 
    echo "Hello World"
```
Bây giờ chạy file bằng các gõ **make** trong thư mục bạn vừa tạo

```bash
$ make

#stdout
echo "Hello World"
Hello World
``` 

Trong ví dụ trên, say_hello hành xử giống như một tên hàm, như trong bất kỳ ngôn ngữ lập trình nào. Đây được gọi 
là target. Các prerequisite hoặc các dependency theo target. Để đơn giản, chúng tôi chưa định nghĩa bất kỳ các prerequisite 
nào trong ví dụ này. Lệnh echo 'Hello World' được gọi là **recipe**. Recipe sử dụng các prerequisite để thực hiện một target.
Target, prerequisite và các recipe cùng nhau tạo nên một quy tắc.

Tóm lại, dưới đây là cú pháp của một quy tắc điển hình:

```bash
target: prerequisites
<TAB> recipe
```

Quay trở lại ví dụ trên, khi thực hiện make, toàn bộ lệnh echo 'Hello World' được hiển thị, theo sau là đầu ra lệnh thực tế. 
Chúng ta thường không muốn điều đó. Để chặn điều đó, chúng ta cần bắt đầu echo với @ :

```bash
say_hello:
        @echo "Hello World"
```

Bây giờ chúng ta sẽ thêm một vài target vào makefile đã được tạo trước đó.

```makefile
say_hello:
        @echo "Hello World"

generate:
        @echo "Creating empty text files..."
        touch file-{1..10}.txt

clean:
        @echo "Cleaning up..."
        rm *.txt
```

Nếu chúng tôi cố gắng run make sau khi thay đổi, chỉ có target say_hello sẽ được thực thi. Đó là bởi vì chỉ có target 
đầu tiên trong file thực hiện là target mặc định. Thường được gọi là default target, đây là lý do bạn sẽ xem tất cả 
là target đầu tiên trong hầu hết các dự án. Đó là trách nhiệm của tất cả để gọi các target khác. Chúng tôi có thể 
ghi đè hành vi này bằng cách sử dụng phony target đặc biệt có tên **.DEFAULT_GOAL**.

Hãy thử thêm nó vào đầu của makefile

```makefile
.DEFAULT_GOAL := generate
```

Như tên cho thấy, Phony target .DEFAULT_GOAL chỉ có thể chạy một target tại một thời điểm. Đây là lý do tại sao hầu hết 
các makefile bao gồm tất cả như một target có thể gọi càng nhiều target cần thiết.

Hãy bao gồm phony target **all** và xóa **.DEFAULT_GOAL**:

```makefile
all: say_hello generate

say_hello:
        @echo "Hello World"

generate:
        @echo "Creating empty text files..."
        touch file-{1..10}.txt

clean:
        @echo "Cleaning up..."
        rm *.txt
```

Trước khi run make, hãy bao gồm một phony target đặc biệt khác, .PHONY, nơi chúng tôi xác định tất cả các target không 
phải các file. make sẽ chạy công thức của nó bất kể tập tin có tên đó tồn tại hay thời gian sửa đổi cuối cùng của nó là gì. 
Đây là makefile hoàn chỉnh:

```makefile
.PHONY: all say_hello generate clean

all: say_hello generate

say_hello:
        @echo "Hello World"

generate:
        @echo "Creating empty text files..."
        touch file-{1..10}.txt

clean:
        @echo "Cleaning up..."
        rm *.txt
```

make có thể run chỉ đinh target  bằng cách chỉ định tên target đó

```bash
make clean
```


## Advanced examples

### Variables

Trong ví dụ trên, hầu hết các giá trị prerequisite và target mã hóa cứng, nhưng trong các dự án thực tế, 
chúng được thay thế bằng các variables và patterns.

Cách đơn giản nhất để xác định một variable trong makefile là sử dụng toán tử =. Ví dụ: để gán lệnh gcc cho CC biến:

```bash
CC = gcc
```

Đây cũng được gọi recursive expanded variable và nó được sử dụng theo quy tắc như dưới đây:

```bash
hello: hello.c
    ${CC} hello.c -o hello
```

Như bạn có thể đoán, recipe expands như bên dưới khi nó được chuyển đến terminal:

```bash
gcc hello.c -o hello
```

$ (CC) là các tham chiếu hợp lệ để gọi gcc. Nhưng nếu một người cố gắng gán lại một variable cho chính nó, nó sẽ gây ra 
một vòng lặp vô hạn. Hãy xác minh điều này:

```bash
CC = gcc
CC = ${CC}

all:
    @echo ${CC}
```

run make sẽ trả về lỗi

```bash
$ make
Makefile:8: *** Recursive variable 'CC' references itself (eventually).  Stop.
```

Để tránh kịch bản này, chúng ta có thể sử dụng toán tử := (đây còn được gọi là simply expanded variable). Chúng ta 
không có vấn đề gì khi chạy makefile dưới đây:

```bash
CC := gcc
CC := ${CC}

all:
    @echo ${CC}
```

### Patterns và Functions

Makefile dưới đây có thể biên dịch tất cả các chương trình C bằng cách sử dụng các variable, pattern và function. Hãy 
khám phá từng dòng một:

```bash
# Usage:
# make        # compile all binary
# make clean  # remove ALL binaries and objects

.PHONY = all clean

CC = gcc                        # compiler to use

LINKERFLAG = -lm

SRCS := $(wildcard *.c)
BINS := $(SRCS:%.c=%)

all: ${BINS}

%: %.o
        @echo "Checking.."
        ${CC} ${LINKERFLAG} $< -o $@

%.o: %.c
        @echo "Creating object.."
        ${CC} -c $<

clean:
        @echo "Cleaning up..."
        rm -rvf *.o ${BINS}
```

* Dòng bắt đầu với # là comments

* Dòng .PHONY = all clean = định nghĩa phony targets all và clean.

* Biến LINKERFLAG định nghĩa các flag được sử dụng với gcc trong recipe.

* SRCS: = $ (wildcard * .c): $ (wildcard pattern) là một trong các chức năng cho filename. Trong trường hợp này,
 tất cả các tệp có .c sẽ được lưu trữ trong một variable SRCS.

* BINS: = $ (SRCS:%. C =%): Đây được gọi là tham chiếu thay thế. Trong trường hợp này, nếu SRCS có các giá trị 'foo.c bar.c', 
BINS sẽ có 'foo bar'.

* Dòng all: $ {BINS}: phony target **all** gọi các giá chị trong ${BINS} dưới dạng các target riêng lẻ.

* Quy tắc 

```bash
%: %.o
  @echo "Checking.."
  ${CC} ${LINKERFLAG} $&lt; -o $@
```

Hãy xem xét một ví dụ để hiểu quy tắc này. Giả sử foo là một trong những giá trị trong ${BINS}. Sau đó % sẽ khớp với foo 
(% có thể khớp với bất kỳ tên target nào). Dưới đây là quy tắc ở dạng mở rộng của nó:

```bash
foo: foo.o
  @echo "Checking.."
  gcc -lm foo.o -o foo
```

 Như được hiển thị,% được thay thế bởi foo. $ <được thay thế bởi foo.o. $ <được mô hình hóa để phù hợp để phù hợp với 
các prerequisite và $ khớp với target. Quy tắc này sẽ được gọi cho mọi giá trị trong $ {BINS}

* Quy tắc 

```bash
%.o: %.c
  @echo "Creating object.."
  ${CC} -c $&lt;
```

Mọi prerequisite trong quy tắc trước được coi là mục tiêu cho quy tắc này. Dưới đây là quy tắc ở dạng mở rộng của nó:

```bash
foo.o: foo.c
  @echo "Creating object.."
  gcc -c foo.c
```

* Cuối cùng, chúng tôi xóa tất cả các file binary và object file trong target clean.

Dưới đây là viết lại của makefile ở trên, giả sử nó được đặt trong thư mục có một tệp foo.c:

```bash
# Usage:
# make        # compile all binary
# make clean  # remove ALL binaries and objects

.PHONY = all clean

CC = gcc                        # compiler to use

LINKERFLAG = -lm

SRCS := foo.c
BINS := foo

all: foo

foo: foo.o
        @echo "Checking.."
        gcc -lm foo.o -o foo

foo.o: foo.c
        @echo "Creating object.."
        gcc -c foo.c

clean:
        @echo "Cleaning up..."
        rm -rvf foo.o foo
```