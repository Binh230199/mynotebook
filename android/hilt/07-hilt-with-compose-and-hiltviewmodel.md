# 07. Hilt với Compose và hiltViewModel()

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- Hilt đi vào app Compose ở đâu
- vai trò của `hiltViewModel()`
- cách nối ViewModel đã được inject với Compose screen
- vì sao nên giữ composable UI tách khỏi DI chi tiết

## App Compose thường cần gì từ Hilt?

Trong kiến trúc Compose + MVVM, thứ thường cần được Hilt tạo và cung cấp nhất là ViewModel có dependency.

Ví dụ:

- `TaskViewModel` cần `TaskRepository`
- `TaskRepository` cần `TaskApi` và `TaskDao`

Hilt giúp chuỗi dependency đó được dựng đúng, còn Compose screen chỉ cần làm việc với ViewModel hoặc `UiState`.

## `hiltViewModel()` làm gì?

Bạn có thể hiểu `hiltViewModel()` là cầu nối tiện lợi để lấy ViewModel đã được Hilt quản lý trong context phù hợp của màn hình Compose.

Nó giúp Compose screen hoặc route lấy đúng ViewModel mà không phải tự tạo thủ công.

## Pattern nên dùng

Một pattern rất tốt là:

- route composable lấy ViewModel bằng `hiltViewModel()`
- route collect `uiState`
- route truyền state và callback xuống screen stateless

Ví dụ tư duy:

```kotlin
@Composable
fun TaskRoute() {
    val viewModel = hiltViewModel<TaskViewModel>()
    val uiState by viewModel.uiState.collectAsStateWithLifecycle()

    TaskScreen(
        uiState = uiState,
        onRefresh = viewModel::refresh
    )
}
```

## Vì sao không để mọi composable gọi `hiltViewModel()`?

Vì như vậy UI sẽ phụ thuộc quá trực tiếp vào DI framework và khó test hơn.

Nếu một component nhỏ cũng tự lấy ViewModel hoặc dependency từ Hilt, bạn sẽ nhanh mất sự tách bạch giữa:

- wiring layer
- pure UI layer

## Nơi Hilt nên xuất hiện

Hilt thường hợp lý ở:

- activity root
- route-level composable
- ViewModel creation
- tầng module cấu hình dependency

Hilt thường không nên xuất hiện sâu trong mọi composable nhỏ tái sử dụng.

## Best practices

- Dùng `hiltViewModel()` ở route hoặc màn hình cấp cao.
- Truyền `UiState` và callback xuống composable con.
- Giữ UI tái sử dụng ở dạng stateless càng nhiều càng tốt.
- Tách DI concern khỏi rendering concern.

## Điều cần tránh

- Composable nhỏ nào cũng tự lấy ViewModel từ Hilt.
- Để UI layer phụ thuộc trực tiếp quá sâu vào framework DI.
- Trộn DI wiring với layout rendering ở nhiều cấp khác nhau.

## Checklist tự kiểm tra

1. Bạn có hiểu `hiltViewModel()` dùng để làm gì không?
2. Bạn có thấy lợi ích của pattern `Route -> Screen` trong app Compose + Hilt không?
3. Bạn có hiểu vì sao không nên kéo Hilt vào mọi composable nhỏ không?

## Bài tiếp theo

Khi app đã dùng Hilt, bạn cũng cần biết cách test các lớp có dependency một cách có kiểm soát.
