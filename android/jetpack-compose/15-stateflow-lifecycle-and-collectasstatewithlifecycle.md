# 15. StateFlow, lifecycle và collectAsStateWithLifecycle

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- vì sao `StateFlow` rất hợp với Compose + ViewModel
- Compose lấy dữ liệu từ `StateFlow` như thế nào
- vì sao cần `collectAsStateWithLifecycle()`
- cách tránh những sai lầm phổ biến khi collect state

## Từ ViewModel sang UI bằng gì?

Một cách rất phổ biến trong Android hiện đại là:

- ViewModel giữ `MutableStateFlow`
- expose ra ngoài bằng `StateFlow`
- Compose collect `StateFlow` để render UI

Ví dụ ý tưởng:

```kotlin
class TaskViewModel : ViewModel() {
    private val _uiState = MutableStateFlow(TaskUiState())
    val uiState: StateFlow<TaskUiState> = _uiState
}
```

## Vì sao `StateFlow` phù hợp?

`StateFlow` phù hợp vì:

- luôn có giá trị hiện tại
- tốt cho việc biểu diễn screen state
- dễ kết hợp với coroutine và ViewModel
- khi giá trị đổi, UI có thể recompose theo state mới

## Compose collect state ra sao?

Compose thường dùng:

```kotlin
val uiState by viewModel.uiState.collectAsStateWithLifecycle()
```

Sau đó render:

```kotlin
TaskScreenContent(uiState = uiState)
```

## Vì sao không chỉ dùng `collectAsState()`?

`collectAsStateWithLifecycle()` quan trọng vì nó nhận biết lifecycle tốt hơn cho Android screen. Nó giúp việc collect dữ liệu an toàn và phù hợp hơn với trạng thái hiển thị thực tế của màn hình.

Trong app Android thật, đây thường là lựa chọn tốt hơn cho UI layer.

## Luồng dữ liệu hoàn chỉnh

Một flow rất điển hình:

1. repository trả dữ liệu
2. ViewModel xử lý và cập nhật `_uiState`
3. `uiState` phát giá trị mới
4. Compose collect giá trị đó
5. UI recompose

Đây chính là UDF hoạt động trong runtime.

## Ví dụ cấu trúc màn hình

```kotlin
@Composable
fun TaskRoute(viewModel: TaskViewModel) {
    val uiState by viewModel.uiState.collectAsStateWithLifecycle()

    TaskScreen(
        uiState = uiState,
        onRefresh = viewModel::refresh
    )
}
```

`TaskRoute()` thường là lớp nối giữa ViewModel và UI stateless.

## Tại sao nên tách `Route` và `Screen`?

Một pattern rất tốt là:

- `Route`: lấy state từ ViewModel, xử lý navigation callback, nối dependencies
- `Screen`: composable thuần nhận `uiState` và callback

Ví dụ:

- `TaskRoute(viewModel: TaskViewModel)`
- `TaskScreen(uiState: TaskUiState, onRefresh: () -> Unit)`

Cách này giúp preview, test và tái sử dụng dễ hơn.

## Sai lầm phổ biến

### 1. Expose `MutableStateFlow` trực tiếp

Không nên. UI chỉ nên nhìn thấy `StateFlow` read-only.

### 2. UI tự launch coroutine để collect business state lung tung

Screen thường nên lấy state theo pattern chính thống thay vì tự thu gom nhiều nơi thiếu kiểm soát.

### 3. Không phân biệt `state` và `event`

`StateFlow` hợp với state liên tục. Với one-time event như snackbar hoặc navigation event, bạn cần cẩn thận hơn và sẽ học rõ hơn ở phần side effects.

## Best practices

- ViewModel giữ `_uiState` là `MutableStateFlow`, expose `uiState` là `StateFlow`.
- UI collect bằng `collectAsStateWithLifecycle()`.
- Tách `Route` và `Screen` nếu màn hình đủ lớn.
- Dùng `StateFlow` cho screen state ổn định.

## Điều cần tránh

- Expose mutable state ra ngoài UI layer.
- Trộn state dài hạn với event một lần theo cách khó kiểm soát.
- Để UI gọi collect thiếu lifecycle awareness.

## Checklist tự kiểm tra

1. Bạn có hiểu vì sao `StateFlow` hợp với screen state không?
2. Bạn có hiểu vai trò của `collectAsStateWithLifecycle()` không?
3. Bạn có thấy lợi ích của việc tách `Route` và `Screen` không?

## Bài tiếp theo

Sau khi màn hình đã có state đúng cách, bạn cần nối nhiều màn hình với nhau bằng Navigation Compose.
