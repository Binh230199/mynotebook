# 05. Scope và vòng đời component trong Hilt

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- scope giải quyết vấn đề gì
- vì sao object lifetime là chủ đề cực quan trọng trong DI
- cách tư duy về app-level, screen-level và object reuse
- vì sao sai scope dễ dẫn đến bug hoặc thiết kế kém

## Vì sao phải quan tâm tới lifetime?

Không phải object nào cũng nên được tạo mới liên tục. Nhưng cũng không phải object nào cũng nên sống suốt đời app.

Ví dụ:

- một API client có thể sống rất lâu
- một object gắn với màn hình có thể chỉ nên sống theo vòng đời màn hình
- một state holder ngắn hạn có thể không cần scope rộng

Nếu bạn không reason về lifetime, app sẽ rất nhanh trở nên khó hiểu.

## Scope là gì?

Scope là cách nói rằng một dependency nên được giữ ổn định trong một phạm vi vòng đời nào đó.

Hiểu đơn giản:

- cùng một scope phù hợp -> object có thể được tái sử dụng trong phạm vi đó
- ra khỏi scope -> object không còn ý nghĩa tồn tại nữa

## Tư duy quan trọng hơn annotation

Điều bạn phải hiểu trước là:

- dependency này thuộc vòng đời nào?
- ai sử dụng nó?
- nó có nên sống chung với app, activity, hay viewmodel?

Nếu chưa trả lời được các câu hỏi đó, bạn chưa nên chọn scope một cách máy móc.

## Ví dụ trực giác

### App-level dependency

Những thứ như cấu hình network hoặc singleton service thường có lifetime rất rộng.

### Screen-level dependency

Một số object chỉ phục vụ một màn hình hoặc một ViewModel cụ thể thì không nên sống quá rộng.

## Scope sai gây gì?

- object bị giữ lâu hơn cần thiết
- chia sẻ state ngoài ý muốn
- phụ thuộc khó hiểu
- bug khó theo dõi vì lifetime không đúng

## Scope không thay cho kiến trúc tốt

Nếu một class vốn chứa state nguy hiểm hoặc ôm nhiều trách nhiệm, gắn scope vào nó không làm thiết kế trở nên tốt hơn.

Scope chỉ giúp mô tả lifetime phù hợp hơn.

## Best practices

- Nghĩ về lifetime trước khi nghĩ về annotation.
- Chỉ mở rộng scope khi có lý do rõ ràng.
- Giữ state và trách nhiệm lớp rõ ràng để scope decision dễ hơn.
- Review những dependency được scope rộng thật cẩn thận.

## Điều cần tránh

- Gắn scope rộng mặc định cho mọi thứ.
- Không biết dependency thực sự sống theo vòng đời nào.
- Dùng scope để “chữa cháy” cho thiết kế thiếu rõ ràng.

## Checklist tự kiểm tra

1. Bạn có hiểu scope là cách mô tả lifetime không?
2. Bạn có thể phân biệt app-level dependency và screen-level dependency không?
3. Bạn có hiểu vì sao chọn sai scope có thể gây bug không?

## Bài tiếp theo

Sau khi hiểu lifetime, bạn có thể áp dụng Hilt vào kiến trúc app thật: inject repository, use case và data source một cách sạch hơn.
