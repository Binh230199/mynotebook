# 09. Sai lầm phổ biến và best practices khi dùng Hilt

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- những lỗi tư duy thường gặp khi dùng Hilt
- vì sao nhiều lỗi Hilt thật ra là lỗi kiến trúc
- cách giữ dependency graph dễ hiểu
- cách dùng Hilt thực dụng hơn trong app thật

## Sai lầm 1: inject mọi thứ

Không phải object nào cũng cần nằm trong DI graph.

Ví dụ state UI tạm thời nhỏ hoặc helper cục bộ đơn giản có thể không cần inject.

Hãy nhớ: dependency injection là công cụ để quản lý dependency có ý nghĩa, không phải để biến mọi object thành dependency framework-managed.

## Sai lầm 2: không hiểu lifetime

Nhiều bug đến từ việc không hiểu object nên sống bao lâu.

Hậu quả có thể là:

- state bị giữ quá lâu
- object bị chia sẻ ngoài ý muốn
- bug khó lý giải

## Sai lầm 3: activity hoặc UI layer ôm quá nhiều dependency

Nếu một activity hoặc composable cấp cao cần quá nhiều dependency trực tiếp, đó thường là dấu hiệu trách nhiệm đang bị dồn sai chỗ.

## Sai lầm 4: module hỗn loạn

Một file module dài chứa đủ mọi provider của toàn app là dấu hiệu cấu trúc đang bắt đầu xấu.

## Sai lầm 5: dùng Hilt để che kiến trúc yếu

Nếu class có trách nhiệm mơ hồ, phụ thuộc quá nhiều thứ và khó test, việc inject nó bằng Hilt không làm vấn đề biến mất.

## Best practices

- Chỉ inject khi có lý do rõ ràng.
- Nghĩ về lifetime trước khi chọn scope.
- Ưu tiên constructor injection khi phù hợp.
- Tổ chức module theo layer hoặc feature.
- Giữ ViewModel là điểm chính nhận dependency cho screen.
- Giữ UI layer càng độc lập với DI framework càng tốt.

## Dấu hiệu graph đang cần xem lại

- dependency chain quá dài và khó hiểu
- một class nhận quá nhiều dependency
- nhiều object scope rộng nhưng không rõ vì sao
- khó test mà không kéo cả ứng dụng lên

## Checklist tự kiểm tra

1. Bạn có đang inject thứ không cần inject không?
2. Bạn có hiểu lifetime của các dependency chính trong app không?
3. Bạn có đang giữ UI layer quá dính với Hilt không?
4. Module của bạn có đang phình thành một nơi chứa tất cả không?

## Bài tiếp theo

Bài cuối sẽ ghép lại Compose, MVVM và Hilt thành một feature thực tế để bạn nhìn toàn cảnh cách các mảnh ghép này làm việc cùng nhau.
