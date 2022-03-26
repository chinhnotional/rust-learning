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
Nếu bạn khai báo một tên biến và khởi tạo nó sau đó ở một chỗ khác, trình biên dịch sẽ ngăn bạn sử dụng nó trước khi nó được khởi tạo:
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
Dấu chấm phẩy đánh dấu kết thúc một câu lệnh (statement):
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
Từ khóa **fn** được sử dụng để khai báo một function.
Dưới đây là một void function:
```
fn greet() {
    println!("Hi there!");
}
```
Còn đây là một hàm trả về một số nguyên có dấu 32 bit. Mũi tên chỉ định kiểu trả về của nó:
```
fn fair_dice_roll() -> i32 {
    4
}
```
Cặp dấu ngoặc nhọn dùng để khai báo một khối (block), mỗi khối đều có phạm vi (scope) riêng của nó:
```
// This prints "in", then "out"
fn main() {
    let x = "out";
    {
        // this is a different `x`
        let x = "in";
        println!("{}", x);
    }
    println!("{}", x);
}
```
Các khối cũng là các biểu thức (expression), có nghĩa là chúng có thể trả về một giá trị:
```
// this:
let x = 42;

// is equivalent to this:
let x = { 42 };
```
Bên trong một khối, có thể có nhiều câu lệnh:
```
let x = {
    let y = 1; // first statement
    let z = 2; // second statement
    y + z // this is the *tail* - what the whole block will evaluate to
};
```
Bạn có thể để ý ở những ví dụ trên, đều không có dấu chấm phẩy đằng sau giá trị trả về của một khối, bởi hai điều sau tương đương với nhau:
```
fn fair_dice_roll() -> i32 {
    return 4;
}

fn fair_dice_roll() -> i32 {
    4
}
```
Câu lệnh điều kiện với **if** cũng là một loại biểu thức:
```
fn fair_dice_roll() -> i32 {
    if feeling_lucky {
        6
    } else {
        4
    }
}
```
**match** cũng là một loại biểu thức:
```
fn fair_dice_roll() -> i32 {
    match feeling_lucky {
        true => 6,
        false => 4,
    }
}
```
Dấu chấm thường được sử dụng để truy cập vào các trường của một giá trị:
```
let a = (10, 20);
a.0; // this is 10

struct People {
    name: String,
    age: u8,
}
let people1 = People {
    name: String::from("Chinh"),
    age: 18
}
people.name; // this is "Chinh"
```
Dấu chấm cũng có thể dùng để gọi method trên một giá trị:
```
let nickname = "chinh";
nickname.len(); // this is 5
```
Double-colon, **::** , cũng được dùng tương tự để gọi method nhưng nó hoạt động trên namespace.

Trong ví dụ này, **std** là một _crate_ (a library), **cmp** là một _module_ (a source file) và **min** là một _function_:
```
let least = std::cmp::min(3, 8); // this is 3
```
Chỉ thị **use** được sử dụng để import các đối tượng từ một namespace khác vào scope và từ đó ta có thể tái sử dụng code đã được đóng gói trong một package khác:
```
use std::cmp::min;

let least = min(7, 1); // this is 1
```
Các cách sử dụng **use** dưới đây là tương tự nhau:
```
// this works:
use std::cmp::min;
use std::cmp::max;

// this also works:
use std::cmp::{min, max};

// this also works!
use std::{cmp::min, cmp::max};
```
Với (*), cho phép bạn import mọi đối tượng từ một namespace:
```
// this brings `min` and `max` in scope, and many other things
use std::cmp::*;
```
Các types cũng là namespace và các phương thức có thể được gọi như các hàm thông thường:
```
let x = "amos".len(); // this is 4
let x = str::len("amos"); // this is also 4
```
**str** là một kiểu dữ liệu nguyên thủy (primitive type), nhưng cũng có nhiều kiểu dữ liệu không nguyên thủy (non-primitive type) cũng nằm trong phạm vi theo mặc định:
```
// `Vec` is a regular struct, not a primitive type
let v = Vec::new();

// this is exactly the same code, but with the *full* path to `Vec`
let v = std::vec::Vec::new();
```
Trong đoạn code trên, hai câu lệnh khai báo biến **v** hoạt động tương tự nhau, nhưng chúng ta chỉ cần sử dụng cách 1 mà không viết như cách 2 bởi Rust đã đưa vào scope theo mặc định khá nhiều thứ như Vec, String, Option và Result:
```
use std::prelude::v1::*;
```
