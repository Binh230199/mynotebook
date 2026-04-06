# Jetpack Compose + MVVM Learning Path

Bộ tài liệu này được viết để tiếp nối trực tiếp chuỗi bài về Android Studio và Kotlin cơ bản. Mục tiêu là đưa bạn đi từ chỗ mới bắt đầu với Jetpack Compose tới chỗ có thể tự tin xây dựng ứng dụng Android bằng Compose theo mô hình MVVM, có tích hợp Hilt ở mức thực dụng.

## Cách dùng bộ tài liệu này

- Đọc theo đúng thứ tự từ bài `01` trở đi.
- Mỗi bài là một chủ đề tương đối atomic, nhưng vẫn nối logic với bài trước.
- Không nên nhảy cóc nếu bạn chưa quen Compose, vì nhiều bài sau dựa vào tư duy state, recomposition và MVVM của các bài đầu.
- Khi gặp phần Hilt, hãy đọc song song bộ tài liệu trong thư mục `docs/hilt`.

## Trục kiến thức chính của chuỗi Compose

1. Hiểu Compose là gì và vì sao nó khác UI XML truyền thống.
2. Biết cấu hình project để Compose chạy ổn định.
3. Hiểu composable, preview, layout và modifier.
4. Hiểu state, recomposition, remember, rememberSaveable.
5. Dùng Compose theo tư duy MVVM với `ViewModel` và `StateFlow`.
6. Xây dựng UI thực tế với Material 3, Scaffold, danh sách, form, dialog, sheet.
7. Điều hướng bằng Navigation Compose.
8. Hiểu side effects, lifecycle integration, testing, performance.
9. Ghép tất cả lại thành một feature hoàn chỉnh theo MVVM.

## Ví dụ xuyên suốt

Trong chuỗi bài này, nhiều ví dụ sẽ xoay quanh một ứng dụng mẫu đơn giản kiểu `Task / Notes app` có các phần:

- màn hình danh sách công việc
- màn hình tạo hoặc sửa công việc
- bộ lọc trạng thái
- màn hình chi tiết
- settings cơ bản

Lý do chọn dạng app này là vì nó đủ nhỏ để học, nhưng đủ thật để bao phủ:

- layout
- form
- state
- ViewModel
- navigation
- repository
- DataStore
- Hilt

## Danh sách bài học

| Bài | Chủ đề |
| --- | --- |
| 01 | [Compose là gì và Compose đứng ở đâu trong kiến trúc MVVM](./01-compose-overview-and-mvvm.md) |
| 02 | [Cấu hình project cho Jetpack Compose](./02-project-setup-compose.md) |
| 03 | [Composable đầu tiên, Preview và tư duy declarative UI](./03-first-composable-and-preview.md) |
| 04 | [State cơ bản với remember và mutableStateOf](./04-compose-state-remember-and-mutablestateof.md) |
| 05 | [Recomposition và cách nghĩ đúng về UI state](./05-recomposition-and-state-thinking.md) |
| 06 | [Layout cơ bản với Column, Row, Box](./06-layout-basics-column-row-box.md) |
| 07 | [Modifier chuyên sâu](./07-modifier-deep-dive.md) |
| 08 | [Text, Button, Image và các component cơ bản](./08-text-button-image-and-basic-components.md) |
| 09 | [TextField, input và form state](./09-textfield-input-and-form-state.md) |
| 10 | [Scaffold và cấu trúc màn hình Material](./10-scaffold-material-structure.md) |
| 11 | [Material 3 theme và design system cơ bản](./11-material3-theme-and-design-system.md) |
| 12 | [Danh sách với LazyColumn, Lazy layouts và quản lý item state](./12-lists-lazycolumn-grid-and-state.md) |
| 13 | [State hoisting và unidirectional data flow](./13-state-hoisting-and-udf.md) |
| 14 | [Compose với ViewModel](./14-compose-with-viewmodel.md) |
| 15 | [StateFlow, collectAsStateWithLifecycle và lifecycle](./15-stateflow-compose-and-lifecycle.md) |
| 16 | [Navigation Compose cơ bản](./16-navigation-compose-basics.md) |
| 17 | [Navigation arguments, nested graphs và kiến trúc điều hướng](./17-navigation-arguments-and-nested-graphs.md) |
| 18 | [Side effects: LaunchedEffect, DisposableEffect, SideEffect](./18-side-effects-in-compose.md) |
| 19 | [rememberSaveable và khôi phục state](./19-remembersaveable-and-state-restoration.md) |
| 20 | [Dialog, BottomSheet, Snackbar và phản hồi UI](./20-dialog-sheet-snackbar-and-feedback.md) |
| 21 | [Animation cơ bản trong Compose](./21-animation-basics.md) |
| 22 | [Tài nguyên trong Compose: string, painter, LocalContext](./22-resources-painter-string-and-localcontext.md) |
| 23 | [Compose UI testing](./23-compose-testing.md) |
| 24 | [Hiệu năng Compose và các best practices](./24-compose-performance-and-best-practices.md) |
| 25 | [Xây dựng một feature hoàn chỉnh với Compose + MVVM](./25-build-a-feature-end-to-end-mvvm-compose.md) |

## Bộ tài liệu Hilt đi kèm

Khi bắt đầu đi vào dependency injection trong app Compose + MVVM, đọc tiếp:

- thư mục `docs/hilt`

Chuỗi Hilt này được viết vừa đủ để bạn dùng được trong ứng dụng Compose thực tế mà không bị chìm quá sâu vào lý thuyết DI ngay từ đầu.

## Cách học hiệu quả

1. Đọc một bài.
2. Viết lại ví dụ bằng tay.
3. Tự thay đổi ví dụ để xem điều gì xảy ra.
4. So sánh với project thật nếu bạn đang có codebase Android mở sẵn.
5. Chỉ chuyển bài sau khi bạn hiểu vì sao ví dụ hoạt động.

## Kỳ vọng sau khi học xong

Sau toàn bộ chuỗi này, bạn nên có thể:

- đọc và viết UI Compose có cấu trúc
- quản lý state đúng cách
- nối UI với ViewModel theo MVVM
- điều hướng giữa các màn hình
- xử lý side effects đúng ngữ cảnh
- test và tối ưu code Compose ở mức cơ bản đến trung cấp
- bắt đầu xây một app Android mới bằng Compose với nền tảng khá chắc
