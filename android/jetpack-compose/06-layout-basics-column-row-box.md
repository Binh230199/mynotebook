# 06. Layout cơ bản với Column, Row, Box

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- `Column`, `Row`, `Box` dùng để làm gì
- khi nào nên chọn từng layout
- `verticalArrangement`, `horizontalArrangement`, `Alignment` có ý nghĩa gì
- cách kết hợp layout để tạo màn hình đơn giản trong Compose

## Vì sao cần nắm chắc ba layout này?

Rất nhiều màn hình Compose thực tế được xây từ những viên gạch cơ bản sau:

- `Column`
- `Row`
- `Box`

Nếu nắm chắc ba khối này, bạn có thể xây phần lớn UI cơ bản trước khi cần tới các container phức tạp hơn.

## `Column`

`Column` xếp các phần tử theo chiều dọc.

```kotlin
@Composable
fun ProfileColumn() {
    Column {
        Text("Name")
        Text("Email")
        Button(onClick = {}) {
            Text("Save")
        }
    }
}
```

Dùng khi:

- các phần tử đi từ trên xuống dưới
- form đơn giản
- màn hình thông tin dạng khối dọc

## `Row`

`Row` xếp phần tử theo chiều ngang.

```kotlin
@Composable
fun ActionRow() {
    Row {
        Button(onClick = {}) { Text("Back") }
        Button(onClick = {}) { Text("Next") }
    }
}
```

Dùng khi:

- các phần tử cần nằm cạnh nhau
- hàng nút hành động
- icon + text
- item ngắn trong list

## `Box`

`Box` cho phép chồng hoặc neo phần tử theo cùng một vùng.

```kotlin
@Composable
fun AvatarWithBadge() {
    Box {
        Text("Avatar")
        Text("Online")
    }
}
```

`Box` hữu ích khi:

- muốn overlay nội dung
- muốn đặt phần tử ở góc hoặc giữa vùng chứa
- cần container đơn để stack nhiều lớp UI

## Arrangement và Alignment

Compose cho bạn kiểm soát cách các phần tử được sắp xếp.

Ví dụ `Column`:

```kotlin
Column(
    verticalArrangement = Arrangement.spacedBy(8.dp),
    horizontalAlignment = Alignment.CenterHorizontally
) {
    Text("A")
    Text("B")
}
```

Ví dụ `Row`:

```kotlin
Row(
    horizontalArrangement = Arrangement.SpaceBetween,
    verticalAlignment = Alignment.CenterVertically
) {
    Text("Title")
    Button(onClick = {}) { Text("Edit") }
}
```

## Ví dụ màn hình đơn giản

```kotlin
@Composable
fun TaskCard() {
    Column {
        Text(text = "Buy groceries")
        Row {
            Text(text = "Today")
            Text(text = "High priority")
        }
    }
}
```

Bạn có thể hiểu:

- `Column` quản lý bố cục tổng theo chiều dọc
- `Row` nhóm metadata nằm ngang

## Kết hợp `Box` với căn chỉnh

```kotlin
@Composable
fun LoadingOverlay(isLoading: Boolean) {
    Box {
        Text("Main content")
        if (isLoading) {
            CircularProgressIndicator()
        }
    }
}
```

Khi đi sâu hơn, bạn sẽ kết hợp `Box` với `Modifier.align()` để đặt phần tử chính xác hơn.

## Best practices

- Bắt đầu bằng các layout đơn giản nhất có thể.
- Đừng tạo cây layout quá sâu nếu không cần.
- Dùng `Column` và `Row` rõ vai trò thay vì nhét mọi thứ vào một `Box`.
- Tách composable nhỏ khi bố cục bắt đầu rối.

## Điều cần tránh

- Dùng sai layout chỉ vì “nó vẫn hiển thị được”.
- Tạo quá nhiều container lồng nhau không cần thiết.
- Không kiểm soát spacing và alignment, làm UI khó đọc.

## Checklist tự kiểm tra

1. Bạn có phân biệt được `Column`, `Row`, `Box` chưa?
2. Bạn có biết khi nào dùng `Arrangement` và khi nào dùng `Alignment` không?
3. Bạn có thể dựng một task item đơn giản bằng ba layout này không?

## Bài tiếp theo

Sau layout, bạn cần nắm `Modifier`, vì gần như mọi composable Compose đều được điều khiển mạnh bằng modifier.
