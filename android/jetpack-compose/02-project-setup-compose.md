# 02. Cấu hình project cho Jetpack Compose

## Mục tiêu

Sau bài này, bạn sẽ hiểu:

- một project Android cần những gì để chạy Compose
- vai trò của Kotlin, AGP, Compose BOM và Material 3 trong project
- nên kiểm tra những file cấu hình nào
- cách tự xác nhận project Compose đã setup đúng chưa

## Compose cần gì ở tầng build?

Để dùng Jetpack Compose, project Android của bạn thường cần các thành phần sau:

- Android Gradle Plugin tương thích
- Kotlin tương thích
- Compose compiler plugin hoặc Compose setup phù hợp với stack hiện tại
- `buildFeatures { compose = true }`
- dependency cho Compose UI, tooling, Material 3, activity-compose

Điều quan trọng là Compose không chỉ là một thư viện UI. Nó còn liên quan đến compiler plugin và build configuration.

## Những file cần nhìn đầu tiên

Khi kiểm tra một project Compose, hãy nhìn các file sau:

- `build.gradle.kts` ở root
- `app/build.gradle.kts`
- `gradle/libs.versions.toml`
- `gradle.properties`

Nếu project quản lý version bằng version catalog, `libs.versions.toml` thường là nơi quan trọng nhất để hiểu stack.

## Cấu hình tối thiểu ở module app

Ví dụ cấu hình cơ bản:

```kotlin
android {
    buildFeatures {
        compose = true
    }
}
```

Nếu project hiện đại dùng Kotlin 2.x và stack mới, project có thể cần plugin Compose riêng ở phần `plugins`.

Ví dụ:

```kotlin
plugins {
    alias(libs.plugins.android.application)
    alias(libs.plugins.compose.compiler)
}
```

## Các dependency thường gặp

Một app Compose cơ bản thường cần:

```kotlin
dependencies {
    implementation(platform(libs.androidx.compose.bom))
    implementation(libs.androidx.compose.ui)
    implementation(libs.androidx.compose.ui.tooling.preview)
    implementation(libs.androidx.compose.material3)
    implementation(libs.androidx.activity.compose)
    debugImplementation(libs.androidx.compose.ui.tooling)
}
```

Giải thích ngắn:

- Compose BOM giúp đồng bộ version cho nhiều artifact Compose
- `ui` là core UI
- `ui-tooling-preview` hỗ trợ preview
- `material3` là component system hiện đại
- `activity-compose` kết nối Compose với `ComponentActivity`
- `ui-tooling` dùng nhiều cho debug và preview

## Cấu hình trong Android Studio

Sau khi sửa build files, bạn thường cần:

1. Sync project
2. Kiểm tra Build Output nếu có lỗi
3. Chạy `assembleDebug`
4. Kiểm tra Preview có hoạt động không

Nếu project không sync được, hãy quay lại các tài liệu build stack, AGP, Kotlin, Compose setup mà bạn đã có trong bộ docs trước.

## Cách xác nhận setup đúng

Một project Compose tối thiểu đã setup đúng thường có các dấu hiệu sau:

- build sync thành công
- trong app có thể gọi `setContent { ... }`
- có thể import `@Composable`
- Preview không báo lỗi dependency cơ bản
- app chạy được trên emulator hoặc device

## Ví dụ khung MainActivity

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            Text("Hello Compose")
        }
    }
}
```

Nếu `setContent` và `Text` dùng được ổn định, nghĩa là project đã có nền móng Compose cơ bản.

## Best practices

- Dùng version catalog để quản lý stack Compose.
- Dùng BOM thay vì pin version rời cho từng artifact Compose khi phù hợp.
- Sync và build ngay sau khi thay đổi cấu hình.
- Ghi chú rõ nếu project cần compatibility flag đặc biệt.

## Điều cần tránh

- Hardcode version rải rác ở nhiều file.
- Chỉ thêm dependency mà quên bật `compose = true`.
- Nâng Kotlin, AGP và Compose cùng lúc mà không kiểm tra compatibility.
- Coi preview lỗi là chuyện nhỏ; preview lỗi thường báo setup chưa ổn ở đâu đó.

## Checklist tự kiểm tra

1. Bạn có biết project bật Compose ở đâu không?
2. Bạn có biết dependency Compose cơ bản nằm ở đâu không?
3. Bạn có hiểu BOM dùng để làm gì không?
4. Bạn có thể tự kiểm tra project Compose đã setup đúng chưa không?

## Bài tiếp theo

Bước tiếp theo là viết composable đầu tiên và hiểu Preview hoạt động như thế nào.
