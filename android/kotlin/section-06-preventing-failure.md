# Section VI - Preventing Failure

Section này tập trung vào một năng lực rất quan trọng khi viết Kotlin thật: không chỉ làm cho code chạy được, mà còn làm cho code thất bại ít hơn, dễ đoán hơn, dễ debug hơn, và dễ test hơn.

Trong Android, đây là phần cực kỳ quan trọng vì ứng dụng chạy trên nhiều thiết bị, nhiều trạng thái mạng, nhiều điều kiện vòng đời khác nhau. Nếu không biết cách phòng ngừa lỗi, bạn sẽ gặp rất nhiều bug khó chịu dù logic nghiệp vụ không quá phức tạp.

Mục tiêu của section này là giúp bạn:

- Hiểu cách xử lý exception một cách rõ ràng
- Biết dùng `require`, `check`, `error` và các check instructions đúng chỗ
- Hiểu `Nothing` là gì và vì sao nó xuất hiện trong Kotlin
- Biết cách cleanup resource an toàn
- Có tư duy logging hợp lý
- Làm quen với unit testing như một công cụ phòng ngừa lỗi

## 1. Exception Handling

Exception handling là cách xử lý các tình huống bất thường trong lúc chương trình chạy.

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
    println("Input error: ${e.message}")
}
```

Kotlin không bắt buộc checked exception như Java. Điều này làm code gọn hơn, nhưng cũng đòi hỏi bạn phải chủ động hơn trong việc xác định lỗi nào nên được bắt, lỗi nào nên để lan ra ngoài.

Nguyên tắc thực dụng:

- Đừng dùng exception cho luồng xử lý bình thường
- Chỉ bắt exception ở nơi bạn thực sự biết cách xử lý hoặc chuyển nó thành thông tin hữu ích hơn
- Tránh `catch (e: Exception)` một cách vô tội vạ rồi nuốt lỗi

## 2. Check Instructions

Kotlin có một nhóm công cụ rất hữu ích để kiểm tra điều kiện và fail sớm khi dữ liệu không hợp lệ.

### `require`
Dùng khi muốn kiểm tra **tham số đầu vào** của hàm hoặc constructor.

```kotlin
fun registerUser(name: String) {
    require(name.isNotBlank()) { "name must not be blank" }
}
```

Nếu điều kiện sai, Kotlin ném `IllegalArgumentException`.

### `check`
Dùng khi muốn kiểm tra **trạng thái nội bộ** của chương trình.

```kotlin
fun process(items: List<String>) {
    check(items.isNotEmpty()) { "items must not be empty" }
}
```

Nếu điều kiện sai, Kotlin ném `IllegalStateException`.

### `error`
Dùng khi bạn muốn dừng ngay với một thông điệp lỗi rõ ràng.

```kotlin
fun unsupported(): Nothing {
    error("This operation is not supported")
}
```

Người mới nên tập dùng `require` và `check` đúng chỗ thay vì viết `if` rồi ném exception thủ công trong mọi trường hợp.

## 3. The Nothing Type

`Nothing` là một type đặc biệt trong Kotlin. Nó biểu thị rằng một hàm **không bao giờ trả về bình thường**.

Ví dụ:

```kotlin
fun fail(message: String): Nothing {
    throw IllegalStateException(message)
}
```

Hoặc:

```kotlin
fun todoFeature(): Nothing = error("Not implemented yet")
```

`Nothing` rất hữu ích về mặt type system vì nó cho compiler biết rằng nhánh code đó sẽ kết thúc bằng exception hoặc không quay lại luồng gọi bình thường.

Bạn sẽ không dùng `Nothing` mỗi ngày, nhưng hiểu nó sẽ giúp bạn đọc các API Kotlin và một số lỗi compiler dễ hơn.

## 4. Resource Cleanup

Khi làm việc với file, stream hoặc resource cần đóng, bạn phải chắc rằng resource được cleanup đúng cách ngay cả khi có lỗi xảy ra.

Trong Kotlin, cách phổ biến là dùng `use`:

```kotlin
import java.io.File

fun readFirstLine(path: String): String {
    return File(path).bufferedReader().use { reader ->
        reader.readLine()
    }
}
```

`use` đảm bảo resource được đóng sau khi block chạy xong, kể cả khi có exception.

Trong Android, tư duy này rất quan trọng khi làm việc với file, stream, cursor hoặc những tài nguyên cần giải phóng đúng lúc.

## 5. Logging

Logging không chỉ để “in cho vui”. Logging là công cụ giúp bạn hiểu ứng dụng đang làm gì trong runtime.

Ví dụ đơn giản:

```kotlin
fun fetchUser(id: Int) {
    println("Fetching user with id=$id")
}
```

Trong Android thực tế, bạn sẽ dùng hệ thống log phù hợp hơn thay vì `println()`, nhưng tư duy logging nên được học sớm.

Nguyên tắc logging tốt:

- Log những điểm quyết định của luồng xử lý
- Log dữ liệu đủ để debug, nhưng không log tràn lan
- Không log secret, token, password hoặc dữ liệu nhạy cảm
- Log phải có ý nghĩa, không phải chỉ là “vao day 1”, “vao day 2”

Ví dụ log tốt:

- bắt đầu request gì
- dữ liệu đầu vào đã được chuẩn hóa ra sao
- vì sao một nhánh xử lý bị dừng
- trạng thái cuối cùng là gì

## 6. Unit Testing

Unit test là một trong những công cụ mạnh nhất để ngăn lỗi quay trở lại.

Ví dụ:

```kotlin
import kotlin.test.Test
import kotlin.test.assertEquals

class PriceCalculatorTest {
    @Test
    fun discountShouldReducePrice() {
        val original = 100
        val discounted = original - 20

        assertEquals(80, discounted)
    }
}
```

Một số nguyên tắc rất nên tập từ sớm:

- Test một hành vi rõ ràng
- Tên test nên mô tả kỳ vọng
- Test nên độc lập, dễ đọc và chạy nhanh

Trong Android, bạn nên ưu tiên test logic Kotlin thuần trước. Đây là loại test dễ viết, dễ chạy và có giá trị rất cao.

## Kết lại Section VI

Sau section này, bạn nên chắc những ý sau:

- Exception cần được xử lý có chủ đích, không đoán mò
- `require`, `check`, `error` giúp fail sớm và fail rõ nghĩa
- `Nothing` là type dành cho các hàm không bao giờ trả về bình thường
- Resource cleanup phải được đảm bảo kể cả khi có lỗi
- Logging và unit testing là hai trụ cột rất quan trọng để phòng ngừa lỗi

Gợi ý luyện tập:

- Viết một hàm dùng `require` để kiểm tra input hợp lệ
- Viết một hàm dùng `check` để xác nhận state nội bộ
- Viết một hàm đọc file bằng `use`
- Viết 2 đến 3 unit test đơn giản cho các hàm Kotlin thuần bạn đã tạo trước đó