# 12. Danh sách với LazyColumn, Lazy layouts và quản lý item state

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- vì sao Compose dùng lazy layouts cho danh sách
- `LazyColumn` là gì
- khi nào nên quan tâm tới `key`
- cách suy nghĩ về state của item trong list

## Vì sao danh sách là chủ đề rất quan trọng?

Hầu hết app thực tế đều có danh sách:

- danh sách công việc
- danh sách tin nhắn
- danh sách sản phẩm
- danh sách cài đặt

Nếu không hiểu cách Compose xử lý list, bạn sẽ nhanh gặp lỗi UI state, hiệu năng hoặc item hiển thị sai khi dữ liệu thay đổi.

## `LazyColumn`

`LazyColumn` là container hiển thị danh sách dọc theo cơ chế lazy. Nghĩa là nó chỉ compose những item cần thiết quanh vùng hiển thị thay vì dựng toàn bộ danh sách ngay từ đầu.

Ví dụ:

```kotlin
@Composable
fun TaskList(tasks: List<String>) {
    LazyColumn {
        items(tasks) { task ->
            Text(text = task)
        }
    }
}
```

## `items()`

Hàm `items()` cho phép bạn duyệt list và render từng item.

Ví dụ với model:

```kotlin
data class Task(val id: Long, val title: String)

@Composable
fun TaskList(tasks: List<Task>) {
    LazyColumn {
        items(tasks) { task ->
            Text(text = task.title)
        }
    }
}
```

## Vì sao `key` quan trọng?

Khi danh sách có thể thay đổi thứ tự, thêm, xóa hoặc cập nhật item, `key` giúp Compose nhận diện item ổn định hơn.

```kotlin
LazyColumn {
    items(
        items = tasks,
        key = { it.id }
    ) { task ->
        Text(text = task.title)
    }
}
```

Nếu không có `key` phù hợp trong một số tình huống, UI state của item có thể bị nhảy sai khi danh sách đổi.

## State của item nên nằm ở đâu?

Đây là điểm rất quan trọng.

Nếu một item có state cục bộ nhỏ như mở rộng/thu gọn, local state có thể chấp nhận được. Nhưng nếu state đó liên quan business logic hoặc cần đồng bộ toàn màn hình, nó nên được quản lý ở screen state hoặc ViewModel.

Đừng tùy tiện giữ quá nhiều state quan trọng sâu bên trong từng row của list.

## Lazy layouts khác

Ngoài `LazyColumn`, bạn còn có thể gặp:

- `LazyRow`
- grid lazy theo version hoặc thư viện tương ứng

Tư duy nền tảng vẫn giống nhau: render danh sách theo hướng lazy và quản lý item identity cẩn thận.

## Ví dụ task list rõ nghĩa hơn

```kotlin
@Composable
fun TaskList(tasks: List<Task>, onTaskClick: (Task) -> Unit) {
    LazyColumn {
        items(tasks, key = { it.id }) { task ->
            Text(
                text = task.title,
                modifier = Modifier.clickable { onTaskClick(task) }
            )
        }
    }
}
```

Ở đây list chỉ render và phát event. Logic xử lý click không nằm trong item composable.

## Best practices

- Dùng `LazyColumn` cho list dọc thực tế.
- Cung cấp `key` ổn định nếu danh sách có khả năng thay đổi thứ tự hoặc nội dung.
- Giữ item composable đơn giản và stateless càng nhiều càng tốt.
- Để screen state và business decision ở ViewModel.

## Điều cần tránh

- Không nghĩ về `key` trong list động.
- Nhét logic quá nhiều vào item composable.
- Giữ state business phân mảnh trong từng item mà không có lý do.

## Checklist tự kiểm tra

1. Bạn có hiểu vì sao `LazyColumn` là lazy không?
2. Bạn có biết khi nào nên thêm `key` không?
3. Bạn có biết item state nào có thể local và item state nào nên đưa lên ViewModel không?

## Bài tiếp theo

Sau list, bạn cần học tư duy quan trọng nhất để Compose + MVVM chạy mượt: state hoisting và unidirectional data flow.
