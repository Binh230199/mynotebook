# 08. Hilt trong testing cơ bản

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- vì sao DI giúp testing dễ hơn
- Hilt ảnh hưởng thế nào tới unit test và instrumented test
- tư duy thay dependency thật bằng dependency test
- vì sao kiến trúc tốt vẫn là điều kiện tiên quyết cho test tốt

## Vì sao DI giúp test dễ hơn?

Nếu class nhận dependency từ bên ngoài, bạn có thể thay dependency thật bằng fake, stub hoặc test double dễ hơn.

Ví dụ:

- ViewModel nhận `TaskRepository`
- trong test, bạn cấp cho nó một fake repository

Điều này dễ hơn nhiều so với việc class tự new repository thật bên trong.

## Unit test và Hilt

Không phải mọi test đều cần khởi động Hilt. Với nhiều unit test thuần Kotlin, bạn hoàn toàn có thể tự tạo class cần test và truyền fake dependency thủ công.

Điều quan trọng là class của bạn đã được thiết kế theo kiểu dependency injection.

## Khi nào Hilt test support hữu ích?

Khi bạn test ở mức Android integration hoặc instrumented test, nơi dependency graph thực sự của app tham gia vào quá trình chạy, Hilt test support trở nên hữu ích hơn.

## Tư duy cốt lõi

Trước khi nghĩ tới annotation test của Hilt, hãy hỏi:

- mình đang test lớp nào?
- test ở cấp độ nào?
- có cần toàn bộ graph thật không?
- có thể thay dependency bằng fake đơn giản hơn không?

Rất nhiều test tốt không cần kéo cả graph Hilt vào.

## Hilt không thay kiến trúc testable

Nếu ViewModel của bạn vừa xử lý logic, vừa gọi Android framework trực tiếp, vừa ôm nhiều dependency rối, thì có Hilt cũng không làm test trở nên dễ.

Thiết kế class rõ ràng vẫn là gốc.

## Best practices

- Unit test ưu tiên fake dependency và constructor injection đơn giản.
- Chỉ dùng Hilt testing support khi thật sự cần môi trường integration phù hợp.
- Giữ abstraction đủ rõ để thay dependency trong test.
- Thiết kế class testable trước, công cụ test đến sau.

## Điều cần tránh

- Kéo Hilt vào mọi test một cách máy móc.
- Dùng dependency thật trong unit test khi không cần.
- Tưởng rằng có Hilt thì testability tự xuất hiện.

## Checklist tự kiểm tra

1. Bạn có hiểu vì sao DI làm test dễ hơn không?
2. Bạn có hiểu nhiều unit test không cần khởi động cả Hilt graph không?
3. Bạn có biết phải nghĩ theo cấp độ test trước khi chọn cách setup không?

## Bài tiếp theo

Trước khi khép series, bạn cần nắm những sai lầm Hilt rất hay gặp trong app thật.
