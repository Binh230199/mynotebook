# 02. Thiết lập Hilt trong project Android

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- Hilt cần những phần cấu hình nào trong project
- vai trò của plugin và dependencies
- vì sao cấu hình build đúng là điều kiện tiên quyết
- các lỗi setup phổ biến thường đến từ đâu

## Các mảnh ghép chính khi setup Hilt

Khi thêm Hilt vào project, bạn thường cần:

- dependency Hilt runtime
- compiler hoặc annotation processing tương ứng
- plugin Hilt cho Gradle
- cấu hình phù hợp trong module app

Ý tưởng chung là build system phải biết cách:

- nhận diện annotation Hilt
- sinh code cần thiết
- gắn wiring vào Android app

## Tư duy setup đúng

Đừng chỉ copy dependency. Hãy hiểu Hilt là một thư viện có code generation. Vì thế nếu cấu hình build sai, lỗi thường xuất hiện ở sync, compile hoặc generated code.

## Những chỗ cần kiểm tra

### 1. Version compatibility

Hilt phải phù hợp với:

- Kotlin version
- AGP version
- annotation processing setup của project

Nếu stack lệch phiên bản, bạn có thể gặp lỗi build khó đọc.

### 2. Module app

Module app là nơi thường bật plugin Hilt và thêm dependency chính.

### 3. KSP hoặc KAPT

Tùy stack của project, bạn cần hiểu project đang dùng cơ chế annotation processing nào. Điều quan trọng không phải thuộc lòng từng dependency, mà là biết project đang dùng pipeline nào và giữ nó nhất quán.

## Sau setup, bước tiếp theo là gì?

Sau khi build config đúng, bạn còn phải:

- đánh dấu `Application` phù hợp
- đánh dấu Android entry points cần inject
- cấu hình ViewModel để Hilt tạo được dependency

## Lỗi setup phổ biến

- thiếu plugin hoặc dependency tương ứng
- sai version giữa các thành phần build
- cấu hình annotation processing không khớp với stack project
- thêm dependency ở sai module

## Cách tư duy khi debug

Nếu Hilt chưa chạy, đừng nhìn vào annotation trước. Hãy debug theo lớp:

1. Gradle sync có ổn không?
2. dependency đã được resolve chưa?
3. plugin có được apply đúng không?
4. generated code có đang được build không?
5. Android entry point đã được đánh dấu đúng chưa?

## Best practices

- Giữ version toolchain nhất quán.
- Hiểu project đang dùng cơ chế code generation nào.
- Setup Hilt theo đúng module cần dùng.
- Nếu gặp lỗi build, debug từ build setup trước khi debug annotation.

## Điều cần tránh

- Copy config từ nhiều nguồn khác nhau rồi trộn bừa.
- Không kiểm tra compatibility của stack.
- Chỉ nhìn lỗi ở class đang inject mà bỏ qua lỗi build nền phía dưới.

## Checklist tự kiểm tra

1. Bạn có hiểu Hilt cần plugin, dependency và code generation không?
2. Bạn có hiểu vì sao version compatibility quan trọng không?
3. Bạn có biết nếu Hilt lỗi thì phải debug từ build setup trước không?

## Bài tiếp theo

Sau setup, bạn sẽ nối Hilt với Android runtime qua `Application`, entry points và `ViewModel`.
