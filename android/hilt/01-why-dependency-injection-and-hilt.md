# 01. Vì sao cần dependency injection và Hilt

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- dependency là gì
- dependency injection là gì
- vì sao app Android càng lớn càng cần DI
- Hilt giúp gì so với việc tự khởi tạo object bằng tay

## Dependency là gì?

Một class hiếm khi sống một mình. Nó thường cần các object khác để làm việc.

Ví dụ:

- `TaskViewModel` cần `TaskRepository`
- `TaskRepository` cần `TaskApi` hoặc `TaskDao`
- `TaskUseCase` cần `TaskRepository`

Những object mà một class phụ thuộc vào gọi là dependency.

## Nếu không có DI thì sao?

Bạn có thể tự tạo object bằng tay:

```kotlin
val repository = TaskRepository(TaskApiImpl())
val viewModel = TaskViewModel(repository)
```

Với app rất nhỏ, cách này không có gì sai. Nhưng khi app lớn lên, việc tự new object bắt đầu gây nhiều vấn đề:

- nơi tạo object bị rải rác
- khó thay implementation
- khó test
- khó quản lý vòng đời
- code khởi tạo chồng chéo, khó theo dõi

## Dependency Injection là gì?

Dependency Injection là cách cung cấp dependency từ bên ngoài vào class thay vì để class tự tạo dependency cho chính nó.

Ví dụ tư duy:

```kotlin
class TaskViewModel(
    private val repository: TaskRepository
)
```

`TaskViewModel` không tự biết cách tạo `TaskRepository`. Nó chỉ nhận repository từ bên ngoài.

Điều này giúp class:

- ít coupling hơn
- dễ test hơn
- rõ trách nhiệm hơn

## Hilt là gì?

Hilt là thư viện dependency injection của Android, được xây trên Dagger, giúp tích hợp DI vào Android app dễ hơn.

Hilt cung cấp:

- cách khai báo dependency bằng annotation
- cách gắn dependency với vòng đời Android
- cách inject vào Android entry point và ViewModel
- cách tổ chức module cung cấp object

## Vì sao Hilt hữu ích trong Android?

Android có nhiều loại vòng đời phức tạp:

- application
- activity
- fragment
- viewmodel

Hilt giúp bạn reason rõ hơn về việc:

- object nào nên sống lâu
- object nào nên sống theo màn hình
- object nào nên được tạo lại khi cần

## Một ví dụ trực giác

Giả sử `TaskViewModel` cần `TaskRepository`.

Không dùng Hilt, bạn phải tự lo:

- tạo repository ở đâu?
- truyền nó vào ViewModel thế nào?
- nếu repository lại cần API client thì sao?

Dùng Hilt, bạn mô tả dependency graph, còn framework hỗ trợ việc tạo và cung cấp object đúng chỗ.

## DI không phải phép màu

Hilt không thay thế thiết kế kiến trúc. Nếu bạn có một kiến trúc rối, Hilt chỉ làm nó được inject một cách có tổ chức hơn, chứ không tự biến nó thành thiết kế tốt.

Hãy nhớ:

- DI giúp wiring dependencies
- DI không tự sửa business logic tệ
- DI không có nghĩa là mọi thứ đều phải inject

## Best practices

- Dùng DI để giảm coupling và tăng testability.
- Chỉ inject những dependency thực sự là dependency.
- Vẫn giữ trách nhiệm lớp rõ ràng, dù có dùng Hilt.

## Điều cần tránh

- Học Hilt như bộ annotation để thuộc lòng mà không hiểu dependency graph.
- Inject mọi thứ một cách máy móc.
- Dùng Hilt để che giấu thiết kế lớp kém.

## Checklist tự kiểm tra

1. Bạn có hiểu dependency là gì không?
2. Bạn có hiểu dependency injection giải quyết vấn đề gì không?
3. Bạn có hiểu Hilt hữu ích ở Android vì yếu tố vòng đời không?

## Bài tiếp theo

Sau khi hiểu vì sao cần Hilt, bạn sẽ thiết lập Hilt đúng ở mức project và module.
