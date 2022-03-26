# 30 phút học Rust

Để có thể sử dụng thuần thục một ngôn ngữ lập trình, người ta phải đọc rất nhiều về ngôn ngữ đó. Nhưng làm thế nào mà bạn có thể đọc nó nhiều nếu ban đầu bạn còn không biết nó là cái gì?

Trong bài viết này, thay vì tập trung vào một hoặc hai khái niệm, chúng ta sẽ cố gắng xem qua càng nhiều đoạn mã Rust càng tốt và giải thích ý nghĩa của các từ khóa và ký hiệu mà chúng chứa.

Sẵn sàng chưa? Bắt đầu nào!

Từ khóa **let** được sử dụng trong ràng buộc biến (variable binding):
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
Nếu bạn khai báo một tên biến và khởi tạo nó sau đó trong một dòng lệnh khác, trình biên dịch sẽ ngăn bạn sử dụng nó trước khi nó được khởi tạo:
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
Dấu gạch dưới  **_**  là một tên biến đặc biệt - hay đúng hơn là "thiếu tên". Về cơ bản, nó được sử dụng để  loại bỏ (throw away) một cái gì đó:
```
// this does *nothing* because 42 is a constant
let _ = 42;

// this calls `get_thing` but throws away its result
let _ = get_thing();
```
Những tên biến bắt đầu bằng dấu gạch dưới cũng giống như những tên biến thông thường, khác ở chỗ trình biên dịch sẽ không cảnh báo khi chúng không được sử dụng:
```
// we may use `_x` eventually, but our code is a work-in-progress
// and we just wanted to get rid of a compiler warning for now.
let _x = 42;
```
Có thể khai báo lại một biến, cái này gọi là _Shadowing_ - nói cách khác, bạn có thể _shadow_ một ràng buộc biến:
```
let x = 13;
let x = x + 3;
// using `x` after that line only refers to the second `x`,
// the first `x` no longer exists.
```
Giống như một vài ngôn ngữ khác, trong Rust cũng có tuple - một dạng dữ liệu lưu trữ các đối tượng có thứ tự và bất biến:
```
let pair = ('a', 17);
pair.0; // this is 'a'
pair.1; // this is 17
```
Nếu bạn muốn chú thích kiểu cho **pair**, viết như sau:
```
let pair: (char, i32) = ('a', 17);
```
Các tuple có thể được destructured, có nghĩa là chúng được chia thành các trường riêng lẻ:
```
let (some_char, some_int) = ('a', 17);
// now, `some_char` is 'a', and `some_int` is 17
```
Điều này đặc biệt hữu ích khi một hàm trả về một tuple:
```
let (left, right) = slice.split_at(middle); // middle is a tuple 
```
Tất nhiên, khi hủy cấu trúc một tuple, **_** có thể được sử dụng để loại bỏ một phần không dùng đến của nó:
```
let (_, right) = slice.split_at(middle);
```
Dấu chấm phẩy đánh dấu kết thúc một câu lệnh:
```
let x = 3;
let y = 5;
let z = y + x;
```
Điều này có nghĩa là một câu lệnh có thể nằm ở trên nhiều dòng:
```
let x = vec![1, 2, 3, 4, 5, 6, 7, 8]
    .iter()
    .map(|x| x + 3)
    .fold(0, |x, y| x + y);
```