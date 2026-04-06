# 11. Material 3 theme và design system cơ bản

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- Material 3 theme là gì
- color scheme, typography, shape đóng vai trò gì
- vì sao app Compose nên có theme nhất quán sớm
- cách nghĩ về design system cơ bản trong app Android

## Vì sao theme quan trọng?

Nếu mỗi màn hình tự chọn màu, font, kích thước và style theo ý riêng, app sẽ rất nhanh trở nên rời rạc và khó bảo trì.

Theme giúp bạn có một nguồn sự thật chung cho các quyết định hiển thị như:

- màu chính
- màu nền
- màu lỗi
- kiểu chữ tiêu đề, nội dung, caption
- độ bo góc và ngôn ngữ hình khối

## MaterialTheme trong Compose

Compose Material 3 thường dùng `MaterialTheme` để cung cấp theme cho cây UI.

Ví dụ ý tưởng:

```kotlin
MaterialTheme(
    colorScheme = lightColorScheme(),
    typography = Typography(),
    content = { AppContent() }
)
```

Trong project thật, bạn thường bọc app hoặc screen root bằng custom theme của mình.

## Color scheme

Color scheme mô tả các màu có vai trò rõ ràng như:

- `primary`
- `secondary`
- `background`
- `surface`
- `error`
- `onPrimary`, `onSurface`, ...

Điểm quan trọng là đừng nghĩ theo kiểu “màu đỏ, xanh, tím” đơn thuần. Hãy nghĩ theo vai trò. Ví dụ:

- `primary` là màu hành động chính
- `surface` là màu nền của card hoặc container
- `error` là màu báo lỗi

## Typography

Typography giúp bạn định nghĩa hệ thống kiểu chữ cho app.

Ví dụ:

- `titleLarge`
- `titleMedium`
- `bodyLarge`
- `bodyMedium`
- `labelSmall`

Dùng typography từ theme giúp app đồng nhất hơn rất nhiều.

## Shapes

Shapes mô tả ngôn ngữ bo góc và hình khối của app.

Ví dụ, card, button, sheet có thể dùng các shape thống nhất để cảm giác sản phẩm rõ ràng hơn.

## Ví dụ sử dụng theme

```kotlin
Text(
    text = "Task title",
    style = MaterialTheme.typography.titleMedium,
    color = MaterialTheme.colorScheme.onSurface
)
```

Đây là cách Compose khuyến khích bạn phụ thuộc vào theme thay vì hardcode style ở mọi nơi.

## Design system cơ bản

Design system không nhất thiết phải là thứ quá lớn như ở công ty lớn. Ở mức vừa đủ, nó có thể là:

- bảng màu chính
- typography rules
- spacing conventions
- cách đặt tên component
- một số custom composable tái sử dụng

Ví dụ:

- `AppPrimaryButton`
- `AppSectionTitle`
- `AppCard`

Những component này được xây trên Material 3 nhưng mang style riêng của app.

## Best practices

- Tạo theme rõ ràng từ sớm.
- Dùng màu theo vai trò, không theo cảm tính.
- Dùng typography từ theme thay vì hardcode khắp nơi.
- Tách các component tái sử dụng thành lớp design system nhỏ nếu app bắt đầu lớn.

## Điều cần tránh

- Hardcode màu và text style trong mọi composable.
- Không có quy ước spacing, shape và title hierarchy.
- Đổi theme cục bộ bừa bãi khiến UI mất nhất quán.

## Checklist tự kiểm tra

1. Bạn có hiểu theme là nguồn sự thật cho style của app không?
2. Bạn có biết color scheme và typography dùng để làm gì không?
3. Bạn có hiểu design system cơ bản ở mức app vừa và nhỏ nên bắt đầu từ đâu không?

## Bài tiếp theo

Sau theme, bạn sẽ gặp một loại UI cực phổ biến: danh sách. Compose giải quyết danh sách bằng các lazy layout rất mạnh.
