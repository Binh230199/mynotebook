# 05. Recomposition và cách nghĩ đúng về UI state

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- recomposition là gì
- vì sao recomposition là trung tâm của Compose
- Compose không phải lúc nào cũng vẽ lại toàn bộ màn hình
- nên suy nghĩ về state như thế nào để tránh UI lỗi và code khó hiểu

## Recomposition là gì?

Recomposition là quá trình Compose chạy lại một composable khi state mà composable đó đọc thay đổi.

Ví dụ:

```kotlin
@Composable
fun CounterLabel(count: Int) {
    Text(text = "Count: $count")
}
```

Khi `count` đổi, Compose có thể chạy lại `CounterLabel()` để tạo UI phù hợp với giá trị mới.

## Compose có vẽ lại toàn bộ màn hình không?

Không nhất thiết.

Đây là một hiểu nhầm rất phổ biến. Compose cố gắng recompose đúng phần UI bị ảnh hưởng bởi state thay đổi, thay vì làm lại mọi thứ vô điều kiện.

Tuy nhiên, để Compose giúp bạn hiệu quả, bạn phải viết code theo tư duy đúng:

- state rõ ràng
- composable nhỏ và có trách nhiệm rõ
- không trộn logic phụ có side effect ngay trong phần render

## Ví dụ dễ hình dung

```kotlin
@Composable
fun Screen(count: Int) {
    Column {
        Text("Header")
        Text("Count: $count")
        Text("Footer")
    }
}
```

Khi `count` đổi, Compose không cần xem đây như “xóa hết rồi dựng lại app từ đầu”. Nó xử lý ở mức runtime tree của Compose để cập nhật phần liên quan.

## Vì sao phải hiểu recomposition?

Nếu không hiểu recomposition, người mới thường:

- đặt log lung tung rồi nghĩ Compose đang “chạy điên”
- gọi API trực tiếp trong composable body
- khởi tạo object nặng ở nơi không phù hợp
- giữ state sai chỗ
- lẫn lộn render logic với side effect

## Tư duy đúng: composable nên giống hàm render

Composable nên được nghĩ gần giống một hàm nhận state rồi mô tả UI tương ứng.

Ví dụ:

```kotlin
@Composable
fun LoginContent(uiState: LoginUiState) {
    if (uiState.isLoading) {
        CircularProgressIndicator()
    } else {
        Text("Ready")
    }
}
```

Cách nghĩ nên là:

- nếu state là loading thì hiện loading
- nếu state không loading thì hiện nội dung phù hợp

Không nên nghĩ:

- bây giờ chạy hàm nào đó để “bật loading view” như kiểu imperative cũ

## Recomposition không phải lỗi, nó là cơ chế hoạt động bình thường

Một số người mới thấy composable chạy lại nhiều lần thì nghĩ có bug. Thực ra, recomposition là hành vi bình thường của Compose.

Cái cần quan tâm không phải là “có recomposition không”, mà là:

- recomposition có đúng chỗ không
- có side effect sai chỗ không
- có tạo object hoặc chạy logic nặng không cần thiết không

## Ví dụ sai phổ biến

```kotlin
@Composable
fun WrongScreen() {
    println("API call here")
    // gọi API ngay trong body composable là sai tư duy
}
```

Vì body composable có thể chạy lại nhiều lần. Nếu bạn đặt logic side effect trực tiếp ở đó, bạn có thể vô tình gọi lại quá nhiều lần.

## Nguồn sự thật của UI state

Trong app MVVM, screen state thường nên tới từ ViewModel.

Composable không nên tự tạo ra business state cốt lõi của màn hình nếu state đó:

- cần sống lâu hơn một composable nhỏ
- cần logic xử lý riêng
- cần đồng bộ với repository hoặc use case

Điểm này giúp recomposition có đường đi rõ ràng:

1. ViewModel cập nhật state
2. Compose đọc state mới
3. UI recompose theo state đó

## Best practices

- Viết composable theo tư duy render từ state.
- Tách composable nhỏ khi logic UI bắt đầu phình ra.
- Phân biệt render logic với side effect.
- Để ViewModel giữ screen state theo MVVM.
- Đừng hoảng khi thấy recomposition; hãy hiểu nó.

## Điều cần tránh

- Gọi API, ghi database, tạo side effect trực tiếp trong body composable.
- Nhồi nhiều trách nhiệm vào một composable lớn.
- Giữ business state quan trọng ở local state mà không có lý do rõ ràng.
- Debug bằng cách chống lại recomposition thay vì hiểu nó.

## Checklist tự kiểm tra

1. Bạn có hiểu recomposition là gì chưa?
2. Bạn có hiểu vì sao Compose không phải lúc nào cũng “vẽ lại tất cả” không?
3. Bạn có biết vì sao side effect không nên đặt trực tiếp trong body composable không?
4. Bạn có bắt đầu nghĩ UI theo chiều state -> render chưa?

## Bài tiếp theo

Sau khi hiểu recomposition, bạn cần làm chủ cách dựng bố cục trong Compose với `Column`, `Row` và `Box`.
