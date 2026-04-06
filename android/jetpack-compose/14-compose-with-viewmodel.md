# 14. Compose với ViewModel

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- vì sao Compose và ViewModel thường đi cùng nhau
- ViewModel nên giữ những loại state nào
- composable nên lấy dữ liệu từ ViewModel ra sao
- ranh giới trách nhiệm giữa ViewModel và UI

## Vì sao cần ViewModel khi đã có Compose state?

Compose có state local, nhưng app thật không chỉ có state cục bộ nhỏ. Bạn còn có:

- loading state
- error state
- dữ liệu từ repository
- event xử lý business logic
- state cần giữ ổn định theo màn hình

Đây là lúc ViewModel phát huy tác dụng.

ViewModel giúp:

- giữ screen state tập trung
- tách logic khỏi UI
- phối hợp với repository, use case
- sống tốt hơn qua thay đổi cấu hình

## Screen state nên nằm ở ViewModel

Ví dụ:

```kotlin
data class TaskUiState(
    val tasks: List<String> = emptyList(),
    val isLoading: Boolean = false,
    val errorMessage: String? = null
)
```

ViewModel thường là nơi tạo và cập nhật `TaskUiState`.

## UI nên làm gì?

Composable screen nên:

- đọc state
- hiển thị state
- phát event như click, retry, input changed

Composable không nên:

- gọi repository trực tiếp
- ôm business logic phức tạp
- tự giữ nhiều screen state phân tán nếu đã có ViewModel

## Ví dụ cấu trúc tư duy

```kotlin
class TaskViewModel : ViewModel() {
    // giữ ui state ở đây
}

@Composable
fun TaskScreen(viewModel: TaskViewModel) {
    // lấy state từ viewModel
    // render UI
    // gọi event về viewModel
}
```

## Event-based interaction

Một pattern rất phổ biến là UI gửi event lên ViewModel thay vì gọi từng hàm rời rạc lung tung.

Ví dụ:

```kotlin
sealed class TaskEvent {
    data object Refresh : TaskEvent()
    data class Delete(val id: Long) : TaskEvent()
}
```

UI có thể gọi:

```kotlin
onEvent(TaskEvent.Refresh)
```

Cách này giúp logic có cấu trúc hơn khi màn hình bắt đầu nhiều tương tác.

## Local UI state vẫn có chỗ đứng

Không phải mọi thứ đều phải đẩy sang ViewModel. Một số state UI rất cục bộ vẫn có thể local trong composable, ví dụ:

- expand hoặc collapse một section nhỏ
- selected tab cục bộ
- mở hoặc đóng menu tạm thời

Điểm quan trọng là phải phân biệt:

- local UI state
- screen state
- business state

## Best practices

- Để ViewModel làm state owner cho màn hình.
- Để composable tập trung vào render và event.
- Dùng `UiState` rõ ràng thay vì quá nhiều biến rời rạc.
- Giữ screen state immutable và cập nhật có kiểm soát.

## Điều cần tránh

- Composable gọi thẳng repository.
- ViewModel biết quá sâu chi tiết layout UI.
- Screen state bị phân tán nửa ở UI, nửa ở ViewModel mà không có nguyên tắc rõ ràng.

## Checklist tự kiểm tra

1. Bạn có hiểu vì sao ViewModel vẫn rất cần khi dùng Compose không?
2. Bạn có biết loại state nào nên ở ViewModel không?
3. Bạn có biết UI nên chỉ render state và phát event không?

## Bài tiếp theo

Sau khi hiểu Compose với ViewModel, bạn cần một cầu nối runtime rất quan trọng: `StateFlow` và `collectAsStateWithLifecycle()`.
