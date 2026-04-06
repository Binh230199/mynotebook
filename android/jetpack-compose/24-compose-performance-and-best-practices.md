# 24. Hiệu năng và best practices trong Compose

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- những nguyên tắc hiệu năng cơ bản trong Compose
- vì sao state placement ảnh hưởng lớn tới recompose
- cách viết UI dễ bảo trì hơn khi app lớn lên
- những sai lầm kiến trúc Compose phổ biến

## Compose không chậm vì Compose

Nhiều vấn đề hiệu năng trong Compose thật ra đến từ cách chúng ta đặt state, tổ chức composable và xử lý luồng dữ liệu.

Nếu bạn:

- đặt state sai chỗ
- truyền dữ liệu không hợp lý
- trộn logic và UI quá chặt
- tạo composable quá lớn

thì app sẽ khó tối ưu và khó bảo trì.

## Nguyên tắc 1: đặt state ở đúng nơi

State đặt quá thấp có thể gây phân mảnh.

State đặt quá cao không cần thiết có thể khiến vùng recompose lớn hơn mức cần thiết.

Bạn cần tìm đúng state owner phù hợp.

## Nguyên tắc 2: chia composable theo trách nhiệm

Composable quá lớn thường có các vấn đề:

- khó đọc
- khó preview
- khó test
- khó tối ưu

Hãy chia theo các khối UI có ý nghĩa rõ ràng.

## Nguyên tắc 3: giữ UI thuần càng nhiều càng tốt

Composable stateless, nhận state và callback rõ ràng, thường dễ reason hơn và bền hơn theo thời gian.

## Nguyên tắc 4: dùng list đúng cách

Với list động, hãy nghĩ về:

- `LazyColumn`
- `key`
- item identity
- state của từng item

Sai ở đây rất dễ tạo bug khó hiểu.

## Nguyên tắc 5: side effect phải có chủ đích

Nhiều bug Compose không đến từ layout mà đến từ side effect chạy sai thời điểm hoặc chạy lặp.

## Nguyên tắc 6: đừng tối ưu quá sớm

Trước hết hãy viết kiến trúc rõ ràng. Khi có vấn đề thật, hãy đo đạc và xem lại nơi đặt state, phạm vi recompose, cấu trúc screen.

## Dấu hiệu code Compose đang có vấn đề

- một composable dài hàng trăm dòng
- mọi composable tự giữ state kiểu riêng, không quy ước
- UI gọi thẳng repository hoặc dependency tầng dưới
- navigation logic nằm lẫn trong component nhỏ
- screen không có mô hình `UiState` rõ ràng

## Best practices tổng kết

- Tách `Route` và `Screen` khi hợp lý.
- Dùng `UiState` rõ ràng ở screen level.
- Hoist state có chủ đích.
- Giữ component UI tái sử dụng ở dạng stateless nếu có thể.
- Giữ luồng dữ liệu một chiều.
- Chỉ dùng side effect khi thực sự cần.

## Điều cần tránh

- Tối ưu vi mô khi kiến trúc tổng thể còn rối.
- Viết composable quá lớn và quá nhiều trách nhiệm.
- Không phân biệt local UI state với screen state.
- Lạm dụng `Context`, navigation hoặc effect trong component nhỏ.

## Checklist tự kiểm tra

1. Bạn có hiểu vì sao state placement quan trọng không?
2. Bạn có thể nhìn ra một composable đang ôm quá nhiều trách nhiệm không?
3. Bạn có thể mô tả các best practices cốt lõi của Compose + MVVM không?

## Bài tiếp theo

Bài cuối của series Compose sẽ ghép lại toàn bộ kiến thức để bạn thấy một feature thực tế nên được tổ chức như thế nào.
