# 04. State cơ bản với remember và mutableStateOf

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- state trong Compose là gì
- vì sao UI không thể chỉ dựa vào biến local thông thường
- `mutableStateOf` dùng để làm gì
- `remember` giải quyết vấn đề gì
- cách viết một composable có state cơ bản đúng cách

## State trong Compose là gì?

State là dữ liệu mà UI phụ thuộc vào để hiển thị.

Ví dụ:

- số lượng item trong giỏ hàng
- nội dung nhập trong `TextField`
- trạng thái loading
- trạng thái bật hoặc tắt của switch

Trong Compose, khi state thay đổi, UI có thể được recomposition để phản ánh giá trị mới.

## Vì sao biến local bình thường không đủ?

Ví dụ sai kiểu nhập môn:

```kotlin
@Composable
fun CounterWrong() {
    var count = 0

    Column {
        Text(text = "Count: $count")
        Button(onClick = { count++ }) {
            Text("Increase")
        }
    }
}
```

Đoạn này trông có vẻ đúng, nhưng thực tế `count` chỉ là biến local thông thường. Compose không theo dõi nó như một state thực thụ.

## `mutableStateOf`

`mutableStateOf` tạo ra một state có thể quan sát được bởi Compose.

```kotlin
val countState = mutableStateOf(0)
```

Bạn có thể đọc và ghi giá trị qua `.value`:

```kotlin
countState.value = countState.value + 1
```

Khi giá trị này đổi, Compose biết rằng UI nào đang đọc nó có thể cần render lại.

## `remember`

`remember` giúp giữ state qua các lần recomposition của composable.

Ví dụ đúng hơn:

```kotlin
@Composable
fun Counter() {
    val count = remember { mutableStateOf(0) }

    Column {
        Text(text = "Count: ${count.value}")
        Button(onClick = { count.value++ }) {
            Text("Increase")
        }
    }
}
```

Nếu không có `remember`, state có thể bị tạo lại mỗi lần composable chạy lại.

## Cú pháp gọn với `by`

Bạn có thể dùng delegated property để code gọn hơn:

```kotlin
@Composable
fun Counter() {
    var count by remember { mutableStateOf(0) }

    Column {
        Text(text = "Count: $count")
        Button(onClick = { count++ }) {
            Text("Increase")
        }
    }
}
```

Đây là cách viết rất phổ biến trong Compose.

## Ví dụ hoàn chỉnh

```kotlin
@Composable
fun CounterScreen() {
    var count by remember { mutableStateOf(0) }

    Column {
        Text(text = "Current count: $count")
        Button(onClick = { count++ }) {
            Text("Increase")
        }
        Button(onClick = { count = 0 }) {
            Text("Reset")
        }
    }
}
```

## Giải thích ví dụ

- `count` là state của composable
- `remember` giữ state qua recomposition
- `mutableStateOf(0)` cho Compose biết đây là state quan sát được
- khi click button, `count` đổi
- khi `count` đổi, phần UI đọc `count` có thể recompose

## Khi nào dùng state local trong composable?

State local phù hợp khi dữ liệu:

- chỉ liên quan tới một composable nhỏ
- không cần chia sẻ với nhiều nơi khác
- không cần sống sót qua cấu hình phức tạp hơn
- không phải business state của màn hình

Ví dụ phù hợp:

- mở hoặc đóng một dropdown nhỏ
- giá trị tab đang chọn trong một component cục bộ
- trạng thái mở rộng hoặc thu gọn của một card

## Khi nào không nên dùng state local?

Không nên giữ state cốt lõi của màn hình ở composable nếu dữ liệu đó:

- cần tồn tại qua nhiều composable
- cần business logic xử lý
- cần gọi API hoặc repository
- nên nằm trong ViewModel theo MVVM

Đây là cầu nối rất quan trọng giữa Compose và kiến trúc app.

## Best practices

- Dùng `remember` cho state UI local.
- Dùng `by` nếu muốn code gọn và dễ đọc.
- Giữ state local càng gần nơi sử dụng càng tốt, nhưng không giữ quá sâu nếu state cần chia sẻ.
- Bắt đầu phân biệt local UI state với screen state từ rất sớm.

## Điều cần tránh

- Dùng biến local thường cho state mong UI tự cập nhật.
- Nhét mọi loại state vào composable dù nó thuộc về ViewModel.
- Dùng state local cho business logic quan trọng.
- Hiểu `remember` như một cách lưu dữ liệu lâu dài xuyên mọi tình huống; nó không phải công cụ đó.

## Checklist tự kiểm tra

1. Bạn có hiểu vì sao biến local thông thường không đủ chưa?
2. Bạn có biết `mutableStateOf` dùng để làm gì không?
3. Bạn có hiểu vai trò của `remember` không?
4. Bạn có biết khi nào nên để state trong composable và khi nào nên đưa sang ViewModel không?

## Bài tiếp theo

Sau khi có state cơ bản, bạn cần hiểu recomposition hoạt động ra sao để tránh viết Compose theo cách gây lỗi hoặc khó kiểm soát.
