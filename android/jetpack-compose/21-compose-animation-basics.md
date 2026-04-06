# 21. Animation cơ bản trong Compose

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- animation trong Compose nên được nhìn như thế nào
- một số trường hợp animation cơ bản và hữu ích
- vì sao animation nên phục vụ trải nghiệm thay vì chỉ để đẹp
- cách tránh lạm dụng animation

## Vì sao animation quan trọng?

Animation tốt giúp người dùng hiểu:

- cái gì vừa thay đổi
- phần tử nào đang xuất hiện hoặc biến mất
- hành động vừa xảy ra có kết quả gì

Animation không chỉ để làm app đẹp. Nó còn giúp truyền đạt ngữ nghĩa tương tác.

## Tư duy đúng về animation

Compose là UI theo state. Vậy animation thường cũng là phản ứng trước thay đổi của state.

Ví dụ:

- `isExpanded` đổi từ `false` sang `true`
- một vùng nội dung mở ra bằng animation

Nghĩa là bạn không nên tách animation khỏi state model quá nhiều.

## Một số loại animation cơ bản

### 1. Xuất hiện và biến mất

Rất hữu ích khi nội dung có điều kiện hiển thị.

### 2. Đổi giá trị có animation

Ví dụ màu, kích thước, alpha hoặc padding thay đổi mềm mại hơn.

### 3. Chuyển đổi trạng thái UI nhỏ

Ví dụ icon xoay khi expand/collapse.

## Ví dụ tư duy đơn giản

```kotlin
val backgroundColor by animateColorAsState(
    targetValue = if (isSelected) Color.Green else Color.Gray
)
```

Ở đây màu nền phản ứng theo state `isSelected`.

## Animation nên dùng ở đâu?

Thường hữu ích cho:

- expand hoặc collapse
- loading indicator hoặc trạng thái chờ
- highlight item vừa thay đổi
- thay đổi nhỏ giúp người dùng theo dõi UI tốt hơn

## Khi nào không nên dùng nhiều?

Nếu animation:

- quá nhiều
- quá chậm
- xuất hiện ở mọi nơi
- không phục vụ hiểu biết của người dùng

thì nó sẽ làm UI nặng và gây mệt.

## Best practices

- Gắn animation với thay đổi state rõ ràng.
- Dùng animation vừa đủ để hỗ trợ nhận thức.
- Ưu tiên những animation đơn giản, dễ hiểu và nhất quán.

## Điều cần tránh

- Dùng animation chỉ vì “có thể làm được”.
- Animation kéo dài quá mức gây chậm tương tác.
- Dùng quá nhiều loại chuyển động khác nhau khiến UI lộn xộn.

## Checklist tự kiểm tra

1. Bạn có hiểu animation nên phản ứng theo state không?
2. Bạn có phân biệt được animation hữu ích và animation thừa không?
3. Bạn có thể nghĩ ra 2-3 chỗ trong app thật sự nên có animation không?

## Bài tiếp theo

Sau animation, bạn cần biết cách làm việc với tài nguyên Android và ngữ cảnh nền tảng khi ở trong Compose.
