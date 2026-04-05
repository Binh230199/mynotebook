# Section I - Programming Basics

Section này là nền móng của toàn bộ quá trình học Kotlin. Nếu phần này chưa chắc, bạn sẽ rất dễ bị chậm lại khi học Android, vì gần như mọi đoạn code Android đều dựa vào các khái niệm cơ bản như biến, kiểu dữ liệu, hàm, điều kiện, vòng lặp và biểu thức.

Mục tiêu của section này là giúp bạn:

- Đọc được cú pháp Kotlin cơ bản
- Phân biệt được `val` và `var`
- Viết được hàm đơn giản
- Dùng được `if`, `while`, `for`, range và `in`
- Hiểu vì sao Kotlin là ngôn ngữ thiên về expression

## 1. Introduction

Kotlin là một ngôn ngữ hiện đại chạy trên JVM và được Google hỗ trợ chính thức cho Android. Nó được thiết kế để ngắn gọn hơn Java, an toàn hơn ở những lỗi phổ biến như `NullPointerException`, và dễ đọc hơn khi dự án lớn dần lên.

Điểm đáng chú ý đầu tiên của Kotlin là cú pháp khá gọn. Bạn thường phải viết ít mã hơn so với Java nhưng vẫn giữ được ý nghĩa rõ ràng. Điều này rất phù hợp với Android, nơi code UI, state và logic có thể tăng rất nhanh khi ứng dụng lớn hơn.

Khi học Kotlin, đừng cố nhớ mọi tính năng ngay từ đầu. Hãy bắt đầu từ những mảnh ghép nền tảng nhất: biến, kiểu dữ liệu, hàm, điều kiện và vòng lặp. Đây là phần bạn sẽ dùng mỗi ngày.

## 2. Why Kotlin?

Vì sao Android hiện nay ưu tiên Kotlin thay vì Java? Có một số lý do rất thực tế.

### Kotlin ngắn gọn hơn
Nhiều đoạn code lặp và ồn trong Java được rút gọn đáng kể trong Kotlin. Ví dụ, khai báo property, viết hàm ngắn, hay tạo model data đều tự nhiên hơn.

### Kotlin an toàn hơn
Kotlin có hệ thống nullability rõ ràng. Nó buộc bạn phải nghĩ tới trường hợp giá trị có thể là `null`, nhờ đó giảm nhiều lỗi runtime rất phổ biến.

### Kotlin phù hợp với Android hiện đại
Jetpack Compose, coroutines, Flow, Room, Ktor, Retrofit với adapter hiện đại, và rất nhiều thư viện Android mới đều ưu tiên Kotlin-first.

### Kotlin vẫn tương thích tốt với Java
Nhiều dự án Android cũ vẫn còn Java. Kotlin có thể gọi Java và Java cũng có thể gọi Kotlin. Điều này giúp migration diễn ra dần dần, không cần viết lại toàn bộ dự án.

### Kotlin hỗ trợ tốt cả hướng đối tượng lẫn hướng hàm
Bạn có thể viết code OOP truyền thống, nhưng cũng có thể tận dụng lambda, higher-order functions, extensions và collection operations rất mạnh. Điều này đặc biệt hữu ích trong Android hiện đại.

## 3. Hello, World!

Chương trình Kotlin đơn giản nhất thường bắt đầu với hàm `main()`.

```kotlin
fun main() {
    println("Hello, World!")
}
```

Giải thích nhanh:

- `fun` dùng để khai báo hàm
- `main` là điểm vào của chương trình
- `println()` in nội dung ra console

Nếu bạn mới học, đây là điều cần để ý: Kotlin không cần `class Main` chỉ để chạy chương trình cơ bản như Java truyền thống. Bạn có thể bắt đầu trực tiếp bằng hàm.

Trong thực tế Android, bạn hiếm khi viết `main()` như trên, nhưng việc hiểu chương trình Kotlin tối giản giúp bạn học ngôn ngữ mà không bị nhiễu bởi Android framework.

## 4. var & val

Đây là một trong những khái niệm đầu tiên và quan trọng nhất.

- `val` là biến chỉ gán một lần
- `var` là biến có thể gán lại

Ví dụ:

```kotlin
fun main() {
    val appName = "MyApp"
    var counter = 0

    counter = counter + 1

    println(appName)
    println(counter)
}
```

`val` không có nghĩa là dữ liệu bên trong luôn bất biến trong mọi trường hợp, nhưng nó có nghĩa là biến đó không thể được gán lại sang giá trị khác.

Nguyên tắc rất nên tập từ đầu:

- Ưu tiên `val`
- Chỉ dùng `var` khi thật sự cần thay đổi giá trị

Trong Android, thói quen này giúp code dễ hiểu hơn, giảm sai sót khi quản lý state.

## 5. Data Types

Kotlin có nhiều kiểu dữ liệu quen thuộc:

- `Int`
- `Long`
- `Double`
- `Float`
- `Boolean`
- `String`
- `Char`

Ví dụ:

```kotlin
val age: Int = 28
val price: Double = 19.99
val isReady: Boolean = true
val title: String = "Kotlin Basics"
val grade: Char = 'A'
```

Kotlin hỗ trợ type inference, nghĩa là compiler thường tự đoán được kiểu dữ liệu:

```kotlin
val age = 28
val title = "Kotlin Basics"
```

Bạn không nhất thiết phải ghi kiểu mọi lúc, nhưng nên hiểu kiểu thật sự của giá trị là gì. Khi code phức tạp hơn, việc hiểu type là nền tảng để đọc lỗi compiler.

## 6. Functions

Hàm trong Kotlin được khai báo bằng `fun`.

```kotlin
fun greet(name: String): String {
    return "Hello, $name"
}
```

Giải thích:

- `greet` là tên hàm
- `name: String` là tham số
- `: String` là kiểu dữ liệu trả về

Kotlin còn hỗ trợ expression body cho hàm ngắn:

```kotlin
fun greet(name: String): String = "Hello, $name"
```

Nếu hàm không trả về giá trị có ý nghĩa, nó trả về `Unit`:

```kotlin
fun logMessage(message: String) {
    println(message)
}
```

Người mới nên tập một số thói quen tốt:

- Hàm nên làm một việc rõ ràng
- Tên hàm nên mô tả ý nghĩa
- Tránh viết một hàm quá dài ngay từ đầu

## 7. if Expressions

Trong Kotlin, `if` không chỉ là câu lệnh điều kiện, mà còn có thể là một expression, nghĩa là nó trả về giá trị.

```kotlin
val max = if (a > b) a else b
```

Ví dụ đầy đủ hơn:

```kotlin
fun maxOf(a: Int, b: Int): Int {
    return if (a > b) a else b
}
```

Điểm này rất quan trọng vì Kotlin thích phong cách expression hơn là khai báo biến rồi gán nhiều lần.

So sánh hai kiểu viết:

```kotlin
val message = if (score >= 50) "Pass" else "Fail"
```

thường gọn và rõ hơn kiểu:

```kotlin
var message = ""
if (score >= 50) {
    message = "Pass"
} else {
    message = "Fail"
}
```

## 8. String Templates

Kotlin hỗ trợ nội suy chuỗi rất tiện bằng ký hiệu `$`.

```kotlin
val name = "Binh"
println("Hello, $name")
```

Nếu cần chèn biểu thức phức tạp hơn, dùng `${...}`:

```kotlin
val a = 10
val b = 20
println("Sum = ${a + b}")
```

Đây là một trong những tính năng giúp code Kotlin đọc tự nhiên hơn, đặc biệt khi log dữ liệu hoặc tạo thông báo UI.

## 9. Number Types

Kotlin có các kiểu số quen thuộc:

- `Byte`
- `Short`
- `Int`
- `Long`
- `Float`
- `Double`

Trong thực tế, bạn dùng nhiều nhất là:

- `Int` cho số nguyên thông thường
- `Long` cho giá trị lớn hơn
- `Double` cho số thực chính xác hơn `Float`

Ví dụ:

```kotlin
val count: Int = 10
val distance: Long = 3_000_000_000
val price: Double = 99.95
```

Kotlin không tự chuyển kiểu số một cách ngầm định như một số ngôn ngữ khác. Nếu cần chuyển, bạn phải gọi rõ:

```kotlin
val number = 10
val longNumber = number.toLong()
```

Điều này giúp tránh lỗi khó đoán.

## 10. Booleans

Kiểu `Boolean` chỉ có hai giá trị:

- `true`
- `false`

Ví dụ:

```kotlin
val isLoggedIn = true
val hasPermission = false
```

Các toán tử logic phổ biến:

- `&&` và
- `||` hoặc
- `!` phủ định

Ví dụ:

```kotlin
if (isLoggedIn && hasPermission) {
    println("Access granted")
}
```

Trong Android, Boolean xuất hiện khắp nơi: loading state, permission state, form validation, feature flags. Vì vậy, bạn nên đặt tên biến Boolean rõ nghĩa như `isLoading`, `hasError`, `canRetry`.

## 11. Repetition with while

`while` dùng khi bạn muốn lặp chừng nào điều kiện còn đúng.

```kotlin
var count = 0

while (count < 3) {
    println(count)
    count++
}
```

Kotlin cũng có `do...while`, nghĩa là khối lệnh chạy ít nhất một lần trước khi kiểm tra điều kiện.

```kotlin
var count = 0

do {
    println(count)
    count++
} while (count < 3)
```

Người mới thường gặp lỗi vòng lặp vô hạn. Vì vậy, hãy luôn tự hỏi:

- Điều kiện dừng là gì?
- Biến điều khiển vòng lặp có thực sự thay đổi không?

## 12. Looping & Ranges

Kotlin có cú pháp lặp với range rất dễ đọc.

```kotlin
for (i in 1..5) {
    println(i)
}
```

Một số biến thể rất hay dùng:

```kotlin
for (i in 0 until 5) {
    println(i)
}

for (i in 10 downTo 1 step 2) {
    println(i)
}
```

Giải thích:

- `1..5` gồm cả 1 và 5
- `until` không lấy cận trên
- `downTo` lặp ngược
- `step` thay đổi bước nhảy

Range được dùng rất nhiều trong Kotlin, không chỉ để lặp mà còn để kiểm tra giá trị nằm trong khoảng nào.

## 13. The in Keyword

Từ khóa `in` có nhiều vai trò quan trọng.

### Dùng trong vòng lặp

```kotlin
for (item in items) {
    println(item)
}
```

### Dùng để kiểm tra thuộc về range hoặc collection

```kotlin
val x = 5
println(x in 1..10)
```

```kotlin
val names = listOf("A", "B", "C")
println("B" in names)
```

`in` giúp code đọc gần với ngôn ngữ tự nhiên hơn, và bạn sẽ gặp nó liên tục trong Kotlin.

## 14. Expressions & Statements

Đây là một ý tưởng quan trọng trong Kotlin.

- **Statement** là thứ thực hiện hành động
- **Expression** là thứ tạo ra giá trị

Kotlin ưu tiên expression hơn nhiều ngôn ngữ cũ.

Ví dụ:

```kotlin
val result = if (score > 50) "Pass" else "Fail"
```

Ở đây `if` là expression vì nó trả về giá trị.

Việc hiểu tư duy expression sẽ giúp bạn đọc Kotlin hiện đại dễ hơn, đặc biệt khi sang Compose, scope functions, lambdas và `when`.

## 15. Summary 1

Sau section này, bạn nên chắc những ý sau:

- Kotlin dùng `fun` để khai báo hàm
- `val` là lựa chọn mặc định tốt hơn `var`
- Kotlin hỗ trợ type inference nhưng bạn vẫn phải hiểu type thật sự
- `if` có thể trả về giá trị
- String templates giúp tạo chuỗi rõ ràng hơn
- `while`, `for`, range và `in` là công cụ nền tảng để điều khiển luồng
- Kotlin thiên về expression, đây là tư duy rất quan trọng cho những phần sau

Checklist tự kiểm tra:

1. Bạn có viết được một hàm nhận tham số và trả về kết quả không?
2. Bạn có phân biệt được `val` với `var` không?
3. Bạn có biết khi nào dùng `1..10` và khi nào dùng `until` không?
4. Bạn có hiểu vì sao `if` trong Kotlin có thể dùng để gán giá trị không?

Gợi ý luyện tập:

- Viết hàm kiểm tra số chẵn lẻ
- Viết chương trình in bảng cửu chương từ 2 đến 5
- Viết hàm nhận tên và tuổi rồi tạo câu mô tả bằng string template
- Viết chương trình dùng `if` expression để phân loại điểm học tập