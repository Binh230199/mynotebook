# 08. Text, Button, Image và các component cơ bản

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- cách dùng một số composable cơ bản nhất của Compose
- cách tùy biến `Text`, `Button`, `Image`
- cách kết hợp các component cơ bản để tạo UI nhỏ có ý nghĩa

## Vì sao bài này quan trọng?

Dù Compose có nhiều API lớn như Navigation, Scaffold, Side Effects, thì phần lớn UI bạn nhìn thấy vẫn được xây từ những mảnh nhỏ như:

- text
- button
- icon
- image
- spacer
- divider

Nắm chắc nhóm này sẽ giúp bạn xây phần lớn component mức cơ bản đến trung bình.

## `Text`

`Text` là composable hiển thị văn bản.

```kotlin
Text(text = "Hello Compose")
```

Bạn có thể tùy biến kiểu hiển thị:

```kotlin
Text(
    text = "Task title",
    style = MaterialTheme.typography.titleMedium,
    color = MaterialTheme.colorScheme.primary
)
```

## `Button`

`Button` là component tương tác rất cơ bản.

```kotlin
Button(onClick = { }) {
    Text("Save")
}
```

Bạn có thể điều khiển trạng thái:

```kotlin
Button(
    onClick = { },
    enabled = false
) {
    Text("Submit")
}
```

## `Image`

`Image` hiển thị hình ảnh hoặc painter.

```kotlin
Image(
    painter = painterResource(id = R.drawable.ic_launcher_foreground),
    contentDescription = "App icon"
)
```

Điểm quan trọng là `contentDescription` nên được cung cấp phù hợp cho accessibility, trừ khi hình chỉ mang tính trang trí.

## `Icon`

Nếu bạn dùng Material Icons hoặc icon dạng vector:

```kotlin
Icon(
    imageVector = Icons.Default.Add,
    contentDescription = "Add item"
)
```

## `Spacer`

`Spacer` giúp tạo khoảng trống có chủ đích.

```kotlin
Spacer(modifier = Modifier.height(8.dp))
```

Đây là cách Compose khuyến khích spacing rõ ràng thay vì nhồi layout khó đoán.

## `Divider` hoặc tương đương

Divider dùng để tách các vùng nội dung.

```kotlin
HorizontalDivider()
```

Tên API cụ thể có thể thay đổi theo version Material, nhưng ý tưởng luôn giống nhau: tách phần UI nhẹ nhàng và rõ ràng.

## Ví dụ hoàn chỉnh

```kotlin
@Composable
fun TaskSummaryCard() {
    Column(modifier = Modifier.padding(16.dp)) {
        Text(
            text = "Buy groceries",
            style = MaterialTheme.typography.titleMedium
        )
        Spacer(modifier = Modifier.height(8.dp))
        Text(
            text = "Need milk, eggs and fruit",
            style = MaterialTheme.typography.bodyMedium
        )
        Spacer(modifier = Modifier.height(12.dp))
        Button(onClick = { }) {
            Text("Mark done")
        }
    }
}
```

Ví dụ này tuy nhỏ nhưng đã có cấu trúc UI thực tế:

- tiêu đề
- mô tả
- spacing
- action chính

## Best practices

- Dùng typography và color từ `MaterialTheme` khi có thể.
- Tạo spacing rõ ràng bằng `Spacer` hoặc arrangement.
- Đặt `contentDescription` hợp lý cho image hoặc icon có ý nghĩa.
- Tách custom component khi một nhóm UI nhỏ bắt đầu lặp lại.

## Điều cần tránh

- Hardcode style lung tung ở nhiều nơi nếu đã có theme.
- Quên accessibility description cho image hoặc icon quan trọng.
- Dùng quá nhiều text style ngẫu nhiên làm UI mất nhất quán.

## Checklist tự kiểm tra

1. Bạn có dùng được `Text`, `Button`, `Image`, `Icon` không?
2. Bạn có hiểu vì sao spacing nên được kiểm soát rõ ràng không?
3. Bạn có thể ghép các component cơ bản thành một card nhỏ có ý nghĩa không?

## Bài tiếp theo

Sau khi có các component cơ bản, bạn sẽ đi vào input với `TextField` và cách quản lý form state trong Compose.
