# 09. TextField, input và form state

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- `TextField` hoạt động như thế nào trong Compose
- vì sao input phải gắn chặt với state
- cách quản lý form state cơ bản
- những best practice quan trọng khi xây form theo MVVM

## Input trong Compose khác gì so với view system cũ?

Trong hệ thống view cũ, bạn thường đọc text từ `EditText` khi cần. Trong Compose, input thường được xem như một phần của state-driven UI.

Điều đó có nghĩa là giá trị field phải tới từ state và event thay đổi field cũng phải rõ ràng.

## Ví dụ `TextField` cơ bản

```kotlin
@Composable
fun NameInput() {
    var text by remember { mutableStateOf("") }

    TextField(
        value = text,
        onValueChange = { text = it },
        label = { Text("Name") }
    )
}
```

Ở đây:

- `value` là trạng thái hiện tại của field
- `onValueChange` là nơi cập nhật state khi người dùng gõ

Đây là pattern cực kỳ quan trọng của Compose.

## Vì sao `TextField` phải đi cùng state?

Nếu input không được xem như state, UI sẽ rất khó kiểm soát:

- validate khó hơn
- đồng bộ với ViewModel khó hơn
- reset form khó hơn
- render lỗi hoặc hiển thị sai dễ xảy ra hơn

Compose khuyến khích bạn xem form như một trạng thái của màn hình.

## Ví dụ form đơn giản

```kotlin
@Composable
fun LoginForm() {
    var email by remember { mutableStateOf("") }
    var password by remember { mutableStateOf("") }

    Column {
        TextField(
            value = email,
            onValueChange = { email = it },
            label = { Text("Email") }
        )
        TextField(
            value = password,
            onValueChange = { password = it },
            label = { Text("Password") }
        )
        Button(onClick = { }) {
            Text("Login")
        }
    }
}
```

## Form state trong MVVM nên ở đâu?

Nếu form nhỏ và cục bộ, local state có thể đủ. Nhưng với màn hình thật trong MVVM, form state thường nên nằm ở ViewModel.

Ví dụ ý tưởng:

```kotlin
data class LoginUiState(
    val email: String = "",
    val password: String = "",
    val isLoading: Boolean = false,
    val emailError: String? = null
)
```

Composable chỉ đọc state và gửi event như:

- email changed
- password changed
- submit clicked

Đây là cách viết form rất bền khi app lớn lên.

## Validation cơ bản

Bạn có thể validate ngay trong composable cho ví dụ nhỏ, nhưng với screen thật, validation nên nằm ở ViewModel hoặc domain layer tùy loại logic.

Ví dụ ý tưởng hiển thị lỗi:

```kotlin
TextField(
    value = email,
    onValueChange = { email = it },
    isError = email.isBlank(),
    label = { Text("Email") }
)
```

Nhưng khi logic validate phức tạp hơn, nên đẩy ra khỏi composable body.

## Best practices

- Luôn gắn input với state rõ ràng.
- Dùng unidirectional data flow cho form quan trọng.
- Tách UI state và validation logic khi màn hình bắt đầu phức tạp.
- Nghĩ về form như một phần của screen state.

## Điều cần tránh

- Để input sống tách rời state rồi chỉ đọc khi submit.
- Nhét quá nhiều validate logic rối vào composable.
- Trộn network call trực tiếp vào nút submit trong composable.

## Checklist tự kiểm tra

1. Bạn có hiểu `TextField(value, onValueChange)` là pattern gì không?
2. Bạn có biết vì sao form nên gắn với state không?
3. Bạn có biết khi nào local state đủ và khi nào nên chuyển sang ViewModel không?

## Bài tiếp theo

Sau input và component cơ bản, bạn sẽ học `Scaffold` để tổ chức cấu trúc màn hình Material một cách bài bản hơn.
