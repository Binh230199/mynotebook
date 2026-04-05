# Section II - Introduction to Objects

Section này đưa bạn từ cú pháp cơ bản sang cách Kotlin tổ chức dữ liệu và hành vi bằng object, class, property và collections. Đây là bước rất quan trọng, vì hầu hết code Android thực tế đều xoay quanh object: model, state, repository, view model, request, response, cấu hình, và dữ liệu UI.

Mục tiêu của section này là giúp bạn:

- Hiểu vì sao “mọi thứ gần như đều là object” trong Kotlin
- Tạo class, property và constructor cơ bản
- Hiểu visibility và package
- Làm quen với exception và testing ở mức nhập môn
- Nắm collections nền tảng: list, set, map
- Viết được property accessor đơn giản

## 1. Objects Everywhere

Một trong những điểm khác với nhiều ngôn ngữ truyền thống là trong Kotlin, bạn làm việc với object gần như ở khắp nơi.

Ví dụ:

- `String` là object
- `List` là object
- `Map` là object
- Các instance của class bạn tự tạo cũng là object

Ngay cả khi bạn dùng những giá trị quen thuộc như chuỗi, danh sách hoặc model data, bạn đều đang làm việc với các object có thuộc tính và hành vi riêng.

Điều này quan trọng với Android vì bạn sẽ liên tục truyền object giữa các lớp: screen state, domain model, DTO, entity, config object, result object.

## 2. Creating Classes

Tạo class trong Kotlin khá gọn.

```kotlin
class Person
```

Đây là class đơn giản nhất. Bạn có thể tạo object từ class này:

```kotlin
val person = Person()
```

Thông thường, class có property và constructor ngay từ đầu:

```kotlin
class Person(val name: String, val age: Int)
```

Khi đó bạn tạo object như sau:

```kotlin
val person = Person("Binh", 28)
```

Kotlin giúp việc khai báo class gọn hơn nhiều so với kiểu viết đầy đủ constructor, field và getter trong Java.

## 3. Properties

Property là dữ liệu thuộc về object.

```kotlin
class User(
    val name: String,
    var isActive: Boolean
)
```

Ở đây:

- `name` là property chỉ đọc
- `isActive` là property có thể thay đổi

Sử dụng:

```kotlin
val user = User("Lan", true)
println(user.name)

user.isActive = false
println(user.isActive)
```

Người mới nên nhớ rằng Kotlin property không chỉ là “biến trong class”. Nó là một cơ chế ngôn ngữ rõ ràng, có thể đi kèm getter và setter.

## 4. Constructors

Kotlin có primary constructor rất gọn:

```kotlin
class Book(val title: String, val pages: Int)
```

Bạn cũng có thể thêm logic khởi tạo bằng `init`:

```kotlin
class Book(val title: String, val pages: Int) {
    init {
        require(pages > 0) { "pages must be positive" }
    }
}
```

`init` chạy khi object được tạo. Đây là nơi phù hợp để kiểm tra điều kiện khởi tạo hoặc chuẩn hóa dữ liệu đầu vào.

Trong Android, constructor thường được dùng nhiều ở model, wrapper object, state object, hoặc các lớp thuần Kotlin.

## 5. Constraining Visibility

Visibility control giúp bạn giới hạn nơi mà class, property hoặc function có thể được truy cập.

Các modifier phổ biến:

- `public`: mặc định, truy cập được ở mọi nơi phù hợp
- `private`: chỉ dùng trong cùng class hoặc file tùy ngữ cảnh
- `internal`: dùng trong cùng module
- `protected`: dùng trong class và subclass

Ví dụ:

```kotlin
class Account(private var balance: Int) {
    fun deposit(amount: Int) {
        balance += amount
    }

    fun currentBalance(): Int = balance
}
```

Ở đây `balance` không thể bị sửa trực tiếp từ bên ngoài. Đây là cách đóng gói logic tốt hơn và giúp giảm bug.

## 6. Packages

Package giúp bạn tổ chức code theo nhóm có ý nghĩa.

Ví dụ:

```kotlin
package com.example.kotlinbasics
```

Bạn có thể import symbol từ package khác:

```kotlin
import com.example.kotlinbasics.User
```

Trong Android, package structure rất quan trọng vì dự án lớn thường chia theo feature hoặc layer như:

- `data`
- `domain`
- `presentation`
- `ui`
- `network`
- `database`

Người mới nên tập giữ package rõ ràng ngay từ đầu, đừng để mọi class nằm cùng một chỗ.

## 7. Testing

Nhiều người mới học Kotlin thường nghĩ testing là phần “để sau”. Cách nghĩ đó không tốt.

Ngay cả khi mới học ngôn ngữ, test giúp bạn:

- kiểm tra hàm có hoạt động đúng không
- hiểu kỳ vọng đầu ra rõ hơn
- tự tin hơn khi sửa code

Ví dụ đơn giản với JUnit:

```kotlin
import kotlin.test.Test
import kotlin.test.assertEquals

class MathTest {
    @Test
    fun addTwoNumbers() {
        assertEquals(4, 2 + 2)
    }
}
```

Trong Android, unit test cho code Kotlin thuần thường là điểm bắt đầu dễ nhất trước khi sang UI test hoặc integration test.

## 8. Exceptions

Exception là cơ chế báo lỗi bất thường trong quá trình chạy.

Ví dụ:

```kotlin
fun divide(a: Int, b: Int): Int {
    if (b == 0) throw IllegalArgumentException("b must not be zero")
    return a / b
}
```

Bạn có thể bắt exception bằng `try/catch`:

```kotlin
try {
    println(divide(10, 0))
} catch (e: IllegalArgumentException) {
    println(e.message)
}
```

Section này chỉ giới thiệu mức cơ bản. Phần xử lý lỗi có hệ thống hơn sẽ được đào sâu ở Section VI.

## 9. Lists

List là collection có thứ tự.

```kotlin
val names = listOf("Lan", "Mai", "Binh")
println(names[0])
```

Nếu muốn thay đổi nội dung, dùng mutable list:

```kotlin
val scores = mutableListOf(10, 20)
scores.add(30)
```

Bạn sẽ dùng list cực kỳ nhiều trong Android:

- danh sách item trên màn hình
- dữ liệu trả về từ API
- tập hợp state hoặc filter

Nguyên tắc rất nên nhớ:

- mặc định dùng immutable list nếu đủ
- chỉ dùng mutable list khi thật sự cần chỉnh sửa

## 10. Variable Argument Lists

`vararg` cho phép hàm nhận nhiều đối số cùng kiểu.

```kotlin
fun printAll(vararg names: String) {
    for (name in names) {
        println(name)
    }
}
```

Gọi hàm:

```kotlin
printAll("Lan", "Mai", "Binh")
```

Nếu đã có sẵn mảng và muốn truyền vào `vararg`, bạn cần spread operator `*`:

```kotlin
val people = arrayOf("Lan", "Mai")
printAll(*people)
```

## 11. Sets

Set là collection không chứa phần tử trùng lặp.

```kotlin
val tags = setOf("kotlin", "android", "kotlin")
println(tags)
```

Kết quả sẽ chỉ còn giá trị duy nhất.

Set phù hợp khi bạn quan tâm tới tính duy nhất hơn là thứ tự, ví dụ:

- tập quyền hạn
- tập tag
- tập ID đã xử lý

## 12. Maps

Map lưu dữ liệu theo cặp key-value.

```kotlin
val userAges = mapOf(
    "Lan" to 20,
    "Binh" to 28
)

println(userAges["Lan"])
```

Map rất hữu ích khi bạn cần tra cứu nhanh theo khóa, ví dụ:

- ID -> object
- key config -> value
- screen route -> title

Mutable map cũng tồn tại:

```kotlin
val scores = mutableMapOf("Lan" to 10)
scores["Mai"] = 20
```

## 13. Property Accessors

Kotlin cho phép bạn tùy chỉnh getter và setter.

```kotlin
class Temperature {
    var celsius: Double = 0.0
        set(value) {
            field = if (value < -273.15) -273.15 else value
        }
}
```

Ở đây `field` là backing field của property.

Bạn cũng có thể viết getter tùy chỉnh:

```kotlin
class Rectangle(val width: Int, val height: Int) {
    val area: Int
        get() = width * height
}
```

Property accessor rất hữu ích khi bạn muốn:

- kiểm soát giá trị đầu vào
- tính toán dữ liệu từ property khác
- giữ logic gần với dữ liệu mà nó quản lý

## 14. Summary 2

Sau section này, bạn nên chắc những ý sau:

- Class là cách gom dữ liệu và hành vi lại với nhau
- Property là dữ liệu thuộc về object
- Constructor giúp tạo object với trạng thái ban đầu rõ ràng
- Visibility giúp ẩn bớt chi tiết nội bộ
- Package giúp tổ chức project có trật tự
- List, Set, Map là ba collection nền tảng mà bạn sẽ dùng liên tục
- Getter và setter tùy chỉnh giúp kiểm soát property tốt hơn

Checklist tự kiểm tra:

1. Bạn có tạo được một class với primary constructor không?
2. Bạn có phân biệt được `listOf()` và `mutableListOf()` không?
3. Bạn có biết khi nào dùng `Set` thay vì `List` không?
4. Bạn có hiểu vì sao nên dùng `private` cho dữ liệu nội bộ không?

Gợi ý luyện tập:

- Tạo class `Student` với `name`, `age`, `score`
- Tạo danh sách sinh viên và in ra tên từng người
- Viết một `Map` từ tên môn học sang điểm số
- Tạo một property có setter để chặn giá trị không hợp lệ