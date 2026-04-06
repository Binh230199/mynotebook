# Hilt cho Android hiện đại

## Mục tiêu của series này

Series này giúp bạn hiểu Hilt theo hướng thực dụng để xây Android app hiện đại với:

- Kotlin
- MVVM
- ViewModel
- Repository pattern
- Jetpack Compose

Mục tiêu không phải là học mọi chi tiết nội bộ của dependency injection framework, mà là hiểu đủ chắc để:

- cấu hình Hilt đúng
- inject dependency đúng chỗ
- tránh kiến trúc rối
- phối hợp Hilt với Compose + ViewModel hiệu quả

## Bạn nên học series này khi nào?

Nên học sau khi bạn đã có nền tảng:

- Android project structure
- Gradle/AGP/Kotlin cơ bản
- Kotlin cơ bản
- ViewModel và MVVM
- Jetpack Compose cơ bản

Nếu bạn đã đi qua series Compose trong [docs/jetpack-compose](../jetpack-compose/README.md), đây là bước tiếp theo rất hợp lý.

## Hilt giải quyết vấn đề gì?

Khi app còn nhỏ, bạn có thể tự tạo object bằng tay. Nhưng khi app lớn hơn, bạn sẽ nhanh gặp các vấn đề:

- ViewModel cần repository
- repository cần data source
- data source cần API service hoặc database
- nhiều object có vòng đời khác nhau
- việc tự khởi tạo thủ công bắt đầu rối và khó test

Hilt giúp bạn quản lý dependency có cấu trúc hơn.

## Lộ trình học

1. [01. Vì sao cần dependency injection và Hilt](01-why-dependency-injection-and-hilt.md)
2. [02. Thiết lập Hilt trong project Android](02-project-setup-hilt.md)
3. [03. Application, Activity và ViewModel với Hilt](03-hilt-application-activity-and-viewmodel.md)
4. [04. Module, @Provides và @Binds](04-hilt-modules-provides-and-binds.md)
5. [05. Scope và vòng đời component](05-scopes-and-component-lifetimes.md)
6. [06. Inject repository, use case và data source](06-inject-repository-usecase-and-datasource.md)
7. [07. Hilt với Compose và hiltViewModel()](07-hilt-with-compose-and-hiltviewmodel.md)
8. [08. Hilt trong testing cơ bản](08-hilt-testing-basics.md)
9. [09. Sai lầm phổ biến và best practices](09-common-mistakes-and-best-practices.md)
10. [10. Ghép Compose + MVVM + Hilt thành một feature hoàn chỉnh](10-build-a-feature-with-compose-mvvm-hilt.md)

## Cách dùng series này

- Đọc theo thứ tự.
- Sau mỗi bài, thử nối khái niệm vào một app nhỏ thật.
- Không học Hilt như một bộ annotation cần nhớ máy móc. Hãy luôn hỏi:
  - object này được tạo ở đâu?
  - ai sở hữu nó?
  - vòng đời của nó là gì?
  - có nên inject nó hay tự tạo local?

## Kết quả mong đợi

Sau series này, bạn nên đủ khả năng:

- thiết lập Hilt đúng trong project
- inject dependency vào ViewModel và các lớp phù hợp
- tránh lạm dụng DI
- tổ chức app Compose + MVVM sạch hơn
- hiểu các lỗi Hilt phổ biến và cách reason về chúng
