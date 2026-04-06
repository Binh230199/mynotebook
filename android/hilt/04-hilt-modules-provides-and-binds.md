# 04. Module, @Provides và @Binds

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- module trong Hilt dùng để làm gì
- khi nào cần tự mô tả cách tạo object
- khác nhau ở mức tư duy giữa `@Provides` và `@Binds`
- cách tránh biến module thành kho tạp nham

## Hilt có tự tạo được mọi object không?

Không. Hilt chỉ tự tạo tốt những object mà nó biết cách khởi tạo dựa trên constructor injection hoặc wiring đã được mô tả rõ.

Khi bạn cần chỉ cho Hilt cách tạo một dependency, bạn dùng module.

## Module là gì?

Bạn có thể hiểu module là nơi khai báo cách cung cấp dependency cho graph.

Ví dụ những thứ hay cần module:

- API client
- repository implementation theo interface
- database hoặc DAO provider
- utility object cần cấu hình đặc biệt

## `@Provides`

Dùng khi bạn cần viết rõ cách tạo object.

Thường phù hợp với các object:

- không thể sửa constructor
- đến từ thư viện ngoài
- cần nhiều bước cấu hình để tạo

## `@Binds`

Dùng khi bạn muốn nói với Hilt rằng:

- khi ai đó cần interface A
- hãy cung cấp implementation B

Tư duy của `@Binds` là mapping giữa abstraction và implementation.

## Ví dụ tư duy phổ biến

Nếu bạn có:

- `TaskRepository` là interface
- `TaskRepositoryImpl` là implementation

thì module là nơi khai báo implementation nào được dùng cho abstraction đó.

## Khi nào nên dùng constructor injection thay vì module?

Nếu class của bạn đơn giản và bạn kiểm soát constructor, constructor injection thường là lựa chọn gọn hơn.

Module chỉ nên xuất hiện khi thật sự cần mô tả cách cung cấp dependency hoặc mapping interface.

## Module nên được tổ chức thế nào?

Khi app lớn dần, đừng bỏ mọi provider vào một file module khổng lồ. Hãy tổ chức module theo feature hoặc layer hợp lý, ví dụ:

- network module
- database module
- repository module

## Best practices

- Ưu tiên constructor injection khi đủ dùng.
- Dùng `@Provides` khi phải mô tả cách tạo object.
- Dùng `@Binds` khi map interface sang implementation.
- Tổ chức module theo mục đích rõ ràng.

## Điều cần tránh

- Biến một module thành nơi chứa mọi loại dependency của app.
- Dùng module cho cả những class mà constructor injection đã đủ.
- Không tách abstraction và implementation rõ ràng khi app cần testability.

## Checklist tự kiểm tra

1. Bạn có hiểu module dùng để làm gì không?
2. Bạn có phân biệt được `@Provides` và `@Binds` ở mức tư duy không?
3. Bạn có biết khi nào constructor injection là đủ không?

## Bài tiếp theo

Sau khi biết Hilt tạo object thế nào, bạn phải hiểu object đó sống bao lâu. Đó là câu chuyện scope và component lifetime.
