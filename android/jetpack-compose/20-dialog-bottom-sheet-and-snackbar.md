# 20. Dialog, bottom sheet và snackbar

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- khi nào dùng dialog, bottom sheet và snackbar
- cách nghĩ về state điều khiển các thành phần này
- vì sao chúng thường gắn với event và side effect
- cách tránh biến UI phản hồi thành mớ logic rối

## Ba loại phản hồi rất phổ biến

### Dialog

Phù hợp khi cần người dùng xác nhận hoặc chú ý tới một quyết định quan trọng.

Ví dụ:

- xác nhận xóa item
- cảnh báo mất dữ liệu chưa lưu

### Bottom sheet

Phù hợp khi cần hiển thị nhóm hành động hoặc nội dung phụ từ cạnh dưới màn hình.

Ví dụ:

- menu hành động của item
- form nhỏ hoặc picker

### Snackbar

Phù hợp cho thông báo ngắn, không chặn luồng chính.

Ví dụ:

- đã lưu thành công
- đã xóa item
- lỗi nhẹ có thể retry

## Điều khiển bằng state

Các thành phần này không nên xuất hiện ngẫu nhiên. Chúng nên được điều khiển bởi state hoặc event rõ ràng.

Ví dụ dialog xác nhận:

```kotlin
if (showDeleteDialog) {
    AlertDialog(
        onDismissRequest = { onDismiss() },
        confirmButton = { /* ... */ },
        dismissButton = { /* ... */ },
        title = { Text("Delete task?") },
        text = { Text("This action cannot be undone.") }
    )
}
```

Ở đây `showDeleteDialog` là state điều khiển việc hiển thị.

## Snackbar thường gắn với event

Snackbar thường là one-time feedback. Vì thế nó thường đi cùng event hoặc effect hơn là một state hiển thị lâu dài.

Ví dụ tư duy:

- user xóa task
- ViewModel phát tín hiệu phù hợp
- UI dùng effect để gọi snackbar host

Đây là chỗ bạn cần phân biệt:

- screen state liên tục
- one-time UI event

## Bottom sheet và state

Bottom sheet thường có state mở hoặc đóng. Điều quan trọng là phải biết ai sở hữu state đó:

- composable cục bộ
- screen
- ViewModel

Nếu sheet chỉ là UI tạm thời, state local thường đủ. Nếu sheet đại diện cho một flow nghiệp vụ rõ ràng, bạn cần cân nhắc state owner phù hợp hơn.

## Ví dụ tư duy xác nhận xóa task

1. user bấm nút xóa
2. `showDeleteDialog = true`
3. dialog hiện ra
4. user xác nhận
5. gửi event lên ViewModel
6. ViewModel xử lý xóa
7. UI đóng dialog và có thể show snackbar

Luồng này rõ hơn nhiều so với việc UI tự xử lý ngẫu nhiên từng bước mà không có nguyên tắc.

## Best practices

- Điều khiển dialog và sheet bằng state rõ ràng.
- Dùng snackbar cho thông báo ngắn, không chặn.
- Tách business action khỏi phần hiển thị popup hoặc feedback.
- Phân biệt UI state và one-time event.

## Điều cần tránh

- Dùng dialog cho mọi loại thông báo nhỏ.
- Show snackbar trực tiếp trong body composable mà không có effect phù hợp.
- Trộn logic xác nhận, xóa dữ liệu và render popup vào cùng một khối khó đọc.

## Checklist tự kiểm tra

1. Bạn có phân biệt được dialog, bottom sheet và snackbar không?
2. Bạn có biết loại nào thường gắn với state, loại nào thường gắn với event không?
3. Bạn có thể mô tả luồng xóa item có dialog xác nhận và snackbar phản hồi không?

## Bài tiếp theo

Sau UI phản hồi, bạn sẽ học animation cơ bản để làm app mượt và rõ phản hồi hơn, nhưng không rối.
