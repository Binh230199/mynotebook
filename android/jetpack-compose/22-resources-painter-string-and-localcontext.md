# 22. Resources, painter, stringResource và LocalContext

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- Compose làm việc với resource Android như thế nào
- khi nào dùng `stringResource()`
- `painterResource()` dùng để làm gì
- `LocalContext` là gì và nên dùng ra sao

## Compose vẫn chạy trong thế giới Android

Dù Compose có mô hình UI mới, app của bạn vẫn đang chạy trên Android platform. Bạn vẫn phải làm việc với:

- string resources
- drawable resources
- context
- resource system nói chung

## `stringResource()`

Thay vì hardcode text trực tiếp, bạn thường lấy chuỗi từ resources:

```kotlin
Text(text = stringResource(id = R.string.app_name))
```

Điều này giúp:

- hỗ trợ đa ngôn ngữ
- tách text khỏi code
- quản lý text tập trung hơn

## `painterResource()`

Khi cần hiển thị ảnh từ drawable resource, bạn thường dùng:

```kotlin
Image(
    painter = painterResource(id = R.drawable.ic_launcher_foreground),
    contentDescription = null
)
```

## `LocalContext`

`LocalContext.current` cho bạn `Context` hiện tại trong cây Compose.

Ví dụ:

```kotlin
val context = LocalContext.current
```

Nó hữu ích khi bạn cần gọi một số API Android framework yêu cầu `Context`.

## Nhưng đừng lạm dụng `Context`

Compose khuyến khích UI sạch và phụ thuộc vào state. Nếu bạn thấy composable nào cũng kéo `Context` để làm đủ thứ, rất có thể kiến trúc đang bắt đầu lệch.

Thường:

- UI chỉ dùng `Context` cho việc thật sự cần platform access
- business logic không nên nằm trong composable chỉ vì lấy được `Context`

## Ví dụ hợp lý

- lấy string resource
- lấy drawable resource
- gọi một API Android nhẹ cần `Context`

## Ví dụ không hợp lý

- UI dùng `Context` để tự truy cập data layer
- UI dùng `Context` để xử lý logic nghiệp vụ phức tạp
- mọi composable đều kéo `Context` vào mà không rõ lý do

## Best practices

- Dùng `stringResource()` cho text có trong resource.
- Dùng `painterResource()` cho drawable đơn giản.
- Dùng `LocalContext` khi thật sự cần Android `Context`.
- Giữ business logic ngoài composable.

## Điều cần tránh

- Hardcode chuỗi quá nhiều trong UI.
- Lạm dụng `LocalContext` như lối tắt cho mọi thứ.
- Trộn Android framework logic với UI rendering quá chặt.

## Checklist tự kiểm tra

1. Bạn có hiểu vì sao nên dùng `stringResource()` không?
2. Bạn có biết `painterResource()` dùng cho trường hợp nào không?
3. Bạn có hiểu `LocalContext` nên dùng có chừng mực không?

## Bài tiếp theo

Đến đây bạn đã biết dựng UI. Nhưng muốn làm việc chuyên nghiệp, bạn phải biết kiểm thử UI Compose.
