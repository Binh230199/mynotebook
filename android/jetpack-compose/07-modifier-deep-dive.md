# 07. Modifier chuyên sâu

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- `Modifier` là gì
- vì sao `Modifier` là một trong những khái niệm quan trọng nhất của Compose
- các nhóm modifier phổ biến
- thứ tự modifier ảnh hưởng tới kết quả UI như thế nào

## Modifier là gì?

`Modifier` là cách Compose cho phép bạn mở rộng hoặc điều chỉnh hành vi của composable.

Nó thường dùng để:

- chỉnh kích thước
- thêm padding
- căn chỉnh
- tô nền
- bắt click
- thêm border
- kiểm soát layout behavior

Ví dụ:

```kotlin
Text(
    text = "Hello",
    modifier = Modifier.padding(16.dp)
)
```

## Vì sao Modifier quan trọng?

Trong XML cũ, rất nhiều thuộc tính layout nằm rải rác trong từng loại view khác nhau. Trong Compose, `Modifier` tạo ra một ngôn ngữ chung cho việc điều khiển UI.

Khi hiểu `Modifier`, bạn sẽ hiểu cách:

- đặt spacing
- fill width/height
- căn vị trí
- xử lý input
- trang trí component

## Một số modifier phổ biến

### Kích thước

```kotlin
Modifier
    .fillMaxWidth()
    .height(56.dp)
    .size(80.dp)
```

### Spacing

```kotlin
Modifier
    .padding(16.dp)
```

### Trang trí

```kotlin
Modifier
    .background(Color.LightGray)
```

### Tương tác

```kotlin
Modifier
    .clickable { /* action */ }
```

### Layout-specific

```kotlin
Modifier.weight(1f)
```

`weight()` thường dùng trong `Row` hoặc `Column` để phân chia không gian.

## Modifier chain

Modifier thường được viết theo chuỗi:

```kotlin
Modifier
    .fillMaxWidth()
    .padding(16.dp)
    .background(Color.Gray)
```

Điểm cực kỳ quan trọng là thứ tự modifier có thể ảnh hưởng tới kết quả.

## Ví dụ về thứ tự modifier

```kotlin
Modifier
    .background(Color.Yellow)
    .padding(16.dp)
```

khác với:

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Yellow)
```

Vì modifier được áp dụng theo chuỗi, nên kết quả vùng nền và vùng padding có thể khác nhau.

## Ví dụ trực quan

```kotlin
@Composable
fun TaskRow() {
    Row(
        modifier = Modifier
            .fillMaxWidth()
            .padding(16.dp)
    ) {
        Text(
            text = "Task title",
            modifier = Modifier.weight(1f)
        )
        Button(onClick = {}) {
            Text("Done")
        }
    }
}
```

Ở đây:

- `Row` chiếm toàn chiều ngang
- `Row` có padding ngoài
- `Text` dùng `weight(1f)` để chiếm phần không gian còn lại

## Modifier nên đặt ở đâu?

Best practice rất quan trọng là nhiều composable tùy biến nên nhận `modifier: Modifier = Modifier` từ bên ngoài.

```kotlin
@Composable
fun TaskTitle(
    title: String,
    modifier: Modifier = Modifier
) {
    Text(text = title, modifier = modifier)
}
```

Cách này giúp composable linh hoạt hơn và dễ tái sử dụng hơn.

## Best practices

- Hầu hết composable tự tạo nên nhận `modifier` từ ngoài.
- Viết chain modifier theo nhóm logic dễ đọc.
- Hiểu thứ tự modifier trước khi debug layout lạ.
- Chỉ dùng modifier cần thiết, đừng chain quá nhiều thứ vô nghĩa.

## Điều cần tránh

- Bỏ qua `modifier` parameter trong custom composable.
- Không để ý thứ tự modifier rồi thắc mắc vì sao UI khác mong đợi.
- Lạm dụng modifier để che đậy vấn đề layout design cơ bản.

## Checklist tự kiểm tra

1. Bạn có hiểu `Modifier` là công cụ gì trong Compose chưa?
2. Bạn có biết một số modifier cơ bản như `padding`, `fillMaxWidth`, `size`, `clickable` không?
3. Bạn có hiểu thứ tự modifier có thể ảnh hưởng tới UI không?
4. Bạn có biết vì sao custom composable nên nhận `modifier` từ bên ngoài không?

## Bài tiếp theo

Sau khi có layout và modifier, bạn sẽ làm việc với các component UI cơ bản như `Text`, `Button`, `Image` và những viên gạch nền tảng khác.
