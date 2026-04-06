# 19. rememberSaveable và khôi phục state

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- vì sao `remember` là chưa đủ trong mọi trường hợp
- `rememberSaveable` giải quyết vấn đề gì
- local UI state nào nên được khôi phục
- ranh giới giữa UI restoration và state thật của màn hình

## Vấn đề

`remember` chỉ giữ state trong composition hiện tại. Khi activity bị recreate vì đổi cấu hình hoặc một số tình huống lifecycle khác, state này có thể mất.

Ví dụ:

- text người dùng đang nhập mất đi
- tab đang chọn bị reset
- trạng thái UI nhỏ quay về mặc định

## `rememberSaveable`

`rememberSaveable` được dùng khi bạn muốn local UI state có khả năng được lưu và khôi phục qua những thay đổi phù hợp.

Ví dụ:

```kotlin
var query by rememberSaveable { mutableStateOf("") }
```

So với `remember`, state này có khả năng sống tốt hơn qua một số thay đổi cấu hình.

## Khi nào nên dùng?

Thường phù hợp cho local UI state như:

- text đang nhập
- tab đang chọn
- trạng thái filter đơn giản
- trạng thái expand đơn giản nếu cần khôi phục

## Khi nào không nên lạm dụng?

Nếu đó là screen state hoặc business state thật sự, nơi phù hợp thường vẫn là ViewModel.

Ví dụ:

- danh sách task tải từ repository
- trạng thái loading hoặc error của màn hình
- state nghiệp vụ cần đồng bộ nhiều phần

Những thứ đó không nên bị nhầm với local saveable UI state.

## Tư duy đúng

- `remember`: state cục bộ trong composition hiện tại
- `rememberSaveable`: state cục bộ nhưng cần khôi phục tốt hơn
- `ViewModel`: screen state và business state

Nếu phân tầng đúng, app của bạn sẽ rõ ràng hơn rất nhiều.

## Ví dụ màn hình search nhỏ

```kotlin
@Composable
fun SearchBar() {
    var query by rememberSaveable { mutableStateOf("") }

    TextField(
        value = query,
        onValueChange = { query = it }
    )
}
```

Ở đây `query` là UI state cục bộ, `rememberSaveable` thường là lựa chọn hợp lý.

## Best practices

- Dùng `rememberSaveable` cho local UI state cần được giữ lại hợp lý.
- Không dùng nó để thay thế ViewModel.
- Phân biệt rõ state tạm của UI và state nghiệp vụ của màn hình.

## Điều cần tránh

- Coi `rememberSaveable` là nơi giữ mọi loại state.
- Trộn state restoration với business state management.
- Không có nguyên tắc rõ ràng về nơi đặt state.

## Checklist tự kiểm tra

1. Bạn có hiểu `remember` và `rememberSaveable` khác nhau ở đâu không?
2. Bạn có biết local UI state nào nên saveable không?
3. Bạn có biết vì sao screen state vẫn thường thuộc về ViewModel không?

## Bài tiếp theo

Sau state restoration, bạn sẽ học các thành phần phản hồi tương tác rất phổ biến ở app thật: dialog, bottom sheet và snackbar.
