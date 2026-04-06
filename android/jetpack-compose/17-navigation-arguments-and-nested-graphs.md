# 17. Navigation arguments và nested graphs

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- cách truyền tham số giữa các màn hình
- vì sao navigation arguments cần được thiết kế cẩn thận
- nested graphs là gì
- cách tổ chức flow nhiều màn hình rõ ràng hơn

## Khi nào cần arguments?

Ví dụ:

- từ task list đi tới task detail theo `taskId`
- từ danh sách sản phẩm sang chi tiết theo `productId`
- từ list sang edit screen theo `id`

Bạn thường không truyền cả object lớn qua navigation. Thường bạn truyền một định danh như `id`, rồi màn hình đích tự lấy dữ liệu từ ViewModel hoặc repository.

## Ví dụ truyền `taskId`

```kotlin
composable("task/{taskId}") {
    TaskDetailScreen()
}
```

Khi điều hướng:

```kotlin
navController.navigate("task/42")
```

Ý tưởng là route chứa placeholder và lời gọi `navigate()` đưa giá trị thật vào.

## Vì sao không nên truyền object lớn?

Vì:

- dễ rối
- tăng coupling giữa màn hình
- khó quản lý lifecycle và nguồn dữ liệu
- dễ làm route khó bảo trì

Cách an toàn hơn thường là truyền ID, sau đó màn hình đích dùng ViewModel để tải dữ liệu phù hợp.

## Nested graphs là gì?

Khi app lớn hơn, bạn không muốn mọi route nằm phẳng trong một graph dài. Bạn có thể nhóm các màn hình thành flow hoặc feature graph.

Ví dụ tư duy:

- auth graph
- main graph
- settings graph

Nested graph giúp code navigation có cấu trúc hơn khi số lượng màn hình tăng.

## Tổ chức feature theo graph

Ví dụ một flow task:

- task list
- task detail
- task edit

Bạn có thể nghĩ đây là một nhóm màn hình thuộc cùng một feature.

Điều này giúp:

- route rõ hơn
- dễ bảo trì hơn
- dễ tách module hơn về sau

## Event từ UI vẫn nên giữ sạch

Ngay cả khi có arguments và nested graphs, composable UI vẫn nên phát event kiểu:

```kotlin
onTaskClick(task.id)
```

Sau đó route hoặc graph layer quyết định navigation string cụ thể.

## Best practices

- Truyền ID hoặc dữ liệu tối thiểu cần thiết qua navigation.
- Để màn hình đích tự lấy dữ liệu bằng ViewModel nếu hợp lý.
- Nhóm các màn hình lớn theo flow hoặc feature graph.
- Giữ UI tách khỏi chi tiết route string.

## Điều cần tránh

- Truyền object lớn hoặc state phức tạp qua route.
- Graph phẳng quá lớn, khó nhìn và khó bảo trì.
- Để UI component nhỏ tự biết chi tiết route template.

## Checklist tự kiểm tra

1. Bạn có hiểu vì sao nên truyền ID thay vì object lớn không?
2. Bạn có hiểu nested graph giúp gì khi app lớn không?
3. Bạn có biết giữ route logic ở tầng trên thay vì nhét vào UI component không?

## Bài tiếp theo

Không phải mọi thứ trong Compose đều là state hiển thị. Có những việc phải làm khi màn hình vào, khi dependency đổi hoặc khi cần đồng bộ với thế giới bên ngoài. Đó là side effects.
