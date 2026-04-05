# Section IV - Functional Programming

Section này là bước chuyển rất quan trọng từ Kotlin cơ bản sang Kotlin hiện đại. Khi đi vào Android thật, đặc biệt với Compose, Flow, collection processing và state transformation, bạn sẽ thấy tư duy hàm xuất hiện ở khắp nơi.

Mục tiêu của section này là giúp bạn:

- Hiểu lambdas là gì và vì sao chúng quan trọng
- Làm việc tốt với collection operations
- Hiểu higher-order functions và member references
- Phân biệt collection eager với sequence lazy
- Dùng được local functions, `fold` và recursion

## 1. Lambdas

Lambda là một hàm ngắn có thể truyền đi như giá trị.

Ví dụ:

```kotlin
val square = { x: Int -> x * x }
println(square(4))
```

Lambda thường được dùng khi bạn cần truyền một hành vi vào cho hàm khác.

Ví dụ với list:

```kotlin
val numbers = listOf(1, 2, 3)
val doubled = numbers.map { it * 2 }
println(doubled)
```

Ở đây `{ it * 2 }` là một lambda.

## 2. The Importance of Lambdas

Vì sao lambdas lại quan trọng đến vậy?

Vì chúng giúp bạn mô tả “cách xử lý dữ liệu” một cách ngắn gọn thay vì phải tạo nhiều class hoặc loop thủ công.

Trong Android, lambdas xuất hiện rất nhiều ở:

- click listener
- collection transformation
- callback
- event handler trong Compose
- state reducer

Ví dụ quen thuộc:

```kotlin
val positiveNumbers = numbers.filter { it > 0 }
```

Phong cách này vừa ngắn hơn vừa thường dễ đọc hơn loop thủ công nếu bạn đã quen.

## 3. Operations on Collections

Kotlin có rất nhiều hàm mạnh để xử lý collection.

Một vài hàm hay dùng:

- `map`
- `filter`
- `count`
- `any`
- `all`
- `find`
- `firstOrNull`

Ví dụ:

```kotlin
val numbers = listOf(1, 2, 3, 4, 5)

val evenNumbers = numbers.filter { it % 2 == 0 }
val labels = numbers.map { "No.$it" }
val hasBigNumber = numbers.any { it > 4 }
```

Khi học phần này, hãy tập chuyển những vòng lặp đơn giản sang collection operations để cảm nhận rõ sự khác biệt.

## 4. Member References

Member reference là cách tham chiếu tới hàm hoặc property có sẵn thay vì viết lambda đầy đủ.

Ví dụ:

```kotlin
data class User(val name: String)

val users = listOf(User("Lan"), User("Mai"))
val names = users.map(User::name)
```

Hoặc:

```kotlin
val numbers = listOf(1, 2, 3)
numbers.forEach(::println)
```

Member reference làm code ngắn hơn khi lambda chỉ đơn giản gọi đúng một hàm hoặc lấy đúng một property.

## 5. Higher-Order Functions

Higher-order function là hàm nhận hàm khác làm tham số hoặc trả về một hàm.

Ví dụ:

```kotlin
fun applyOperation(x: Int, operation: (Int) -> Int): Int {
    return operation(x)
}

val result = applyOperation(10) { it * 3 }
println(result)
```

Đây là nền tảng cho rất nhiều API Kotlin hiện đại. Khi bạn dùng `map`, `filter`, `onEach`, `run`, `let`, `apply`, bạn đang làm việc với higher-order functions.

## 6. Manipulating Lists

Ngoài `map` và `filter`, Kotlin còn có nhiều cách biến đổi list rất hữu ích:

- `sortedBy`
- `distinct`
- `reversed`
- `chunked`
- `zip`
- `partition`

Ví dụ:

```kotlin
val names = listOf("Mai", "Lan", "Lan", "Binh")

println(names.distinct())
println(names.sortedBy { it.length })
println(names.partition { it.startsWith("L") })
```

Những thao tác này xuất hiện rất nhiều khi xử lý dữ liệu UI hoặc API response trước khi render lên màn hình.

## 7. Building Maps

Kotlin có nhiều helper để xây dựng map từ list.

Ví dụ:

```kotlin
data class User(val id: Int, val name: String)

val users = listOf(User(1, "Lan"), User(2, "Binh"))

val byId = users.associateBy { it.id }
val nameLengths = users.associate { it.name to it.name.length }
```

Ngoài ra, `groupBy` cũng rất mạnh:

```kotlin
val grouped = users.groupBy { it.name.first() }
```

Khi bạn cần tra cứu nhanh dữ liệu theo ID hoặc gom nhóm kết quả, các thao tác này rất tiện.

## 8. Sequences

Collection operations mặc định thường xử lý eager, nghĩa là chạy ngay từng bước trên toàn bộ collection trung gian.

Sequence thì xử lý lazy hơn, tức là hoãn lại và kết hợp pipeline hiệu quả hơn cho dữ liệu lớn hoặc chuỗi biến đổi dài.

```kotlin
val result = listOf(1, 2, 3, 4, 5)
    .asSequence()
    .map { it * 2 }
    .filter { it > 5 }
    .toList()
```

Với dữ liệu nhỏ, list thường đủ tốt và dễ đọc hơn. Với pipeline dài hoặc dữ liệu lớn, sequence có thể giúp tiết kiệm chi phí xử lý trung gian.

## 9. Local Functions

Kotlin cho phép khai báo hàm bên trong hàm khác.

```kotlin
fun formatUser(name: String, age: Int): String {
    fun normalize(text: String): String = text.trim().replaceFirstChar { it.uppercase() }

    return "${normalize(name)} - $age"
}
```

Local function hữu ích khi:

- logic hỗ trợ chỉ dùng trong một hàm duy nhất
- bạn muốn tránh lộ helper ra phạm vi rộng hơn cần thiết

Đây là công cụ nhỏ nhưng rất tiện để giữ code gọn.

## 10. Folding Lists

`fold` dùng để gom nhiều phần tử thành một kết quả.

```kotlin
val numbers = listOf(1, 2, 3, 4)

val sum = numbers.fold(0) { acc, item -> acc + item }
println(sum)
```

Ở đây:

- `0` là giá trị khởi đầu
- `acc` là accumulator
- `item` là phần tử hiện tại

`fold` rất mạnh khi bạn muốn biến list thành:

- tổng
- chuỗi kết hợp
- map tích lũy
- object kết quả tổng hợp

## 11. Recursion

Recursion là khi hàm tự gọi lại chính nó.

Ví dụ:

```kotlin
fun factorial(n: Int): Int {
    return if (n <= 1) 1 else n * factorial(n - 1)
}
```

Recursion rất đẹp trong một số bài toán như cây, cấu trúc lồng nhau, hoặc thuật toán chia nhỏ vấn đề. Tuy nhiên, không phải lúc nào recursion cũng là lựa chọn tốt nhất.

Người mới nên nhớ:

- recursion cần có điều kiện dừng rõ ràng
- với các bài toán lặp đơn giản, loop thường dễ hiểu hơn

## Kết lại Section IV

Sau section này, bạn nên chắc những ý sau:

- Lambda là công cụ cực kỳ quan trọng của Kotlin hiện đại
- Collection operations giúp code xử lý dữ liệu ngắn và rõ hơn
- Higher-order functions là nền tảng của rất nhiều API Kotlin
- Sequence phù hợp khi bạn cần pipeline lazy
- `fold` giúp gom dữ liệu rất mạnh
- Functional style sẽ xuất hiện liên tục trong Android, đặc biệt với Compose và state transformation

Gợi ý luyện tập:

- Dùng `map` để biến danh sách số thành chuỗi
- Dùng `filter` để lấy các số chẵn
- Dùng `associateBy` để tạo map từ danh sách user
- Dùng `fold` để tính tổng và trung bình
- Viết một hàm recursion đơn giản như factorial hoặc countdown