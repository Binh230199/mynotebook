# 16. Navigation Compose cơ bản

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- Navigation Compose giải quyết bài toán gì
- `NavController` là gì
- `NavHost` là gì
- cách tổ chức route cơ bản
- cách điều hướng giữa các màn hình

## Vì sao cần navigation?

App thực tế không chỉ có một màn hình. Bạn cần chuyển giữa:

- màn hình danh sách
- màn hình chi tiết
- màn hình cài đặt
- màn hình thêm mới

Navigation Compose là cách phổ biến để điều hướng trong app Compose.

## Thành phần chính

### `NavController`

Đây là đối tượng điều phối điều hướng.

### `NavHost`

Đây là nơi khai báo graph màn hình và route nào sẽ render composable nào.

### `composable(route = ...)`

Đây là node mô tả một destination trong graph.

## Ví dụ đơn giản

```kotlin
@Composable
fun AppNavigation() {
    val navController = rememberNavController()

    NavHost(
        navController = navController,
        startDestination = "home"
    ) {
        composable("home") {
            HomeScreen(
                onOpenDetail = {
                    navController.navigate("detail")
                }
            )
        }

        composable("detail") {
            DetailScreen(
                onBack = { navController.popBackStack() }
            )
        }
    }
}
```

## Ý tưởng quan trọng

Navigation không nên bị rải khắp mọi nơi. Bạn nên giữ điều hướng ở mức screen hoặc route rõ ràng.

UI component nhỏ như `TaskRow` không nên tự cầm `NavController` nếu không cần thiết. Nó chỉ nên phát event như `onClick`.

## Route string

Ban đầu, nhiều ví dụ dùng chuỗi như `"home"`, `"detail"`. Cách này dễ hiểu cho người mới. Nhưng khi app lớn, bạn nên có cấu trúc route rõ hơn để tránh hardcode lung tung.

Ví dụ:

```kotlin
object AppRoute {
    const val HOME = "home"
    const val DETAIL = "detail"
}
```

## Điều hướng đi và về

### Đi tới màn hình mới

```kotlin
navController.navigate("detail")
```

### Quay lại

```kotlin
navController.popBackStack()
```

## Tách navigation ra khỏi screen UI

Một pattern tốt là:

- `TaskScreen` nhận callback `onTaskClick`
- route hoặc navigation host quyết định callback đó sẽ `navigate(...)`

Điều này giữ UI composable độc lập hơn với framework navigation.

## Best practices

- Giữ navigation ở tầng screen/route, không đẩy sâu xuống component nhỏ.
- Đặt tên route rõ ràng, nhất quán.
- Dùng callback từ UI lên thay vì để mọi composable cầm `NavController`.

## Điều cần tránh

- Hardcode route tràn lan khắp codebase.
- Component UI nhỏ điều hướng trực tiếp nếu chỉ cần phát event.
- Trộn navigation logic với rendering logic quá chặt.

## Checklist tự kiểm tra

1. Bạn có hiểu `NavController` và `NavHost` làm gì không?
2. Bạn có thể tự dựng navigation giữa 2 màn hình chưa?
3. Bạn có hiểu vì sao nên giữ navigation ở tầng route hoặc screen không?

## Bài tiếp theo

Navigation thật sự hữu ích khi bạn cần truyền tham số, chia graph và tổ chức flow lớn hơn. Đó là nội dung bài sau.
