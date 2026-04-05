# Section III - Usability

Section này tập trung vào những tính năng khiến Kotlin trở nên tiện dụng hơn khi viết code hằng ngày. Đây là phần giúp code ngắn hơn, rõ hơn, ít lặp hơn, và đặc biệt rất gần với phong cách code Android hiện đại.

Mục tiêu của section này là giúp bạn:

- Dùng được extension functions và extension properties
- Hiểu named arguments, default arguments và overloading
- Nắm `when`, `enum`, `data class`, destructuring
- Làm chủ nullability và các toán tử an toàn
- Làm quen với generics ở mức nền tảng

## 1. Extension Functions

Extension function cho phép bạn thêm hành vi cho một kiểu dữ liệu mà không cần sửa chính class đó.

```kotlin
fun String.lastChar(): Char = this[this.lastIndex]
```

Sử dụng:

```kotlin
println("Kotlin".lastChar())
```

Đây là một tính năng rất mạnh vì nó giúp code tự nhiên hơn. Trong Android, bạn sẽ thấy extension function dùng rất nhiều để:

- format dữ liệu
- chuyển đổi kiểu
- viết helper cho `Context`, `View`, `String`, `List`

Điều quan trọng cần nhớ: extension function không thực sự sửa class gốc. Nó chỉ là cú pháp tiện lợi do compiler hỗ trợ.

## 2. Named & Default Arguments

Kotlin cho phép gọi hàm bằng cách chỉ rõ tên tham số.

```kotlin
fun createUser(name: String, age: Int, isAdmin: Boolean) {
    println("name=$name, age=$age, isAdmin=$isAdmin")
}

createUser(name = "Lan", age = 20, isAdmin = false)
```

Điều này giúp lời gọi hàm dễ đọc hơn, đặc biệt khi hàm có nhiều tham số cùng kiểu.

Kotlin cũng hỗ trợ default arguments:

```kotlin
fun greet(name: String, prefix: String = "Hello") {
    println("$prefix, $name")
}

greet("Binh")
greet("Lan", prefix = "Hi")
```

Trong Android, named và default arguments giúp giảm rất nhiều số lượng overload không cần thiết.

## 3. Overloading

Overloading là khai báo nhiều hàm cùng tên nhưng khác danh sách tham số.

```kotlin
fun showMessage(message: String) {
    println(message)
}

fun showMessage(message: String, tag: String) {
    println("[$tag] $message")
}
```

Overloading hữu ích khi cùng một hành động có nhiều cách gọi hợp lý. Tuy nhiên, Kotlin thường ưu tiên default arguments hơn là tạo quá nhiều overload đơn giản.

Người mới nên nhớ:

- Overloading có ích
- Nhưng nếu default arguments đủ giải quyết, đó thường là cách gọn hơn

## 4. when Expressions

`when` là một trong những tính năng rất đẹp của Kotlin. Nó giống `switch` trong ngôn ngữ khác, nhưng mạnh hơn và có thể dùng như expression.

```kotlin
fun describeNumber(x: Int): String = when (x) {
    0 -> "zero"
    1 -> "one"
    else -> "many"
}
```

`when` có thể kiểm tra:

- giá trị cụ thể
- nhiều giá trị trên cùng nhánh
- khoảng giá trị
- kiểu dữ liệu với `is`

Ví dụ:

```kotlin
fun classify(score: Int): String = when (score) {
    in 0..49 -> "Fail"
    in 50..79 -> "Pass"
    in 80..100 -> "Excellent"
    else -> "Invalid"
}
```

`when` xuất hiện liên tục trong Android, đặc biệt khi xử lý UI state, event, action type và sealed class.

## 5. Enumerations

Enum class dùng để mô tả một tập giá trị hữu hạn và rõ nghĩa.

```kotlin
enum class Direction {
    NORTH, SOUTH, EAST, WEST
}
```

Sử dụng:

```kotlin
val direction = Direction.NORTH
println(direction.name)
println(direction.ordinal)
```

Enum phù hợp khi bạn có số lượng trạng thái nhỏ, cố định, ví dụ:

- hướng
- loại tài khoản
- chế độ hiển thị
- trạng thái đơn giản

Nếu trạng thái phức tạp hơn hoặc có dữ liệu đi kèm, sealed class thường phù hợp hơn.

## 6. Data Classes

Data class là một trong những tính năng khiến Kotlin rất tiện để viết model.

```kotlin
data class User(
    val name: String,
    val age: Int
)
```

Khi dùng data class, Kotlin tự sinh:

- `toString()`
- `equals()`
- `hashCode()`
- `copy()`
- `componentN()` cho destructuring

Ví dụ:

```kotlin
val user1 = User("Lan", 20)
val user2 = user1.copy(age = 21)

println(user1)
println(user2)
```

Trong Android, data class được dùng cực nhiều cho:

- UI state
- domain model
- network response model
- local database model

## 7. Destructuring Declarations

Destructuring cho phép tách object thành nhiều biến cùng lúc.

```kotlin
data class Point(val x: Int, val y: Int)

val point = Point(10, 20)
val (x, y) = point

println(x)
println(y)
```

Bạn cũng gặp destructuring khi lặp qua map:

```kotlin
val scores = mapOf("Lan" to 10, "Mai" to 9)

for ((name, score) in scores) {
    println("$name -> $score")
}
```

Destructuring làm code gọn hơn, nhưng đừng lạm dụng nếu nó làm mất đi ý nghĩa của object ban đầu.

## 8. Nullable Types

Null-safety là một phần rất quan trọng của Kotlin.

Trong Kotlin:

- `String` nghĩa là không được `null`
- `String?` nghĩa là có thể `null`

Ví dụ:

```kotlin
val name: String = "Lan"
val nickname: String? = null
```

Nếu bạn cố gọi method trên một nullable value mà không xử lý `null`, compiler sẽ chặn lại.

Đây là lý do Kotlin giúp giảm rất nhiều lỗi runtime kiểu `NullPointerException`.

## 9. Safe Calls & the Elvis Operator

Safe call dùng `?.`

```kotlin
val length = nickname?.length
```

Nếu `nickname` là `null`, biểu thức trả về `null` thay vì crash.

Elvis operator dùng `?:`

```kotlin
val length = nickname?.length ?: 0
```

Ý nghĩa là:

- nếu bên trái khác `null`, lấy giá trị đó
- nếu bên trái là `null`, lấy giá trị mặc định bên phải

Đây là một cặp công cụ bạn sẽ dùng rất nhiều trong Android, từ form input, network response, cho tới state rendering.

## 10. Non-Null Assertions

Toán tử `!!` ép Kotlin tin rằng giá trị không phải `null`.

```kotlin
val length = nickname!!.length
```

Nếu `nickname` thực sự là `null`, chương trình sẽ crash ngay.

Vì vậy:

- `!!` không bị cấm tuyệt đối
- nhưng nên tránh dùng nếu còn lựa chọn tốt hơn như `?.`, `?:`, kiểm tra `if`, hoặc thiết kế lại API

Người mới thường lạm dụng `!!` để “cho code compile được”. Đây là thói quen rất nên tránh.

## 11. Extensions for Nullable Types

Bạn có thể viết extension function cho kiểu nullable.

```kotlin
fun String?.isNullOrShort(): Boolean {
    return this == null || this.length < 3
}
```

Sử dụng:

```kotlin
val name: String? = null
println(name.isNullOrShort())
```

Đây là một kỹ thuật rất tiện để gom logic xử lý `null` lặp đi lặp lại vào một nơi duy nhất.

## 12. Introduction to Generics

Generic cho phép bạn viết code làm việc với nhiều kiểu dữ liệu mà vẫn an toàn về type.

```kotlin
class Box<T>(val value: T)

val intBox = Box(10)
val stringBox = Box("Kotlin")
```

Ở đây `T` là type parameter.

Generic rất quan trọng trong Kotlin và Android vì bạn sẽ gặp nó ở khắp nơi:

- `List<T>`
- `Map<K, V>`
- `Result<T>`
- repository, API wrapper, UI state wrapper

Người mới chưa cần đi sâu vào variance ngay ở bước này. Chỉ cần hiểu generic giúp code tái sử dụng mà vẫn giữ type safety.

## 13. Extension Properties

Ngoài extension function, Kotlin còn hỗ trợ extension property.

```kotlin
val String.firstChar: Char
    get() = this[0]
```

Sử dụng:

```kotlin
println("Android".firstChar)
```

Extension property không thể có backing field riêng. Nó chỉ là một cách viết getter hoặc setter tính toán dựa trên dữ liệu đã có.

## 14. break & continue

Trong vòng lặp:

- `break` dừng toàn bộ vòng lặp
- `continue` bỏ qua lần lặp hiện tại và sang lần tiếp theo

Ví dụ:

```kotlin
for (i in 1..5) {
    if (i == 3) continue
    println(i)
}
```

```kotlin
for (i in 1..5) {
    if (i == 3) break
    println(i)
}
```

Hãy dùng chúng khi thật sự làm cho logic rõ hơn. Nếu dùng quá nhiều trong các vòng lặp lồng nhau, code sẽ khó đọc.

## Kết lại Section III

Sau section này, bạn nên chắc các điểm sau:

- Extension function và extension property giúp code gọn hơn
- Named arguments và default arguments làm lời gọi hàm dễ đọc hơn
- `when` là một công cụ rất mạnh trong Kotlin
- Data class là lựa chọn gần như mặc định cho model dữ liệu đơn giản
- Null-safety là một trụ cột rất quan trọng của Kotlin
- Generic là nền móng của các collection và nhiều API hiện đại

Gợi ý luyện tập:

- Viết extension function cho `String` để in hoa ký tự đầu
- Viết data class `UiState` với vài trường cơ bản
- Viết hàm dùng `when` để mô tả trạng thái đơn hàng
- Viết hàm nhận `String?` và trả về text fallback an toàn bằng Elvis operator