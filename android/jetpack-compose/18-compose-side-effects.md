# 18. Side effects trong Compose

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- side effect là gì trong Compose
- vì sao composable không nên làm việc phụ một cách tùy tiện trong body
- vai trò của `LaunchedEffect`, `DisposableEffect`, `SideEffect`
- cách nghĩ đúng về effect trong UI declarative

## Side effect là gì?

Compose muốn UI được mô tả như một hàm của state. Nhưng trong app thật, có những việc không chỉ là render UI, ví dụ:

- gọi load dữ liệu khi vào màn hình
- show snackbar
- theo dõi lifecycle hoặc listener
- đồng bộ trạng thái nào đó với hệ ngoài

Những việc như vậy gọi chung là side effect.

## Vì sao không làm trực tiếp trong body composable?

Vì composable có thể recompose nhiều lần. Nếu bạn gọi side effect trực tiếp trong body, bạn có thể vô tình:

- gọi API nhiều lần
- chạy logic lặp lại ngoài ý muốn
- tạo bug khó đoán

## `LaunchedEffect`

`LaunchedEffect` thường dùng để chạy coroutine gắn với lifecycle của composable và phụ thuộc vào key.

Ví dụ ý tưởng:

```kotlin
LaunchedEffect(Unit) {
    viewModel.loadData()
}
```

Ý nghĩa thường gặp là: khi composable vào composition lần đầu, chạy đoạn này một lần theo key tương ứng.

## `DisposableEffect`

`DisposableEffect` phù hợp khi bạn cần đăng ký thứ gì đó và dọn dẹp khi composable rời composition.

Ví dụ tư duy:

- đăng ký listener
- hủy listener khi thoát màn hình

## `SideEffect`

`SideEffect` thường dùng khi cần đẩy trạng thái hiện tại của Compose ra một đối tượng bên ngoài sau mỗi composition thành công.

Đây là công cụ hẹp hơn và ít gặp hơn với người mới.

## Ví dụ thực tế thường gặp

### Load dữ liệu lần đầu

```kotlin
LaunchedEffect(Unit) {
    onLoadInitialData()
}
```

### Show snackbar khi có message

Thường bạn không show snackbar trực tiếp bằng if đơn giản trong body. Bạn sẽ dùng effect để phản ứng với state hoặc event phù hợp.

## Key của effect rất quan trọng

Ví dụ:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Nếu `userId` đổi, effect sẽ chạy lại. Đây là điểm bạn phải hiểu rõ để tránh load sai hoặc load lặp.

## Side effect và ViewModel

Không phải mọi effect đều phải nằm trong UI. Một số logic load có thể được kích hoạt từ `init` của ViewModel hoặc từ event rõ ràng. Một số effect thì đúng là trách nhiệm của UI layer.

Bạn cần phân biệt:

- business logic ở ViewModel
- UI-triggered effect ở Compose

## Best practices

- Không gọi side effect tùy tiện trong body composable.
- Dùng `LaunchedEffect` khi cần coroutine gắn với composition.
- Chọn key cẩn thận.
- Giữ effect rõ lý do tồn tại và giới hạn phạm vi của nó.

## Điều cần tránh

- Gọi API hoặc chạy logic nặng trực tiếp trong thân composable.
- Dùng `LaunchedEffect(Unit)` bừa bãi mà không hiểu vòng đời.
- Không suy nghĩ về key khiến effect chạy lặp hoặc không chạy khi cần.

## Checklist tự kiểm tra

1. Bạn có hiểu vì sao side effect không nên đặt trực tiếp trong body composable không?
2. Bạn có hiểu `LaunchedEffect` dùng cho trường hợp nào không?
3. Bạn có hiểu key ảnh hưởng thế nào tới effect không?

## Bài tiếp theo

Một phần quan trọng khác của trải nghiệm Android là giữ state khi cấu hình thay đổi hoặc process recreation xảy ra. Compose có công cụ chuyên cho chuyện này.
