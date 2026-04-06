# 10. Scaffold và cấu trúc màn hình Material

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- `Scaffold` là gì
- vì sao `Scaffold` giúp tổ chức màn hình tốt hơn
- cách kết hợp top bar, floating action button, snackbar host và content
- khi nào nên dùng `Scaffold`

## Vì sao cần `Scaffold`?

Khi màn hình bắt đầu có cấu trúc Material rõ ràng, bạn thường cần các vùng như:

- top app bar
- bottom bar
- floating action button
- snackbar
- phần nội dung chính

Nếu tự ráp từng phần bằng `Column`, `Box` và `Modifier`, bạn vẫn có thể làm được. Nhưng `Scaffold` giúp bạn tổ chức bố cục màn hình chuẩn hơn và dễ quản lý hơn.

## Ví dụ cơ bản

```kotlin
@Composable
fun HomeScreen() {
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Tasks") }
            )
        },
        floatingActionButton = {
            FloatingActionButton(onClick = { }) {
                Text("+")
            }
        }
    ) { innerPadding ->
        Column(modifier = Modifier.padding(innerPadding)) {
            Text("Screen content")
        }
    }
}
```

## `innerPadding` là gì?

Đây là một điểm rất quan trọng.

`Scaffold` cung cấp `innerPadding` để phần content không bị đè lên các vùng như top bar hoặc bottom bar.

Nếu bạn quên áp `innerPadding`, nội dung có thể bị trùng lấp hoặc hiển thị không đúng.

## Khi nào nên dùng `Scaffold`?

Dùng `Scaffold` khi màn hình thật sự có cấu trúc cấp màn hình, ví dụ:

- màn hình chính với top app bar
- màn hình có floating action button
- màn hình cần snackbar host
- layout kiểu Material screen hoàn chỉnh

Không nhất thiết mọi composable nhỏ đều phải có `Scaffold`.

## Ví dụ có snackbar host

```kotlin
@Composable
fun MessageScreen() {
    val snackbarHostState = remember { SnackbarHostState() }

    Scaffold(
        snackbarHost = {
            SnackbarHost(hostState = snackbarHostState)
        }
    ) { innerPadding ->
        Column(modifier = Modifier.padding(innerPadding)) {
            Text("Hello")
        }
    }
}
```

Khi đi xa hơn, snackbar thường được kích hoạt từ event hoặc side effect chứ không gọi ngẫu nhiên trong body composable.

## Best practices

- Dùng `Scaffold` cho screen-level layout, không phải cho mọi UI nhỏ.
- Luôn xử lý `innerPadding` đúng cách.
- Xem `Scaffold` như khung màn hình, không phải nơi chứa mọi logic phức tạp.

## Điều cần tránh

- Quên dùng `innerPadding`.
- Nhét tất cả UI của mọi cấp độ vào một `Scaffold` duy nhất quá lớn.
- Gọi side effect ngẫu nhiên chỉ vì đang có snackbar host hoặc FAB trong `Scaffold`.

## Checklist tự kiểm tra

1. Bạn có hiểu `Scaffold` dùng cho cấp màn hình không?
2. Bạn có biết vì sao `innerPadding` quan trọng không?
3. Bạn có thể tự dựng một màn hình có top bar và FAB bằng `Scaffold` không?

## Bài tiếp theo

Sau khi có cấu trúc màn hình, bạn cần một visual language nhất quán: theme, color, typography và design system cơ bản trong Material 3.
