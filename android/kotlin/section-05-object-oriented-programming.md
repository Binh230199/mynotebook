# Section V - Object-Oriented Programming

Section này đi sâu vào cách Kotlin hỗ trợ lập trình hướng đối tượng. Dù Kotlin có rất nhiều tính năng hàm mạnh mẽ, OOP vẫn là phần rất quan trọng trong Android vì project thực tế thường được tổ chức bằng interface, class, abstraction, delegation và các object có trách nhiệm rõ ràng.

Mục tiêu của section này là giúp bạn:

- Hiểu interface, inheritance và polymorphism trong Kotlin
- Phân biệt khi nào nên dùng abstract class, interface hoặc composition
- Nắm các khái niệm như sealed class, type checking, delegation, companion object
- Đọc và thiết kế class trong dự án Android dễ dàng hơn

## 1. Interfaces

Interface mô tả một hợp đồng hành vi.

```kotlin
interface Clickable {
    fun click()
}
```

Class triển khai interface:

```kotlin
class Button : Clickable {
    override fun click() {
        println("Button clicked")
    }
}
```

Trong Android, interface thường được dùng để:

- định nghĩa contract cho repository
- tách abstraction khỏi implementation
- mô tả callback hoặc hành vi dùng chung

## 2. Complex Constructors

Khi khởi tạo object cần nhiều bước kiểm tra hoặc chuẩn hóa dữ liệu, constructor có thể phức tạp hơn.

```kotlin
class User(name: String, age: Int) {
    val name: String
    val age: Int

    init {
        require(name.isNotBlank()) { "name must not be blank" }
        require(age >= 0) { "age must be non-negative" }

        this.name = name.trim()
        this.age = age
    }
}
```

Nguyên tắc tốt là nếu constructor bắt đầu chứa quá nhiều logic, hãy cân nhắc tách phần chuẩn bị dữ liệu ra ngoài hoặc dùng factory function.

## 3. Secondary Constructors

Kotlin hỗ trợ secondary constructor, nhưng trong thực tế bạn sẽ dùng ít hơn so với Java.

```kotlin
class User(val name: String, val age: Int) {
    constructor(name: String) : this(name, 0)
}
```

Secondary constructor hữu ích khi bạn muốn cung cấp nhiều cách khởi tạo object. Tuy nhiên, default arguments hoặc factory functions thường là lựa chọn gọn hơn.

## 4. Inheritance

Khác với Java, class trong Kotlin mặc định là `final`. Nếu muốn cho kế thừa, bạn phải đánh dấu `open`.

```kotlin
open class Animal {
    open fun sound() = "Unknown"
}

class Dog : Animal() {
    override fun sound() = "Woof"
}
```

Điều này giúp giảm việc kế thừa bừa bãi. Kotlin khuyến khích bạn phải chủ động nói rõ class nào được phép mở rộng.

## 5. Base Class Initialization

Khi kế thừa, thứ tự khởi tạo rất quan trọng. Base class luôn được khởi tạo trước subclass.

```kotlin
open class Base(val name: String) {
    init {
        println("Base initialized: $name")
    }
}

class Derived(name: String) : Base(name) {
    init {
        println("Derived initialized: $name")
    }
}
```

Người mới nên cẩn thận khi base class gọi tới các member có thể bị override, vì thứ tự khởi tạo có thể gây hành vi bất ngờ.

## 6. Abstract Classes

Abstract class là class không thể tạo instance trực tiếp và thường chứa cả phần đã có logic lẫn phần bắt buộc subclass phải triển khai.

```kotlin
abstract class Shape {
    abstract fun area(): Double
}

class Circle(private val radius: Double) : Shape() {
    override fun area(): Double = Math.PI * radius * radius
}
```

Abstract class phù hợp khi bạn có một “họ class” chung với một phần logic dùng chung thật sự.

## 7. Upcasting

Upcasting là xem một object cụ thể như kiểu cha hoặc interface của nó.

```kotlin
val clickable: Clickable = Button()
clickable.click()
```

Upcasting rất phổ biến vì nó cho phép code phụ thuộc vào abstraction thay vì implementation cụ thể.

## 8. Polymorphism

Polymorphism nghĩa là cùng một lời gọi nhưng hành vi thực tế phụ thuộc vào object cụ thể.

```kotlin
fun makeSound(animal: Animal) {
    println(animal.sound())
}

makeSound(Dog())
```

Điều này giúp code mở rộng tốt hơn. Trong Android, repository, use case handler, renderer hoặc navigator abstraction đều thường dựa trên polymorphism.

## 9. Composition

Composition là xây dựng class bằng cách chứa object khác bên trong thay vì kế thừa.

```kotlin
class Engine {
    fun start() = println("Engine start")
}

class Car(private val engine: Engine) {
    fun drive() {
        engine.start()
        println("Car moving")
    }
}
```

Đây thường là lựa chọn an toàn và linh hoạt hơn inheritance. Quy tắc quen thuộc là: ưu tiên composition hơn inheritance khi có thể.

## 10. Inheritance & Extensions

Một điểm rất quan trọng: extension function không tham gia polymorphism như member function.

```kotlin
open class Shape
class Rectangle : Shape()

fun Shape.describe() = "Shape"
fun Rectangle.describe() = "Rectangle"

val item: Shape = Rectangle()
println(item.describe())
```

Kết quả vẫn là `Shape`, không phải `Rectangle`, vì extension được resolve theo kiểu tĩnh tại compile time.

Đây là một điểm người mới rất dễ hiểu nhầm.

## 11. Class Delegation

Kotlin hỗ trợ delegation rất tiện với từ khóa `by`.

```kotlin
interface Printer {
    fun print()
}

class ConsolePrinter : Printer {
    override fun print() {
        println("Printing...")
    }
}

class PrinterManager(private val printer: Printer) : Printer by printer
```

Delegation giúp tái sử dụng hành vi mà không cần kế thừa trực tiếp. Đây là một tính năng rất mạnh của Kotlin.

## 12. Downcasting

Downcasting là ép kiểu từ cha xuống con. Đây là thao tác cần cẩn trọng.

```kotlin
val animal: Animal = Dog()
val dog = animal as Dog
```

An toàn hơn là dùng `as?`:

```kotlin
val dog = animal as? Dog
```

Nếu ép kiểu sai, bạn có thể gặp crash. Vì vậy, chỉ nên downcast khi thật sự cần và đã hiểu rõ kiểu dữ liệu đang xử lý.

## 13. Sealed Classes

Sealed class rất phù hợp để mô tả một tập trạng thái đóng và rõ ràng.

```kotlin
sealed class UiState {
    data object Loading : UiState()
    data class Success(val message: String) : UiState()
    data class Error(val reason: String) : UiState()
}
```

Khi dùng với `when`, Kotlin có thể kiểm tra exhaustiveness tốt hơn.

```kotlin
fun render(state: UiState): String = when (state) {
    UiState.Loading -> "Loading"
    is UiState.Success -> state.message
    is UiState.Error -> state.reason
}
```

Trong Android, sealed class rất phù hợp cho UI state, navigation event, result type và action type.

## 14. Type Checking

Kotlin dùng `is` để kiểm tra kiểu dữ liệu.

```kotlin
fun printLength(value: Any) {
    if (value is String) {
        println(value.length)
    }
}
```

Kotlin hỗ trợ smart cast, nghĩa là sau khi kiểm tra `is String`, bạn có thể dùng `value` như `String` luôn trong phạm vi hợp lệ.

## 15. Nested Classes

Nested class là class nằm bên trong class khác nhưng không giữ tham chiếu tới outer class.

```kotlin
class Outer {
    class Nested {
        fun message() = "Nested"
    }
}
```

Sử dụng:

```kotlin
val nested = Outer.Nested()
println(nested.message())
```

Nested class phù hợp khi bạn muốn nhóm các loại class liên quan về mặt tổ chức.

## 16. Objects

Kotlin có `object` declaration để tạo singleton dễ dàng.

```kotlin
object AppLogger {
    fun log(message: String) {
        println(message)
    }
}
```

Sử dụng:

```kotlin
AppLogger.log("Started")
```

Ngoài ra còn có object expression để tạo anonymous object tại chỗ.

## 17. Inner Classes

Inner class khác nested class ở chỗ nó giữ tham chiếu tới outer class.

```kotlin
class Outer(private val name: String) {
    inner class Inner {
        fun greet() = "Hello from $name"
    }
}
```

Inner class hữu ích khi object bên trong thật sự cần truy cập dữ liệu của object bên ngoài.

## 18. Companion Objects

Companion object cho phép bạn định nghĩa thành phần giống “static” trong Kotlin.

```kotlin
class User private constructor(val name: String) {
    companion object {
        fun create(name: String): User {
            return User(name.trim())
        }
    }
}
```

Sử dụng:

```kotlin
val user = User.create(" Lan ")
```

Companion object thường được dùng cho:

- factory methods
- constant
- helper gắn với class

## Kết lại Section V

Sau section này, bạn nên chắc những ý sau:

- Interface giúp code phụ thuộc vào abstraction
- Inheritance có ích nhưng nên dùng có chủ đích
- Composition thường linh hoạt hơn inheritance
- Sealed class rất mạnh cho mô hình trạng thái đóng
- `is`, smart cast, `as?`, delegation và companion object là những công cụ cực kỳ thực dụng trong Kotlin

Gợi ý luyện tập:

- Viết interface `Repository` và một class triển khai nó
- Tạo sealed class cho trạng thái tải dữ liệu
- Viết ví dụ phân biệt nested class và inner class
- Tạo companion object cho một factory method đơn giản