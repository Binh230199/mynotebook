# 23. Testing UI Compose cơ bản

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- vì sao UI Compose vẫn cần test
- test Compose thường tập trung vào điều gì
- tư duy test theo hành vi thay vì theo implementation details
- cách chuẩn bị mindset để viết test UI có giá trị

## Vì sao phải test UI?

UI là nơi người dùng thực sự tương tác. Nếu không test, bạn dễ gặp:

- nút bấm không hoạt động
- state hiển thị sai
- text hoặc component không xuất hiện như mong đợi
- flow người dùng bị gãy sau refactor

## Test cái gì?

UI test tốt thường tập trung vào:

- thành phần có xuất hiện không
- text có đúng không
- tương tác click có tạo ra kết quả mong đợi không
- state khác nhau có render đúng không

## Test theo hành vi, không theo chi tiết cài đặt

Ví dụ tốt:

- khi `isLoading = true`, màn hình hiển thị loading indicator
- khi bấm nút retry, callback `onRetry` được gọi

Ví dụ yếu:

- kiểm tra quá sâu vào cách nội bộ composable được viết nếu điều đó không phản ánh hành vi người dùng thấy

## Tách stateless screen giúp test dễ hơn

Đây là một lợi ích lớn của kiến trúc Compose + state hoisting.

Nếu bạn có:

```kotlin
@Composable
fun TaskScreen(
    uiState: TaskUiState,
    onRetry: () -> Unit
)
```

thì việc test các trạng thái UI khác nhau sẽ dễ hơn nhiều.

## Các nhóm test nên nghĩ tới

### 1. Render test

UI có hiển thị đúng với state đã cho không?

### 2. Interaction test

Người dùng bấm nút, nhập text, chọn item thì callback hoặc thay đổi hiển thị có xảy ra đúng không?

### 3. State-specific test

- loading state
- empty state
- content state
- error state

Đây là các trạng thái rất đáng test ở screen level.

## Best practices

- Thiết kế composable dễ test bằng cách tách state và event rõ ràng.
- Test các trạng thái quan trọng của màn hình.
- Test những hành vi người dùng thật sự quan tâm.
- Ưu tiên test screen hoặc component có logic hiển thị đáng kể.

## Điều cần tránh

- Viết test chỉ để có số lượng test.
- Test quá chặt vào implementation details.
- Không tách UI state khiến test trở nên khó và mong manh.

## Checklist tự kiểm tra

1. Bạn có hiểu vì sao UI Compose vẫn cần test không?
2. Bạn có biết những trạng thái nào của màn hình nên test không?
3. Bạn có thấy lợi ích testability của composable stateless không?

## Bài tiếp theo

Trước khi ghép thành feature hoàn chỉnh, bạn cần học tư duy hiệu năng và best practices để tránh nhiều lỗi kiến trúc Compose phổ biến.
