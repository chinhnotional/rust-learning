# 30 phút học Rust

Để có thể sử dụng thuần thục một ngôn ngữ lập trình, người ta phải đọc rất nhiều về ngôn ngữ đó. Nhưng làm thế nào mà bạn có thể đọc nó nhiều nếu ban đầu bạn còn không biết nó là cái gì?

Trong bài viết này, thay vì tập trung vào một hoặc hai khái niệm, chúng ta sẽ cố gắng xem qua càng nhiều đoạn mã Rust càng tốt và giải thích ý nghĩa của các từ khóa và ký hiệu mà chúng chứa.

Sẵn sàng chưa? Bắt đầu nào!

Từ khóa **let** được sử dụng trong ràng buộc biến:
```
let x; // declare "x"
x = 42; // assign 42 to "x"
```
Công việc trên có thể thực hiện chỉ trong một dòng:
```
let x = 42;
```
Bạn có thể chỉ định kiểu của biến một cách tường minh với **:** , một kiểu annotation:
```
let x: i32; // 'i32' is a signed 32-bit integer
x = 42;

// there's i8, i16, i32, i64, i128
//    also u8, u16, u32, u64, u128 for unsigned
```
Điều trên cũng có thể thực hiện chỉ trong một dòng:
```
let x: i32 = 42;
```
Nếu bạn khai báo một biến và khởi tạo nó sau đó trong một dòng lệnh khác, trình biên dịch sẽ ngăn bạn sử dụng nó trước khi nó được khởi tạo:
```
let x;
foobar(x); // error: borrow of possibly-uninitialized variable: `x`
x = 42;
```
Để sửa lỗi ở trên, bạn cần khởi tạo biến trước khi nó được được dùng ở đâu đó:
```
let x;
x = 42;
foobar(x); // the type of `x` will be inferred from here
```