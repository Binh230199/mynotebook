# 03. Composable đầu tiên, Preview và tư duy declarative UI

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- `@Composable` là gì
- một composable function được viết ra sao
- `setContent {}` có vai trò gì
- Preview dùng để làm gì và khi nào nên dùng
- cách tách composable nhỏ để code dễ đọc hơn

## Composable là gì?

Composable là hàm Kotlin có annotation `@Composable`, dùng để mô tả UI.

Ví dụ tối giản:

```kotlin
@Composable
fun Greeting(name: String) {
    Text(text = "Hello, $name")
}
```

Điểm quan trọng là:

- đây vẫn là hàm Kotlin
- nhưng nó chạy trong ngữ cảnh Compose runtime
- nó không nên bị hiểu như một hàm “thường” hoàn toàn

Compose runtime theo dõi composable để biết khi nào cần render lại UI.

## `setContent {}` làm gì?

Trong `ComponentActivity`, `setContent {}` là điểm bạn gắn UI Compose vào activity.

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            Greeting(name = "Android")
        }
    }
}
```

Bạn có thể hiểu `setContent {}` như điểm entry cho UI Compose của màn hình.

## Ví dụ đầu tiên có cấu trúc hơn

```kotlin
@Composable
fun Greeting(name: String) {
    Column {
        Text(text = "Hello, $name")
        Text(text = "Welcome to Jetpack Compose")
    }
}
```

Ở đây `Greeting()` mô tả một cây UI nhỏ gồm `Column` chứa hai `Text`.

## Preview là gì?

Preview cho phép bạn xem composable ngay trong Android Studio mà không cần chạy full app.

Ví dụ:

```kotlin
@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    Greeting(name = "Preview")
}
```

Điều này rất hữu ích khi bạn:

- đang chỉnh layout
- muốn xem nhanh component trông ra sao
- muốn thử nhiều trạng thái UI nhỏ

## Cách dùng Preview đúng

1. Viết composable càng stateless càng tốt.
2. Tạo preview riêng cho composable đó.
3. Truyền dữ liệu mẫu rõ ràng.
4. Nếu component có nhiều trạng thái, tạo nhiều preview khác nhau.

Ví dụ:

```kotlin
@Preview(showBackground = true)
@Composable
fun GreetingShortNamePreview() {
    Greeting(name = "Lan")
}

@Preview(showBackground = true)
@Composable
fun GreetingLongNamePreview() {
    Greeting(name = "Nguyen Thi Minh Lan")
}
```

Đây là cách rất tốt để kiểm tra UI trong nhiều tình huống trước khi chạy app.

## Tư duy declarative trong composable đầu tiên

Một composable không nên nghĩ theo kiểu:

- lấy view
- set text
- đổi màu
- ẩn hiện bằng tay

Thay vào đó, bạn nên nghĩ:

- nếu state là thế này, UI sẽ hiển thị thế nào

Ví dụ:

```kotlin
@Composable
fun StatusLabel(isOnline: Boolean) {
    Text(
        text = if (isOnline) "Online" else "Offline"
    )
}
```

Bạn không ra lệnh đổi text trực tiếp. Bạn chỉ mô tả UI theo state đầu vào.

## Cách tách composable nhỏ

Một trong những lợi thế lớn của Compose là UI dễ tách thành các khối nhỏ.

Ví dụ:

```kotlin
@Composable
fun GreetingScreen(name: String) {
    Column {
        GreetingTitle(name)
        GreetingSubtitle()
    }
}

@Composable
fun GreetingTitle(name: String) {
    Text(text = "Hello, $name")
}

@Composable
fun GreetingSubtitle() {
    Text(text = "Welcome to Jetpack Compose")
}
```

Tách như vậy giúp:

- file dễ đọc hơn
- preview dễ hơn
- tái sử dụng tốt hơn
- test UI đơn vị dễ hơn về sau

## Best practices

- Viết composable nhỏ, có trách nhiệm rõ ràng.
- Đặt tên composable theo ý nghĩa UI.
- Tạo preview cho component quan trọng.
- Truyền dữ liệu vào composable thay vì hardcode nếu có thể.

## Điều cần tránh

- Viết một composable dài hàng trăm dòng ngay từ đầu.
- Gắn logic nghiệp vụ vào composable cơ bản.
- Chỉ có một preview cho mọi trường hợp UI.
- Coi preview là thứ tùy chọn không cần dùng.

## Checklist tự kiểm tra

1. Bạn có viết được composable đầu tiên chưa?
2. Bạn có hiểu vì sao `setContent {}` quan trọng không?
3. Bạn có thể tạo preview cho composable không?
4. Bạn có bắt đầu nghĩ UI theo state thay vì theo lệnh cập nhật view chưa?

## Bài tiếp theo

Sau composable đầu tiên, bước quan trọng nhất là hiểu state trong Compose, đặc biệt là `remember` và `mutableStateOf`.
