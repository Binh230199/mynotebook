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

## Lấy version ở đâu để điền cho đúng?

Nguyên tắc quan trọng: không tự đoán version. Hãy lấy từ nguồn đã được xác nhận tương thích.

Bạn có thể lấy version theo thứ tự ưu tiên sau:

1. **Template project mới từ Android Studio (khuyến nghị nhất)**
    - Tạo project `Empty Activity` có bật Compose.
    - Mở các file build của project mới đó để copy stack sang project hiện tại.
    - Lý do: đây là stack mà chính Android Studio đang generate và thường tương thích tốt.

2. **Release notes chính thức**
    - AGP: https://developer.android.com/build/releases/gradle-plugin
    - Compose setup: https://developer.android.com/jetpack/compose/setup
    - Kotlin releases: https://kotlinlang.org/docs/releases.html
    - Gradle compatibility: https://docs.gradle.org/current/userguide/compatibility.html
    - Dùng khi bạn muốn nâng cấp chủ động hoặc cần verify lại stack.

3. **Project mẫu trong team đang build xanh**
    - Nếu team đã có project Compose chạy ổn, dùng nó làm baseline.
    - Chỉ cần đảm bảo minSdk/targetSdk và yêu cầu nghiệp vụ của bạn phù hợp.

## Tra version theo thứ tự nào?

Đừng nâng tất cả cùng lúc. Đi theo thứ tự này để tránh lỗi dây chuyền:

1. Chốt **AGP** trước.
2. Từ AGP, xác nhận **Gradle wrapper** và **JDK** tương thích.
3. Chốt **Kotlin** tương thích với AGP.
4. Nếu dùng Kotlin 2.x, bật **Compose compiler plugin** phù hợp.
5. Chọn **Compose BOM**.
6. Chọn các thư viện ngoài BOM (ví dụ `activity-compose`).

Nếu làm ngược (ví dụ chọn BOM hoặc Kotlin trước), bạn rất dễ gặp lỗi sync/build khó hiểu.

## Điền `libs.versions.toml` như thế nào?

## Hiểu rõ `versions`, `libraries`, `plugins` trong Version Catalog

Trong `libs.versions.toml`, bạn có thể hiểu nhanh như sau:

- `[versions]`: nơi đặt biến version dùng lại nhiều nơi.
- `[libraries]`: nơi định nghĩa dependency coordinates (`group`, `name`, `version` hoặc `version.ref`).
- `[plugins]`: nơi định nghĩa plugin id và version để dùng bằng alias trong block `plugins {}`.

Ví dụ:

```toml
[versions]
agp = "9.0.1"

[plugins]
android-application = { id = "com.android.application", version.ref = "agp" }

[libraries]
androidx-core-ktx = { group = "androidx.core", name = "core-ktx", version = "1.18.0" }
```

Ý nghĩa:

- `agp` là biến version.
- plugin alias `android-application` tham chiếu tới `agp`.
- library alias `androidx-core-ktx` map tới artifact thật `androidx.core:core-ktx:1.18.0`.

## Quy tắc map alias thành `libs...` trong build script

Đây là quy tắc dùng rất thường xuyên.

Bạn có thể đặt alias theo tên bất kỳ (ví dụ `dog-cat`) miễn là hợp lệ và bạn dùng nhất quán.
Tuy nhiên, nên đặt tên có nghĩa để dễ đọc và dễ bảo trì.

### 1) Với `[plugins]`

Nếu khai báo:

```toml
[plugins]
android-application = { id = "com.android.application", version.ref = "agp" }
compose-compiler = { id = "org.jetbrains.kotlin.plugin.compose", version.ref = "kotlin" }
```

Thì ở `build.gradle.kts` sẽ dùng:

```kotlin
plugins {
    alias(libs.plugins.android.application)
    alias(libs.plugins.compose.compiler)
}
```

Quy tắc map:

1. Bắt đầu bằng `libs.plugins`.
2. Alias trong TOML dùng các dấu phân tách như `-`, `_`, `.` sẽ được map thành các segment accessor trong Kotlin DSL.
3. Khi viết trong build script, bạn truy cập theo dạng chấm `.` giữa các segment.
4. Vì vậy:
   - `android-application` -> `libs.plugins.android.application`
   - `compose-compiler` -> `libs.plugins.compose.compiler`
    - `dog-cat` -> `libs.plugins.dog.cat`

### 2) Với `[libraries]`

Nếu khai báo:

```toml
[libraries]
androidx-compose-ui-tooling-preview = { group = "androidx.compose.ui", name = "ui-tooling-preview" }
androidx-compose-bom = { group = "androidx.compose", name = "compose-bom", version.ref = "composeBom" }
```

Thì ở `dependencies` dùng:

```kotlin
dependencies {
    implementation(libs.androidx.compose.ui.tooling.preview)
    implementation(platform(libs.androidx.compose.bom))
}
```

Quy tắc map:

1. Bắt đầu bằng `libs` (không có `.plugins`).
2. Alias trong TOML dùng `-`, `_`, `.` đều map thành accessor theo từng segment.
3. Trong build script, truy cập bằng dấu chấm `.`.
4. Vì vậy:
   - `androidx-compose-ui-tooling-preview` -> `libs.androidx.compose.ui.tooling.preview`
   - `androidx-compose-bom` -> `libs.androidx.compose.bom`
    - `dog-cat` -> `libs.dog.cat`

### 3) Với `[versions]`

`[versions]` thường không gọi trực tiếp trong `dependencies` hay `plugins` block.
Nó được tham chiếu gián tiếp qua `version.ref` trong `[libraries]` và `[plugins]`.

Ví dụ:

```toml
[versions]
kotlin = "2.2.0"

[plugins]
kotlin-android = { id = "org.jetbrains.kotlin.android", version.ref = "kotlin" }
```

Ở build script bạn gọi plugin alias, không gọi `versions.kotlin` trực tiếp.

### 4) Với BOM (Compose BOM)

BOM là "Bill of Materials". BOM không phải UI library để hiển thị màn hình, mà là dependency đặc biệt để quản lý phiên bản cho một nhóm artifact.

Khi dùng BOM trong Compose:

```kotlin
dependencies {
    implementation(platform(libs.androidx.compose.bom))
    implementation(libs.androidx.compose.ui)
    implementation(libs.androidx.compose.material3)
    implementation(libs.androidx.compose.ui.tooling.preview)
}
```

Ý nghĩa:

1. `platform(libs.androidx.compose.bom)` chốt "line version" cho các artifact Compose nằm trong BOM.
2. Các artifact thuộc BOM (`ui`, `material3`, `ui-tooling-preview`, `ui-tooling`) không cần ghi version rời trong `[libraries]`.
3. Artifact ngoài BOM (ví dụ `androidx.activity:activity-compose`) vẫn cần `version` hoặc `version.ref` riêng.

Quy tắc thực hành:

1. Ưu tiên dùng BOM cho nhóm Compose UI để tránh lệch version giữa các artifact.
2. Không trộn vừa BOM vừa pin version rời cho cùng artifact Compose, trừ khi bạn biết rõ lý do compatibility.
3. Khi nâng cấp Compose, thường nâng `composeBom` trước rồi kiểm tra sync/build/preview.

Mục tiêu của Version Catalog là mọi module dùng chung một nguồn alias, tránh hardcode version rải rác.

### Bước 1: Khai báo `versions`

```toml
[versions]
agp = "..."
kotlin = "..."
composeBom = "..."
activityCompose = "..."
```

- `agp`, `kotlin`: lấy từ template Android Studio hoặc release notes.
- `composeBom`: lấy từ tài liệu Compose hoặc template project mới.
- `activityCompose`: là artifact ngoài BOM, cần version riêng.

### Bước 2: Khai báo `plugins`

```toml
[plugins]
android-application = { id = "com.android.application", version.ref = "agp" }
kotlin-android = { id = "org.jetbrains.kotlin.android", version.ref = "kotlin" }
compose-compiler = { id = "org.jetbrains.kotlin.plugin.compose", version.ref = "kotlin" }
```

Ý chính: với stack Kotlin 2.x, plugin Compose compiler thường đi theo line Kotlin.

### Bước 3: Khai báo `libraries`

```toml
[libraries]
androidx-compose-bom = { group = "androidx.compose", name = "compose-bom", version.ref = "composeBom" }
androidx-compose-ui = { group = "androidx.compose.ui", name = "ui" }
androidx-compose-ui-tooling-preview = { group = "androidx.compose.ui", name = "ui-tooling-preview" }
androidx-compose-material3 = { group = "androidx.compose.material3", name = "material3" }
androidx-compose-ui-tooling = { group = "androidx.compose.ui", name = "ui-tooling" }
androidx-activity-compose = { group = "androidx.activity", name = "activity-compose", version.ref = "activityCompose" }
```

Lưu ý quan trọng:

- Khi đã dùng BOM, **không pin version rời** cho `ui`, `material3`, `ui-tooling-preview`, `ui-tooling`.
- Chỉ các library ngoài BOM mới cần version riêng.

### Bước 4: Sử dụng đúng ở `app/build.gradle.kts`

```kotlin
plugins {
     alias(libs.plugins.android.application)
     alias(libs.plugins.kotlin.android)
     alias(libs.plugins.compose.compiler)
}

dependencies {
     implementation(platform(libs.androidx.compose.bom))
     implementation(libs.androidx.compose.ui)
     implementation(libs.androidx.compose.ui.tooling.preview)
     implementation(libs.androidx.compose.material3)
     implementation(libs.androidx.activity.compose)
     debugImplementation(libs.androidx.compose.ui.tooling)
}
```

## Cách kiểm tra bạn điền đúng chưa

1. Sync project không lỗi unresolved alias trong version catalog.
2. `assembleDebug` chạy được.
3. Có thể import `@Composable` và `setContent`.
4. Preview mở được, không báo thiếu dependency Compose cơ bản.
5. Không có cảnh báo mismatch rõ ràng giữa Kotlin và Compose compiler plugin.

Nếu fail, quay lại kiểm tra lại đúng thứ tự: AGP -> Gradle/JDK -> Kotlin -> Compose plugin -> BOM.

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
