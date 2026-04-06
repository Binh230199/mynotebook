# Section VII - Power Tools

Section cuối của chặng Kotlin cơ bản này tập trung vào các công cụ mạnh hơn của ngôn ngữ. Đây là những tính năng mà lúc mới học bạn có thể chưa cần dùng mỗi ngày, nhưng càng viết code Android thật, bạn sẽ càng gặp chúng nhiều hơn.

Mục tiêu của section này là giúp bạn:

- Hiểu extension lambdas và scope functions
- Viết generic tốt hơn
- Làm quen với operator overloading
- Hiểu property delegation và các công cụ liên quan
- Phân biệt `lazy` và `lateinit`

Đây là những chủ đề rất hay xuất hiện trong code Kotlin hiện đại, kể cả trong Android framework, Jetpack, Compose và nhiều thư viện phổ biến.

## 1. Extension Lambdas

Extension lambda là lambda có receiver. Điều này có nghĩa là bên trong lambda, bạn có thể gọi trực tiếp các member của receiver mà không cần nhắc lại tên object.

Ví dụ kiểu hàm:

```kotlin
val greet: String.() -> String = {
    "Hello, $this"
}

println("Kotlin".greet())
```

Extension lambdas rất quan trọng vì nhiều DSL trong Kotlin được xây trên ý tưởng này. Jetpack Compose cũng khiến bạn dần quen với kiểu API rất giàu receiver context.

## 2. Scope Functions

Kotlin có một nhóm scope functions rất nổi tiếng:

- `let`
- `run`
- `with`
- `apply`
- `also`

Chúng không phải “ma thuật”, chỉ là các helper để làm việc với object trong một scope gọn hơn.

Ví dụ:

```kotlin
data class User(var name: String, var age: Int)

val user = User("Lan", 20).apply {
    age = 21
}

println(user)
```

Hiểu nhanh:

- `let`: thường dùng để xử lý nullable value hoặc chain kết quả
- `run`: chạy block và trả về kết quả cuối
- `with`: giống `run` nhưng gọi trên object truyền vào như tham số
- `apply`: cấu hình object rồi trả lại chính object đó
- `also`: làm thêm việc phụ rồi trả lại chính object đó

Nguyên tắc quan trọng:

- Scope functions rất tiện
- Nhưng nếu lồng quá nhiều, code sẽ khó đọc

## 3. Creating Generics

Ở Section III bạn đã làm quen với generic ở mức đọc hiểu. Bây giờ ta đi thêm một bước nữa: tự viết generic class và generic function.

Generic class:

```kotlin
class Box<T>(val value: T)
```

Generic function:

```kotlin
fun <T> printValue(value: T) {
    println(value)
}
```

Generic giúp bạn viết code tái sử dụng mà không đánh đổi type safety. Đây là nền tảng của nhiều API mạnh trong Kotlin và Android.

## 4. Operator Overloading

Kotlin cho phép bạn định nghĩa ý nghĩa của một số toán tử cho class riêng.

Ví dụ:

```kotlin
data class Point(val x: Int, val y: Int) {
    operator fun plus(other: Point): Point {
        return Point(x + other.x, y + other.y)
    }
}

val result = Point(1, 2) + Point(3, 4)
println(result)
```

Operator overloading có thể làm code đẹp hơn khi kiểu dữ liệu của bạn thật sự có ngữ nghĩa phù hợp với toán tử. Nhưng nếu dùng bừa bãi, code sẽ khó hiểu.

## 5. Using Operators

Kotlin có nhiều convention operator ngoài `+`, `-`, `*`, `/` thông thường.

Ví dụ:

- `in`
- `[]`
- `()`
- `+=`
- `==`

Bạn có thể định nghĩa các operator tương ứng bằng các hàm như:

- `contains`
- `get`
- `set`
- `invoke`
- `plusAssign`

Ví dụ:

```kotlin
class NameBook(private val names: List<String>) {
    operator fun contains(name: String): Boolean = name in names
}

val book = NameBook(listOf("Lan", "Mai"))
println("Lan" in book)
```

Điều quan trọng là chỉ dùng operator khi nó làm code tự nhiên hơn, không phải chỉ vì “nhìn ngầu”.

## 6. Property Delegation

Property delegation là một tính năng rất mạnh của Kotlin. Thay vì tự quản lý getter và setter, bạn có thể ủy quyền việc đó cho một delegate object.

Ví dụ đơn giản:

```kotlin
import kotlin.properties.Delegates

class User {
    var name: String by Delegates.observable("Unknown") { _, old, new ->
        println("name changed from $old to $new")
    }
}
```

Delegation giúp gom logic quản lý property vào những công cụ có sẵn hoặc delegate tự viết.

## 7. Property Delegation Tools

Kotlin standard library cung cấp một số delegation tool rất hữu ích.

### `lazy`
Khởi tạo khi dùng lần đầu.

### `observable`
Cho phép theo dõi khi giá trị thay đổi.

### `vetoable`
Cho phép chặn một thay đổi nếu không hợp lệ.

Ví dụ `vetoable`:

```kotlin
import kotlin.properties.Delegates

class ScoreHolder {
    var score: Int by Delegates.vetoable(0) { _, _, new ->
        new >= 0
    }
}
```

Đây là những công cụ cực kỳ đáng biết vì chúng giúp bạn giảm bớt code quản lý state lặp đi lặp lại.

## 8. Lazy Initialization

`lazy` dùng khi bạn muốn trì hoãn việc tạo object cho tới lúc thật sự cần dùng.

```kotlin
val config by lazy {
    println("Initializing config")
    mapOf("theme" to "dark")
}
```

Lúc này `config` chỉ được khởi tạo khi bạn truy cập tới nó lần đầu.

`lazy` hữu ích khi:

- object tạo ra tốn chi phí
- chưa cần dùng ngay lúc khởi động
- bạn muốn code gọn hơn so với tự kiểm tra null rồi khởi tạo thủ công

## 9. Late Initialization

`lateinit` cho phép bạn khai báo một `var` non-null nhưng gán giá trị sau.

```kotlin
class ScreenController {
    lateinit var title: String

    fun setup() {
        title = "Home"
    }
}
```

Nếu bạn truy cập `title` trước khi gán, chương trình sẽ lỗi.

Điều kiện dùng `lateinit`:

- chỉ áp dụng cho `var`
- không dùng cho kiểu nguyên thủy như `Int` trực tiếp theo kiểu thông thường của delegate này

Trong Android, `lateinit` từng xuất hiện khá nhiều với view binding hoặc dependency injection. Tuy nhiên, bạn nên dùng nó có kiểm soát, không xem nó là cách né null-safety.

## Kết lại Section VII

Sau section này, bạn nên chắc những ý sau:

- Extension lambdas là nền tảng của nhiều DSL Kotlin
- Scope functions rất hữu ích nhưng cần dùng có kỷ luật
- Generic cho phép viết code tổng quát và an toàn về type
- Operator overloading là công cụ mạnh nhưng phải dùng đúng ngữ cảnh
- Property delegation giúp quản lý property gọn hơn
- `lazy` và `lateinit` đều là kỹ thuật trì hoãn khởi tạo, nhưng dùng cho các tình huống khác nhau

Gợi ý luyện tập:

- Viết một class dùng `observable` để log khi property thay đổi
- Viết một object dùng `by lazy` để khởi tạo cấu hình khi cần
- Viết generic function trả về phần tử đầu tiên của list hoặc `null`
- So sánh hai cách viết code dùng `apply` và code viết trực tiếp để cảm nhận ưu nhược điểm