# Cách Xây dựng Ứng dụng Android Đơn giản - Phát triển Native Compose

# 1 Ứng dụng Android và Phát triển Android Là Gì

Trong hướng dẫn này, chúng ta sẽ hoàn thành một vòng lặp khép kín: **từ một ý tưởng trong đầu đến một ứng dụng thực sự có thể được cài đặt và chạy thành công trên điện thoại Android.**

Để thực hiện hướng dẫn này, bạn cần ít nhất:

- Một máy tính có hiệu năng tốt (Windows hoặc Mac)
- Một điện thoại Android (tùy chọn; nếu không có, chúng ta sẽ sử dụng trình mô phỏng)
- Đã cài đặt Android Studio (để xây dựng)
- Đã cài đặt và đăng ký Trae (để lập trình AI)

## 1.1 Định nghĩa Ứng dụng Android

Ứng dụng Android là một ứng dụng native chạy trên hệ điều hành Android. Khác với mini program, nó không phụ thuộc vào nền tảng chủ như WeChat. Nó chạy trực tiếp ở cấp hệ thống, có biểu tượng riêng trên màn hình chính, khởi động nhanh, hoạt động mượt mà và có thể truy cập sâu vào các tính năng cấp hệ thống như Bluetooth, cảm biến và dịch vụ chạy nền.

![](/zh-cn/stage-3/cross-platform/android-app/images/image1.png)

## 1.2 Phát triển Ứng dụng Android

Phát triển Android là toàn bộ quá trình xây dựng các ứng dụng như vậy. Trong chế độ phát triển Vibe Coding được sử dụng trong hướng dẫn này, với **lập trình hỗ trợ AI**, vai trò của nhà phát triển chuyển từ "người viết code" sang "kiến trúc sư sản phẩm":

1. **Bạn (kiến trúc sư / PM)**: chịu trách nhiệm thiết kế logic nghiệp vụ, viết prompt và kiểm duyệt kết quả cuối cùng.
2. **Trae (kỹ sư AI)**: chịu trách nhiệm thực thi hướng dẫn, chuyển đổi ngôn ngữ tự nhiên thành code Kotlin chuẩn và bố cục Jetpack Compose, xử lý lỗi cú pháp và chi tiết logic.
3. **Android Studio (nhà máy xây dựng)**: chịu trách nhiệm cung cấp môi trường biên dịch, đóng gói code thành ứng dụng có thể chạy và cung cấp xem trước trên trình mô phỏng.

## 1.3 Các Phương pháp Phổ biến để Xây dựng Ứng dụng Android

Trong phát triển thực tế, có nhiều hơn một cách để xây dựng ứng dụng Android. Chúng ta sẽ không đi sâu ở đây, mà chỉ cung cấp cái nhìn tổng quan.

**Cách thứ nhất: Phát triển Native**
Đây là lộ trình chính thức được Google khuyến nghị. Bạn trực tiếp sử dụng **Kotlin** và **Jetpack Compose** để phát triển. Ưu điểm là hiệu năng tốt nhất và truy cập đầy đủ phần cứng điện thoại.

![](/zh-cn/stage-3/cross-platform/android-app/images/image2.png)![](/zh-cn/stage-3/cross-platform/android-app/images/image3.png)

**Cách thứ hai: Phát triển Cross-Platform**
Ví dụ Flutter hoặc React Native. Ý tưởng cốt lõi là "viết một codebase và tạo ra cả ứng dụng Android và iOS."

**Cách thứ ba: Phát triển Hybrid**
Về bản chất, đây là bọc một trang web bên trong vỏ ứng dụng. Cách này phát triển nhanh, nhưng trải nghiệm và độ mượt thường không tốt bằng ứng dụng native, và khó tạo ra một công cụ nhỏ tinh tế và sống động.

**Lựa chọn của hướng dẫn này: phát triển native (** **Kotlin + Compose)** kết hợp với công cụ AI để lập trình.
Lý do rất đơn giản: code Jetpack Compose native có cấu trúc rất rõ ràng và cực kỳ phù hợp để AI hiểu và tạo ra. Chúng ta không cần tự viết code từ đầu. Thay vào đó, chúng ta hướng dẫn Trae bằng ngôn ngữ tự nhiên để tạo ra code native chất lượng cao.

![](/zh-cn/stage-3/cross-platform/android-app/images/image4.png)

## 1.4 Các Bước Phát triển Ứng dụng Android được Trình bày trong Hướng dẫn này

Để quá trình học tập thú vị hơn, hướng dẫn này xoay quanh một case thư giãn nhưng mang tính biểu diễn về mặt kỹ thuật - **Mộc Ngư Điện tử**. Chúng ta kết hợp chế độ Vibe Coding của Trae với một lộ trình bạn có thể tái sử dụng nhiều lần:

1. **Xây dựng hiểu biết và môi trường**: hiểu ứng dụng Android là gì, cài đặt Android Studio và Trae, cấu hình mirror phù hợp với Trung Quốc để toolchain hoạt động trơn tru.
2. **Xây dựng khung dự án**: tạo một dự án Android trắng có thể chạy thành công trên trình mô phỏng.
3. **Phát triển lặp với AI**: mở dự án trong Trae, sau đó thông qua trò chuyện với AI, dần dần triển khai hình ảnh mộc ngư, hoạt ảnh chạm, hiệu ứng âm thanh, văn bản nổi và hơn thế nữa.
4. **Gỡ lỗi và tinh chỉnh trên thiết bị thực**: vượt ra khỏi trình mô phỏng, cài đặt ứng dụng trên điện thoại thực, trải nghiệm phản hồi rung thực sự và để AI giúp điều tra lỗi.
5. **Đóng gói và xuất bản**: tạo APK chính thức và hiểu cách chia sẻ hoặc phát hành nó.

Phần này chỉ vẽ bức tranh tổng thể và chưa mở rộng tất cả các lệnh. Bây giờ, chỉ cần nhớ dòng chính: **Thiết lập môi trường -> Xây dựng khung -> Mô tả và tạo bằng AI -> Tinh chỉnh trên thiết bị thực -> Đóng gói và giao hàng**. Trong các chương tiếp theo, chúng ta sẽ hướng dẫn bạn qua từng bước.

# 2 Thiết lập Môi trường Phát triển

## 2.1 Các Công cụ được Sử dụng trong Hướng dẫn này

Trong suốt quá trình phát triển, chúng ta sử dụng ba công cụ cùng nhau, đóng các vai trò "thiết kế," "xây dựng," và "kiểm duyệt."

- **Trae**: đây là **đối tác lập trình AI** của bạn. Trong chế độ Vibe Coding, chúng ta không cần phải gõ code từng dòng. Thay vào đó, chúng ta chủ yếu nói với AI bằng ngôn ngữ tự nhiên những gì mình muốn và nó xử lý việc tạo và sửa đổi code.
- **Android Studio**: đây là **nhà máy xây dựng ứng dụng** chính thức của Google. Mặc dù có nhiều nút, trong hướng dẫn này chúng ta chủ yếu sử dụng nó để tạo khung dự án và biên dịch code do Trae tạo thành thứ gì đó có thể cài đặt trên điện thoại.
- **Một thiết bị Android**: đóng vai trò là **thiết bị kiểm tra** để xem kết quả. Bạn có thể kết nối nó với máy tính để gỡ lỗi trên thiết bị thực và cảm nhận phản hồi rung thực sự. Nếu không có, **Trình mô phỏng** tích hợp sẵn của Android Studio có thể mô phỏng hoàn hảo một điện thoại ảo, đủ cho giai đoạn phát triển ban đầu.

## 2.2 Tải xuống Trae

Trae là chiến trường chính của chúng ta cho **Vibe Coding**. Bạn có thể hiểu đơn giản nó là một **"trình soạn thảo code được hỗ trợ bởi AI."**

Truy cập trang web chính thức [https://www.trae.cn](https://www.trae.cn), tải xuống phiên bản phù hợp với hệ thống máy tính của bạn (Windows hoặc Mac) và cài đặt nó giống như phần mềm thông thường bằng cách nhấp đúp vào trình cài đặt và làm theo hướng dẫn. Khi công cụ này sẵn sàng, trong thực hành sau này chúng ta sẽ ngừng nhìn vào các cửa sổ code nhàm chán và thay vào đó mở dự án tại đây và yêu cầu AI xây dựng bằng ngôn ngữ tự nhiên.

![](/zh-cn/stage-3/cross-platform/android-app/images/image5.png)

## 2.3 Tải xuống Android Studio

Chúng ta cần Android Studio để cung cấp Android SDK và trình mô phỏng cần thiết để chạy ứng dụng. Truy cập trang tải xuống chính thức [https://developer.android.com/studio?hl=zh-cn](https://developer.android.com/studio?hl=zh-cn) và tải xuống gói cho hệ điều hành của bạn (hướng dẫn này dựa trên **2025.2.3**). Sau khi tải xuống, cài đặt như phần mềm bình thường, giữ các tùy chọn mặc định xuyên suốt.

**Lưu ý đặc biệt cho người mới bắt đầu:**

Mặc dù các phiên bản Android Studio hiện đại đã đơn giản hóa cấu hình đáng kể, nhưng nó vẫn phụ thuộc vào **JDK (Java Development Kit)** bên dưới. Nếu đây là lần đầu tiên bạn làm phát triển, hoặc nếu bạn gặp lỗi liên quan đến biến môi trường hoặc cấu hình SDK trong quá trình cài đặt, đừng hoảng sợ. Bạn có thể tham khảo hướng dẫn cài đặt chi tiết này: [Cài đặt Android Studio 2024: cấu hình SDK và Gradle](https://blog.csdn.net/keiraee/article/details/142321644?ops_request_misc=elastic_search_misc&request_id=a2b858d1f665095c53afa9114ad8864d&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-2-142321644-null-null.142^v102^pc_search_result_base4&utm_term=android%20studio%E5%AE%89%E8%A3%85%E5%8F%8A%E9%85%8D%E7%BD%AE&spm=1018.2226.3001.4187)

![](/zh-cn/stage-3/cross-platform/android-app/images/image6.png)

## 2.4 Tạo Dự án Mới

Mở Android Studio và nhấp vào **New Project** trên màn hình chào mừng.

**Bước 1: Chọn template**

Trong danh sách template, chọn **Empty Activity** (chú ý biểu tượng Jetpack Compose trên đó).

![](/zh-cn/stage-3/cross-platform/android-app/images/image7.png)![](/zh-cn/stage-3/cross-platform/android-app/images/image8.png)

**Bước 2: Điền thông tin cấu hình dự án**

Sau đó bạn sẽ thấy một biểu mẫu cấu hình. Điền roughly như sau và giữ phần còn lại ở mặc định:

| **Trường** | **Giá trị Khuyến nghị** | **Giải thích** |
| ----------------- | -------------------------------------------------- | ---------------------------------------- |
| **Name** | My Application 1 | Tên ứng dụng hiển thị trên màn hình chính điện thoại |
| **Package name** | com.example.myapplication1 | Định danh ứng dụng duy nhất |
| **Save location** | Đường dẫn tùy chỉnh (ví dụ `E:\AndroidProjects\Myapplication1`) | Vị trí lưu trữ dự án; không khuyến nghị đặt trên ổ C |
| **Minimum SDK** | API 30 | Phủ sóng hơn 90% thiết bị đang hoạt động trong khi cân bằng khả năng tương thích và tính năng |
| **Language** | Kotlin (khuyến nghị) | Kotlin là ngôn ngữ được Google khuyến nghị chính thức, gọn gàng và an toàn hơn |

![](/zh-cn/stage-3/cross-platform/android-app/images/image9.png)

**Bước 3: Đợi xây dựng dự án**

Nhấp **Finish**. Android Studio sẽ tự động tải xuống các dependency và xây dựng dự án (bạn sẽ thấy thanh tiến trình ở góc dưới bên phải).

- _Lưu ý: lần tạo dự án đầu tiên có thể mất vài phút. Đợi kiên nhẫn cho đến khi tiến trình ở dưới cùng hoàn tất và cây tệp dự án được tải đầy đủ ở bên trái._

## 2.5 Cấu hình Dependency: Tải xuống Gradle và Mirror Kho lưu trữ Gradle

> Đây là một trong số ít các bước trong quy trình Vibe Coding mà **thao tác thủ công** được khuyến nghị. Mặc dù AI cũng có thể giúp sửa đổi cấu hình, nhưng cấu hình môi trường liên quan đến các tệp cấp thấp, nên thay đổi thủ công đáng tin cậy hơn.

Tại sao cần sửa đổi cấu hình?

Theo mặc định, Android Studio kết nối đến máy chủ nước ngoài, do đó việc tải xuống công cụ xây dựng và các dependency có thể mất một giờ hoặc thậm chí thất bại. Sau khi chuyển sang mirror trong nước, thường hoàn thành trong vài phút. **Đây là nhiệm vụ một lần mang lại lợi ích mãi mãi.**

1. **Chuẩn bị**

Nếu thanh trạng thái ở góc dưới bên phải của Android Studio đang hiển thị thanh tiến trình như `Gradle Building...`, hãy tạm dừng việc tải dependency đang diễn ra để tránh xung đột tệp.

![](/zh-cn/stage-3/cross-platform/android-app/images/image10.png)

2. **Tăng tốc tải xuống Gradle**

Trong cây tệp dự án ở bên trái, mở rộng `gradle` -> `wrapper`, sau đó mở `gradle-wrapper.properties`. Thay đổi nguồn tải xuống thành mirror của Tencent:

```text
distributionUrl=https\://mirrors.cloud.tencent.com/gradle/gradle-8.7-bin.zip
```

Lưu ý: bạn chỉ cần thay thế `services.gradle.org/distributions` bằng `mirrors.cloud.tencent.com/gradle`. Không thay đổi bất kỳ thứ gì khác.

![](/zh-cn/stage-3/cross-platform/android-app/images/image11.png)

3. **Tăng tốc tải xuống kho lưu trữ dependency**

Sau đó, mở `settings.gradle.kts` trong thư mục gốc dự án và thay thế nội dung bên trong khối `repositories` bằng như sau:

![](/zh-cn/stage-3/cross-platform/android-app/images/image12.png)

Thay thế phần được đánh dấu bằng code sau (danh sách nguồn mới nhất tính đến 2025-02-21):

```json
        // Aliyun mirrors (bao phủ Maven Central, Google, JCenter, v.v.)
        maven { setUrl("https://maven.aliyun.com/repository/public/") }
        maven { setUrl("https://maven.aliyun.com/repository/google/") }
        maven { setUrl("https://maven.aliyun.com/repository/jcenter/") }
        maven { setUrl("https://maven.aliyun.com/repository/gradle-plugin/") }
        // Huawei Cloud mirror
        maven { setUrl("https://repo.huaweicloud.com/repository/maven/") }
        // Tencent Cloud mirror
        maven { setUrl("https://mirrors.cloud.tencent.com/nexus/repository/maven-public/") }
        // NetEase mirror
        maven { setUrl("https://mirrors.163.com/maven/repository/maven-public/") }
```

Nó sẽ trông như ảnh chụp màn hình dưới đây:

![](/zh-cn/stage-3/cross-platform/android-app/images/image13.png)

4. **Lưu và áp dụng thay đổi**

Lúc này, lưu tệp và nhấp `Try Again` ở góc trên bên phải. Android Studio sẽ chạy lại việc tải xuống. Đợi vài phút. Khi bảng điều khiển hiển thị `BUILD SUCCESSFUL`, điều đó có nghĩa là thiết lập môi trường đã hoàn tất đầy đủ và chúng ta đã sẵn sàng bắt đầu lập trình.

![](/zh-cn/stage-3/cross-platform/android-app/images/image14.png)

## 2.6 Hiểu Cấu trúc Dự án

Sau khi tạo dự án thành công, bảng **Project** sẽ xuất hiện ở bên trái. Chuyển sang chế độ xem **Android** (mặc định) và bạn sẽ thấy các thư mục chính như sau:

```text
app/
├── manifests/
│   └── AndroidManifest.xml            <- "CMND" của ứng dụng, khai báo tên ứng dụng và Activity đầu vào (MainActivity)
│
├── java/
│   └── com.example.myapplication1/
│       ├── MainActivity.kt            <- đầu vào ứng dụng, xây dựng UI bằng Jetpack Compose
│       │
│       └── ui/                        <- kiểm soát phong cách UI tổng thể (màu sắc, phông chữ)
├── res/
│   ├── drawable/                      <- tài nguyên hình ảnh (ví dụ ic_launcher.png)
│   ├── mipmap/                        <- biểu tượng ứng dụng
│   ├── values/                        <- văn bản, màu sắc, phong cách chủ đề
│   │   ├── colors.xml
│   │   ├── strings.xml
│   │   └── themes.xml
│   └── xml/                           <- các tệp cấu hình liên quan đến hệ thống (không phải UI)
└── build.gradle (Module: app)         <- cấu hình xây dựng ứng dụng (thường không chạm ở giai đoạn người mới bắt đầu)
```

Là người mới bắt đầu, chúng ta thường chỉ cần quan tâm đến ba tệp:

- `MainActivity.kt`: kiểm soát hành vi và quyết định "những gì xuất hiện trên màn hình"
- `AndroidManifest.xml`: đăng ký các thành phần và quyết định "ứng dụng bắt đầu từ đâu"
- `Theme.kt`: định nghĩa giao diện trực quan

# 3 Phát triển Ứng dụng Android

Trong hai chương đầu tiên, chúng ta đã hiểu ứng dụng Android là gì và mài giũa hai công cụ chính: Trae và Android Studio. Từ phần này trở đi, chúng ta rời khỏi thảo luận lý thuyết và bước vào thực hành thực tế. Chúng ta sẽ áp dụng chế độ Vibe Coding để xây dựng một ứng dụng giảm căng thẳng rất phổ biến từ đầu - **Mộc Ngư Điện tử**. Nó phù hợp với chủ đề "Vibe" (đơn giản và thư giãn), đồng thời cũng bao gồm ba phần cốt lõi của phát triển Android: **Tương tác UI (chạm), lưu trữ dữ liệu (số công đức), và đa phương tiện (hiệu ứng âm thanh)**.

Bây giờ, hãy làm theo và gửi hướng dẫn đầu tiên đến AI.

## 3.1 "Master Prompt" Đầu tiên: Từ Không đến Một

Trong chế độ Vibe Coding, chúng ta không cần phải tạo tệp bố cục trước rồi mới viết code logic như trong phát triển truyền thống. Điều chúng ta cần làm là **mô tả yêu cầu rõ ràng trong một lần và để AI tạo ra nguyên mẫu có thể chạy đầu tiên**.

Mở thư mục dự án vừa tạo trong Trae và trong bảng trò chuyện ở bên phải, nhập Prompt sau:

```text
Bạn là một chuyên gia phát triển Android kỳ cựu. Vui lòng viết lại MainActivity.kt hiện tại và biến nó thành ứng dụng "Mộc Ngư Điện tử". Yêu cầu:
1. Nền màn hình màu đen.
2. Hiển thị hình ảnh mộc ngư ở trung tâm màn hình, kích thước vừa phải, màu trắng.
3. Hiển thị một dòng văn bản trắng phía trên: "Công đức: 0".
4. Khi chạm vào mộc ngư ở trung tâm, số tăng lên 1 và xuất hiện hiệu ứng hoạt ảnh phóng to đơn giản (mô phỏng cảm giác gõ).
5. Sử dụng Jetpack Compose.
```

Sau khi gửi, Trae sẽ bắt đầu phân tích cấu trúc dự án của bạn. Vài giây sau, nó sẽ trực tiếp tạo code hoàn chỉnh cho `MainActivity.kt`.

1. Từ phản hồi, chúng ta có thể thấy logic lập luận và logic tương tác của nó
2. Chúng ta có thể trực tiếp thấy phần code nào đã được viết lại
3. Nếu không hài lòng với kết quả, chúng ta có thể quay lại phiên bản trước

![](/zh-cn/stage-3/cross-platform/android-app/images/image15.png)

## 3.2 Chạy và Xem trước (Gỡ lỗi trên Trình mô phỏng)

Lúc này, AI đã hoàn thành vòng phát triển đầu tiên. Nhưng hãy nhớ rằng, những gì chúng ta thấy trong Trae chỉ là "bản thiết kế" code, không phải ứng dụng tương tác thực sự. Trae không thể trực tiếp chạy ứng dụng Android, vì vậy chúng ta cần dựa vào **Trình mô phỏng Thiết bị Ảo** do Android Studio cung cấp. Nó giống như biến màn hình máy tính thành một điện thoại Android ảo, cho phép chúng ta cài đặt code ngay lập tức và xem kết quả thực tế.

Tiếp theo, hãy cấu hình "điện thoại ảo" này.

**Bước 1: Tạo trình mô phỏng**

Quay lại Android Studio, tìm và nhấp **Device Manager** trong thanh công cụ bên phải. Nếu không tìm thấy, mở từ `View -> Tool Windows -> Device Manager`.

Trong bảng điều khiển, nhấp **Add a new device**, sau đó chọn **Create Virtual Device** để vào cửa sổ chọn thiết bị.

![](/zh-cn/stage-3/cross-platform/android-app/images/image16.png)

![](/zh-cn/stage-3/cross-platform/android-app/images/image17.png)

Trong cửa sổ chọn phần cứng, chọn **Phone** và sau đó **Smart Phone** (kích thước màn hình trung bình), hoặc bất kỳ hồ sơ thiết bị nào bạn muốn như Pixel, sau đó nhấp **Next**.

![](/zh-cn/stage-3/cross-platform/android-app/images/image18.png)

**Bước 2: Cấu hình hệ điều hành image**

Trong hộp thoại **System Image**, chọn **API 36.1**. Nếu chưa được tải xuống, nhấp **Download** trước, sau đó chọn sau khi tải xuống hoàn tất và nhấp **Finish**.

![](/zh-cn/stage-3/cross-platform/android-app/images/image19.png)

**Bước 3: Khởi động trình mô phỏng**

Sau khi tạo thành công, điện thoại mới của bạn sẽ xuất hiện trong danh sách quản lý thiết bị. Nhấp **nút phát hình tam giác** ở bên phải. Sau một khoảng chờ ngắn, một cửa sổ hình điện thoại sẽ bật lên - đây là trình mô phỏng Android của bạn.

![](/zh-cn/stage-3/cross-platform/android-app/images/image20.png)

![](/zh-cn/stage-3/cross-platform/android-app/images/image21.png)

**Bước 4: Chạy ứng dụng**

Bây giờ đến khoảnh khắc kỳ diệu. Đảm bảo trình mô phỏng đã khởi động và hiển thị màn hình chính, sau đó nhấp **nút Run màu xanh lá cây nổi bật** trong thanh công cụ trên cùng của Android Studio (hoặc sử dụng phím tắt `Shift + F10`). Android Studio sẽ tự động biên dịch code do Trae viết, đóng gói nó thành ứng dụng và cài đặt vào trình mô phỏng.

Trong vài giây, bạn sẽ thấy màn hình trình mô phỏng sáng lên, hiển thị hình ảnh mộc ngư trắng ở trung tâm với văn bản "Công đức: 0" phía trên. Thử chạm vào nó và xem liệu số có tăng lên và hoạt ảnh có hoạt động không. Đây là ứng dụng Android đầu tiên của bạn.

![](/zh-cn/stage-3/cross-platform/android-app/images/image22.png)

![](/zh-cn/stage-3/cross-platform/android-app/images/image23.png)

## 3.3 Lặp Tối ưu hóa (Thêm Asset và Âm thanh)

Ở giai đoạn này, ứng dụng của chúng ta đã có hình dạng cơ bản: chạm để tăng số. Nhưng nó vẫn chỉ là một hình học trắng "câm", thiếu thú vị. Tiếp theo, chúng ta sẽ làm cho Mộc Ngư Điện tử trở nên sống động hơn nhiều bằng cách thêm hình ảnh thực và hiệu ứng âm thanh gõ.

**Đây chính là phần hấp dẫn nhất của chế độ Vibe Coding.** Trong phát triển truyền thống, thêm hiệu ứng âm thanh và hoạt ảnh phức tạp hơn thường là ác mộng của người mới bắt đầu. Bạn cần quản lý việc tải và giải phóng tài nguyên `MediaPlayer` (nếu không có thể gây rò rỉ bộ nhớ), và cũng phải tính toán đường cong hoạt ảnh. Trong chế độ Vibe Coding, bạn hoàn toàn không cần quan tâm đến các chi tiết cấp thấp này. Bạn chỉ cần nói với AI như một đạo diễn: "đổi đạo cụ và thêm hiệu ứng âm thanh khi chạm," và triển khai xuất hiện ngay lập tức.

**Bước 1: Chuẩn bị asset**
Bạn cần một hình ảnh mộc ngư (`png`) và một hiệu ứng âm thanh gõ (`mp3`).

- **Asset hình ảnh**: sao chép `white_muyu.png` đã chuẩn bị vào `app/src/main/res/drawable`
- **Asset âm thanh**: trong Android Studio, nhấp chuột phải vào thư mục `res` trong bảng dự án bên trái, chọn `New -> Android Resource Directory`, chọn **raw** làm loại tài nguyên, nhấp OK, sau đó sao chép `voice.mp3` vào thư mục `res/raw` mới. _(Lưu ý: nếu bạn dự định phát hành thương mại, hãy đảm bảo bạn có quyền hợp pháp đối với tất cả các asset.)_

Dưới đây là asset hình ảnh và âm thanh tôi đã tìm cho bạn. Nếu bạn không tiện tự tìm, có thể sử dụng trực tiếp.

![](/zh-cn/stage-3/cross-platform/android-app/images/image24.png)

Link tải xuống âm thanh gõ: https://www.aigei.com/s?q=%E6%9C%A8%E9%B1%BC&type=sound
Chọn hiệu ứng âm thanh 1 giây đầu tiên.

![](/zh-cn/stage-3/cross-platform/android-app/images/image25.png)

**Bước 2: Gửi hướng dẫn lặp**

Sau khi asset đã sẵn sàng, quay lại Trae. Trae sẽ sửa đổi code lần nữa và xử lý logic tải âm thanh và hoạt ảnh cho bạn. Bạn chỉ cần nói với nó asset nào sẽ sử dụng. Nhập Prompt này:

```text
Tôi đã thêm các asset. Đường dẫn hình ảnh là res/drawable/white_muyu.png và đường dẫn hiệu ứng âm thanh là res/raw/voice.mp3. Vui lòng cập nhật code:
1. Thay thế biểu tượng mộc ngư ở trung tâm bằng hình ảnh của tôi.
2. Phát âm thanh gõ mỗi khi mộc ngư được chạm.
3. Khi chạm, hiển thị văn bản "+1" tạm thời phía trên mộc ngư, sau đó để nó nổi lên và biến mất (như văn bản điểm số nổi trong game).
```

![](/zh-cn/stage-3/cross-platform/android-app/images/image26.png)

**Bước 3: Xác minh kết quả**

Sau khi Trae hoàn thành sửa đổi code, quay lại Android Studio và nhấp nút Run màu xanh lá cây lần nữa (Re-run) để khởi động lại trình mô phỏng. Lúc này, ứng dụng của bạn sẽ cảm thấy được biến đổi. Thử chạm liên tục - bạn sẽ nghe thấy âm thanh "tok tok" giòn và thấy văn bản "Công đức +1" nổi lên nhảy ra. Điều này hoàn thành sự chuyển đổi quan trọng từ "demo" sang "sản phẩm."

![](/zh-cn/stage-3/cross-platform/android-app/images/image27.png)

![](/zh-cn/stage-3/cross-platform/android-app/images/image28.png)

## 3.4 Nếu Lỗi Xuất hiện Thì Sao? (Vòng lặp Gỡ lỗi với AI)

Code do AI tạo ra không được đảm bảo hoàn hảo ở lần thử đầu tiên, giống như các kỹ sư hàng đầu cũng không thể hứa code không có lỗi trong một lần. Nhưng trong chế độ Vibe Coding, lỗi không còn là bức tường chặn bạn; chúng trở thành bước đệm trong sự hợp tác giữa bạn và AI.

**Trường hợp 1: ứng dụng bị crash**

Giả sử ứng dụng crash ngay sau khi nhấp Run, hoặc chạm vào mộc ngư không phát âm thanh. Theo truyền thống, bạn sẽ cần tìm kiếm mã lỗi, duyệt hàng chục diễn đàn kỹ thuật và đọc nhiều bài đăng tiếng Anh khó hiểu. Trong chế độ Vibe Coding, bạn chỉ cần làm một việc - **trở thành người chuyển phát**.

**Các bước:**

1. **Mở nhật ký**: tìm bảng **Logcat** ở dưới cùng của Android Studio (biểu tượng con mèo nhỏ).
2. **Xác định lỗi**: bạn sẽ thấy các nhật ký cuộn, và các **dòng màu đỏ** thường là lỗi chính.
3. **Sao chép và dán**: chọn văn bản lỗi tiếng Anh màu đỏ, sao chép và dán vào Trae: "Tôi gặp lỗi này khi chạy. Vui lòng giúp tôi sửa."
4. AI có thể ngay lập tức nói với bạn điều gì đó như: "Điều này xảy ra vì quyền rung chưa được khai báo trong `AndroidManifest.xml`," và sau đó cung cấp cho bạn code đã sửa. Bạn chỉ cần nhấp Apply và tiếp tục.

**Trường hợp 2: ứng dụng chạy, nhưng trải nghiệm cảm thấy tồi**

Đôi khi ứng dụng không crash, nhưng vẫn cảm thấy không thỏa đáng. Ví dụ, khi chạm vào mộc ngư rất nhanh, bạn có thể nhận thấy rằng các hoạt ảnh "+1" mới không xuất hiện cho đến khi "+1" trước đó biến mất hoàn toàn. Điều đó làm cho phản hồi cảm thấy chậm và không thỏa mãn. Bạn không cần phải tự nghiên cứu đa luồng hoặc hàng đợi hoạt ảnh. Bạn chỉ cần mô tả rõ ràng sự khó chịu đó cho AI.

Gửi "hướng dẫn nâng cao" này cho Trae:

```text
Vui lòng sửa đổi logic hoạt ảnh hiện tại để giải quyết vấn đề "chạm nhanh không kích hoạt".
Vấn đề hiện tại: dường như chỉ có một trạng thái hoạt ảnh, vì vậy tôi phải đợi cho đến khi "+1" trước đó biến mất hoàn toàn trước khi một cú nhấp khác phản hồi.
Yêu cầu:
1. Thay thế trạng thái hoạt ảnh đơn bằng danh sách dựa trên mutableStateListOf.
2. Mỗi khi mộc ngư được chạm, ngay lập tức thêm một instance "+1" mới vào danh sách (với ID và vị trí ban đầu riêng), bất kể hoạt ảnh trước đó đã hoàn tất hay chưa.
3. Trong UI, lặp qua danh sách này để mỗi "+1" chạy hoạt ảnh nổi lên + mờ dần riêng.
4. Sau khi hoạt ảnh "+1" hoàn tất, tự động loại bỏ nó khỏi danh sách để ngăn rò rỉ bộ nhớ.
Vui lòng cung cấp trực tiếp code MainActivity.kt đã cập nhật.
```

![](/zh-cn/stage-3/cross-platform/android-app/images/image29.png)

![](/zh-cn/stage-3/cross-platform/android-app/images/image30.png)

## 3.5 Trình bày Kết quả Cuối cùng

Trong các bước trước đó, chúng ta đã hoàn thành một Mộc Ngư Điện tử có thể nhìn và nghe thấy. Để làm cho nó gần hơn với một ứng dụng có thể xuất bản, chúng ta sẽ sử dụng một lần lặp cuối cùng để thêm **phản hồi chạm** và **tùy chỉnh**. Chúng ta sẽ triển khai hai tính năng cốt lõi: thứ nhất, **phản hồi rung**, để mỗi lần chạm nhận được phản hồi vật lý từ động cơ rung của điện thoại và cải thiện đáng kể sự chìm đắm; thứ hai, **văn bản tùy chỉnh**, cho phép người dùng sửa đổi văn bản trên màn hình, ví dụ đổi "Công đức +1" thành "Lương +1" hoặc "Rắc rối -1".

Gửi Prompt được thiết kế cẩn thận sau đây cho Trae. Nó sẽ xử lý logic hộp thoại, chuyển đổi trạng thái và tương tác phần cứng trong một lần:

```text
Vai trò: Bạn là chuyên gia Android Jetpack Compose.
Nhiệm vụ: Vui lòng thêm "văn bản tùy chỉnh" và "phản hồi rung" vào ứng dụng Mộc Ngư Điện tử hiện có.
Yêu cầu:
1. Phản hồi Haptic
Mỗi khi người dùng chạm vào mộc ngư, ngoài âm thanh và hoạt ảnh, gọi phản hồi haptic của điện thoại (sử dụng LocalHapticFeedback.current) để đưa ra phản hồi xúc giác nhẹ.
2. Tính năng Văn bản Tùy chỉnh (UI và tương tác)
Đầu vào: Thêm một biểu tượng chỉnh sửa nhỏ bên cạnh văn bản phía trên như "Công đức +1" (có thể sử dụng Icons.Default.Edit).
Logic hộp thoại: Khi biểu tượng được chạm, hiển thị hộp thoại (Dialog/AlertDialog).
    Tiêu đề hộp thoại: "Sửa đổi Nội dung"
    Đầu vào: Cho phép người dùng nhập văn bản họ muốn tích lũy (mặc định là "Công đức")
    Lựa chọn giá trị: Bên dưới đầu vào, cung cấp hai tùy chọn (ví dụ RadioButton hoặc toggle) để người dùng chọn "+1" hoặc "-1"
    Nút lưu: Sau khi nhấp lưu, đóng hộp thoại và áp dụng cài đặt mới vào màn hình chính
    Làm mới dữ liệu: Nếu người dùng cập nhật nội dung, đặt lại bộ đếm phía trên về 0 và bắt đầu đếm lại từ đầu
3. Cập nhật hiệu ứng
Sau khi lưu, cả văn bản bộ đếm phía trên và văn bản hoạt ảnh nổi hiển thị khi chạm mộc ngư nên thay đổi theo định dạng tùy chỉnh của người dùng.
    Kích thước văn bản nổi không được vượt quá kích thước văn bản bộ đếm phía trên
    Ví dụ: nếu người dùng nhập "Lương" và chọn "+1", logic bộ đếm phía trên trở thành +1 và văn bản nổi trở thành "Lương+1"
    Nếu người dùng nhập "Rắc rối" và chọn "-1", logic bộ đếm phía trên trở thành -1 và văn bản nổi trở thành "Rắc rối-1"
4. Yêu cầu kỹ thuật:
Đảm bảo trạng thái mới (văn bản và số) ảnh hưởng đúng đến hoạt ảnh.
Vui lòng cung cấp trực tiếp code MainActivity.kt đã cập nhật đầy đủ trong khi giữ nguyên logic âm thanh và hoạt ảnh trước đó.
```

![](/zh-cn/stage-3/cross-platform/android-app/images/image31.png)

# 4 Gỡ lỗi và Tinh chỉnh trên Thiết bị Thực

Trình mô phỏng tiện lợi, nhưng nó không thể mô phỏng rung điện thoại thực hoặc phản ánh đầy đủ độ trễ chạm thực tế. Để có "cảm giác" chính xác nhất, chúng ta cần cài đặt ứng dụng trên điện thoại Android thực. Dưới đây là hai phương thức kết nối bạn có thể chọn:

1. **Gỡ lỗi không dây (Wi-Fi)**: không cần cáp dữ liệu, tiện lợi cho việc kiểm tra hàng ngày. Nhưng máy tính và điện thoại của bạn phải trên **cùng một mạng Wi-Fi**.
2. **Gỡ lỗi USB có dây**: ổn định hơn và ít bị ngắt kết nối hơn, phù hợp khi mạng kém hoặc cài đặt ban đầu thất bại.

## 4.1 Gỡ lỗi Không dây

Đây là phương thức tiện lợi nhất trên Android 11 trở lên.

**Bước 1: Chuẩn bị điện thoại**

1. Đảm bảo điện thoại và máy tính trên **cùng một Wi-Fi**.
2. Mở **Tùy chọn nhà phát triển** và bật **Gỡ lỗi không dây**.
3. Nhấp **Gỡ lỗi không dây** để vào chi tiết, sau đó chọn **Ghép nối thiết bị bằng mã QR**. Điện thoại của bạn sẽ mở chế độ quét.

![](/zh-cn/stage-3/cross-platform/android-app/images/image32.png)![](/zh-cn/stage-3/cross-platform/android-app/images/image33.png)

**Bước 2: Ghép nối trên máy tính**

1. Quay lại Android Studio, nhấp bộ chọn thiết bị trong thanh công cụ trên cùng.
2. Chọn **Pair Devices Using Wi-Fi** từ menu thả xuống.
3. Một mã QR sẽ xuất hiện trên màn hình.

![](/zh-cn/stage-3/cross-platform/android-app/images/image34.png)

**Bước 3: Quét để kết nối**

1. Sử dụng điện thoại để quét mã QR trên màn hình máy tính.
2. Cả điện thoại và máy tính sẽ hiển thị "ghép nối thành công."
3. Lúc này, thanh thiết bị phía trên của Android Studio sẽ tự động hiển thị kiểu điện thoại của bạn (ví dụ `Google Pixel 8`).

![](/zh-cn/stage-3/cross-platform/android-app/images/image35.png)

4. Chạy ứng dụng bằng cách nhấp ▶️ Run

![](/zh-cn/stage-3/cross-platform/android-app/images/image36.png)

## 4.2 Gỡ lỗi USB Có dây

Nếu kết nối không dây không ổn định, hoặc mạng của bạn phức tạp, cắm cáp luôn là giải pháp đáng tin cậy nhất. Mặc dù kém tiện lợi hơn, nhưng nó cho tốc độ truyền nhanh nhất và gần như không bao giờ bị ngắt kết nối.

### 4.2.1 Chuẩn bị Trình điều khiển USB trong Android Studio (Chỉ Windows)

Người dùng Mac có thể bỏ qua bước này, vì macOS thường nhận diện điện thoại trực tiếp. Người dùng Windows cần đảm bảo máy tính có thể nhận diện điện thoại Android, điều này thường có nghĩa là cài đặt trình điều khiển USB của Google:

1. Trong Android Studio, nhấp `Tools -> SDK Manager` (hoặc tìm nó dưới `Settings -> Languages & Frameworks -> Android SDK`)
2. Chuyển sang tab **SDK Tools**
3. Đánh dấu **Google USB Driver** và nhấp **Apply** để tải xuống và cài đặt

![](/zh-cn/stage-3/cross-platform/android-app/images/image37.png)![](/zh-cn/stage-3/cross-platform/android-app/images/image38.png)

![](/zh-cn/stage-3/cross-platform/android-app/images/image39.png)

### 4.2.2 Tải xuống Phiên bản SDK Giống với Thiết bị Thực của Bạn

**Bước 1: Kiểm tra phiên bản Android của điện thoại**

Sử dụng điện thoại OPPO làm ví dụ: mở Cài đặt -> Thông tin điện thoại -> kiểm tra phiên bản Android (trong ví dụ là Android 12).

![](/zh-cn/stage-3/cross-platform/android-app/images/image40.png)

**Bước 2: Tải xuống phiên bản nền tảng Android đó trong Android Studio**

1. Trong Android Studio, nhấp `Tools -> SDK Manager`
2. Ở tab **SDK Platforms** mặc định
3. Chọn Android 12.0 và nhấp Apply để tải xuống

![](/zh-cn/stage-3/cross-platform/android-app/images/image41.png)

### 4.2.3 Bật Chế độ Nhà phát triển trên Điện thoại

Mở cài đặt điện thoại, vào tùy chọn nhà phát triển và bật **Gỡ lỗi USB**.

![](/zh-cn/stage-3/cross-platform/android-app/images/image42.png)

### 4.2.4 Cấp phép Trình điều khiển USB trên Điện thoại

Lúc này, hãy cầm điện thoại lên. Nó sẽ hiển thị một hộp thoại bảo mật quan trọng: "Cho phép gỡ lỗi USB?" Đảm bảo đánh dấu **Luôn cho phép** và sau đó nhấp **Cho phép** hoặc **OK**. Đây là ủy quyền quan trọng cho phép máy tính kiểm soát việc gỡ lỗi.

![](/zh-cn/stage-3/cross-platform/android-app/images/image43.png)

### 4.2.5 Chạy Ứng dụng trên Điện thoại

1. Trong bộ chọn thiết bị phía trên của Android Studio, bây giờ bạn sẽ thấy kiểu điện thoại của mình (ví dụ `OPPO-PDKM00`)
2. Nhấp ▶️ Run. Điện thoại của bạn sẽ hiển thị hộp thoại "Cho phép gỡ lỗi USB?"; đánh dấu "Luôn cho phép" và xác nhận
3. Ứng dụng sẽ tự động cài đặt và khởi chạy

Bây giờ hãy thử chạm vào mộc ngư trên điện thoại và cảm nhận phản hồi của động cơ rung thực sự. Đây là trải nghiệm Vibe Coding hoàn chỉnh.

![](/zh-cn/stage-3/cross-platform/android-app/images/image44.png)![](/zh-cn/stage-3/cross-platform/android-app/images/image45.png)![](/zh-cn/stage-3/cross-platform/android-app/images/image46.png)

# 5 Đóng gói Ứng dụng thành APK

Code đã hoàn tất và kiểm tra trên thiết bị thực cũng hoạt động. Bây giờ chúng ta cần "lấy ứng dụng ra" khỏi Android Studio và biến nó thành một tệp bạn có thể gửi cho bạn bè để cài đặt. Quá trình này được gọi là **đóng gói**. Trong phát triển Android, đóng gói có hai chế độ hoàn toàn khác nhau và chúng ta chọn dựa trên kịch bản sử dụng.

## 5.1 Đóng gói Phiên bản Debug (để Chia sẻ Nhanh)

Nếu bạn chỉ muốn chia sẻ ứng dụng với bạn bè để thử nhanh, hoặc gửi đến điện thoại kiểm tra để xác minh, **Phiên bản Debug** là lựa chọn nhanh nhất. Nó giống như một "bản nháp" - đầy đủ chức năng, nhưng không được ký chính thức, vì vậy không thể gửi lên cửa hàng ứng dụng.

**Các bước rất đơn giản:** trong menu trên cùng của Android Studio, tìm `Build`, di chuột qua `Generate App Bundles or APKs` và nhấp `Generate APKs` từ menu con.

![](/zh-cn/stage-3/cross-platform/android-app/images/image47.png)

Đợi khoảng 5 giây tùy thuộc vào kích thước dự án. Trong vùng bảng điều khiển góc dưới bên phải của Android Studio, một lời nhắc sẽ xuất hiện. Nhấp vào liên kết `locate` màu xanh và thư mục đầu ra sẽ tự động mở. Tệp có tên `app-debug.apk` là gói chúng ta muốn.

Bạn có thể trực tiếp gửi nó qua WeChat hoặc QQ đến bất kỳ điện thoại Android nào và người nhận có thể cài đặt và sử dụng. Lưu ý rằng debug không phải là phiên bản phát hành.

![](/zh-cn/stage-3/cross-platform/android-app/images/image48.png)

![](/zh-cn/stage-3/cross-platform/android-app/images/image49.png)

## 5.2 Đóng gói Phiên bản Release

Nếu bạn muốn xuất bản ứng dụng lên cửa hàng ứng dụng (như Google Play hoặc Huawei AppGallery), hoặc tránh cảnh báo "ứng dụng không an toàn" trong quá trình cài đặt, thì bạn phải đóng gói **Phiên bản Release**. Phiên bản này yêu cầu một **chữ ký số** duy nhất, giống như một con dấu chống giả chứng minh rằng bạn đã phát triển ứng dụng này và nó chưa bị can thiệp.

> Mục đích cốt lõi của việc ký
>
> - Xác định danh tính nhà xuất bản: vì một ứng dụng có cùng tên gói có thể thay thế một chương trình đã cài đặt, việc ký ngăn chặn việc lạm dụng điều đó
> - Đảm bảo tính toàn vẹn của ứng dụng: quá trình ký bao phủ mọi tệp trong gói, đảm bảo chúng không bị thay thế sau đó

Ký ứng dụng Android giống như gắn một con dấu. Sau khi con dấu được gắn, ứng dụng và nhà phát triển được khóa lại với nhau: ứng dụng là của bạn và bạn chịu trách nhiệm về nó. Người khác không thể mạo danh bạn và bạn không thể mạo danh người khác.

**Bước 1: Bắt đầu thuật sĩ ký**

Trong menu trên cùng, chọn `Build`, sau đó nhấp `Generate Signed Bundle / APK`. Trong cửa sổ bật lên, bạn sẽ đối mặt với hai lựa chọn:

- Android App Bundle (`.aab`): yêu cầu bởi Google Play, nhỏ hơn về kích thước, nhưng không thể cài đặt trực tiếp trên điện thoại
- APK: gói cài đặt chuẩn, có thể cài đặt trực tiếp
_Với mục đích trình diễn, chúng ta chọn APK trước và nhấp Next._

![](/zh-cn/stage-3/cross-platform/android-app/images/image50.png)![](/zh-cn/stage-3/cross-platform/android-app/images/image51.png)

**Bước 2: Tạo khóa số (KeyStore)**

Đây là nơi người mới bắt đầu thường bị kẹt nhất. Vì đây là lần đóng gõi release đầu tiên của bạn, bạn cần tạo một **keystore** mới. Nhấp **Create new** bên dưới `Key store path`.

![](/zh-cn/stage-3/cross-platform/android-app/images/image52.png)

Trong cửa sổ bật lên, điền thông tin cần thiết, tương tự như đăng ký tài khoản. Chúng tôi khuyến nghị mạnh mẽ rằng mật khẩu keystore và mật khẩu bí danh khóa nên **giống nhau** và bạn **ghi chú cẩn thận**. Nếu bạn mất mật khẩu này, ứng dụng của bạn sẽ không bao giờ có thể cập nhật trong tương lai.

Sau khi hoàn tất, nhấp OK. Bạn sẽ quay lại màn hình trước và thông tin khóa bạn vừa điền đã được tự động điền.

![](/zh-cn/stage-3/cross-platform/android-app/images/image53.png)![](/zh-cn/stage-3/cross-platform/android-app/images/image54.png)

**Bước 3: Tạo gói chính thức**

Nhấp Next, chọn **release** dưới Build Variants và cuối cùng nhấp **Create**.

Sau một khoảng chờ ngắn, Android Studio sẽ lại hiển thị lời nhắc thành công "Generate Signed APK" ở góc dưới bên phải. Nhấp **locate** và lần này bạn sẽ thấy gói chính thức đã được ký số trong thư mục (thường có tên `app-release.apk`). Tệp này là sản phẩm cuối cùng bạn giao hàng với tư cách là nhà phát triển.

![](/zh-cn/stage-3/cross-platform/android-app/images/image55.png)

![](/zh-cn/stage-3/cross-platform/android-app/images/image56.png)![](/zh-cn/stage-3/cross-platform/android-app/images/image57.png)

# 6 Xuất bản Chính thức lên Cửa hàng / Thị trường Ứng dụng

Khi phát triển ứng dụng của bạn hoàn tất và gói Release đã sẵn sàng, bước tiếp theo là xuất bản nó để nhiều người có thể tải xuống và sử dụng. Hiện tại, các kênh phân phối chính được chia thành hai loại: **cửa hàng ứng dụng Android trong nước** và **cửa hàng ứng dụng nước ngoài (Google Play)**.

## 6.1 Xuất bản lên Thị trường Trong nước

Hệ sinh thái Android ở Trung Quốc đại lục rất đặc biệt. Không có một cửa hàng chính thức duy nhất (vì Google Play không thể truy cập trực tiếp). Thay vào đó, thị trường được chia giữa **cửa hàng ứng dụng của nhà sản xuất điện thoại** và **nền tảng bên thứ ba**. Các **cửa hàng nhà sản xuất** lớn bao gồm Huawei, Xiaomi, OPPO, vivo, Meizu, Samsung, v.v. Vì chúng được cài đặt sẵn trên thiết bị, chúng có lưu lượng lớn nhất. Các **nền tảng bên thứ ba** chính bao gồm Tencent MyApp và 360 Mobile Assistant.

### 6.1.1 Khó khăn Cốt lõi: "Chướng ngại vật" cho Nhà phát triển Cá nhân

Trước khi đăng ký tài khoản, có một điều rất quan trọng bạn cần biết: **thị trường ứng dụng trong nước rất khắt khe với nhà phát triển cá nhân.**

Hiện tại, gần như tất cả các cửa hàng ứng dụng trong nước lớn (Huawei, Xiaomi, OV, MyApp, v.v.) **yêu cầu** *Giấy chứng nhận Đăng ký Bản quyền Phần mềm* để nộp.

![](/zh-cn/stage-3/cross-platform/android-app/images/image58.png)![](/zh-cn/stage-3/cross-platform/android-app/images/image59.png)

- **Nó là gì?** Đây là một tài liệu pháp lý chứng minh ứng dụng thuộc về bạn.
- **Chi phí để có được nó**: bạn cần nộp đơn qua cục bản quyền. Tự làm thường mất 2-3 tháng; sử dụng dịch vụ để xử lý nhanh hơn có thể mất từ vài trăm đến hơn một nghìn RMB.
- **Thực tế hiện tại**: nếu không có giấy chứng nhận này, ứng dụng của bạn rất có thể sẽ không vượt qua đánh giá, hoặc bạn thậm chí có thể không thể tạo mục ứng dụng. Ngoài ra, các danh mục như tin tức, tài chính và y tế cũng có thể yêu cầu hồ sơ ICP hoặc các bằng cấp khác.

Vì vậy, nếu ứng dụng của bạn chỉ là một dự án thực hành cá nhân hoặc công cụ nhỏ và bạn không muốn dành thời gian và tiền bạc để xin giấy chứng nhận này, tôi đề nghị chuyển thẳng đến Mục 6.2 và cân nhắc Google Play thay thế, hoặc đơn giản là chia sẻ tệp APK với bạn bè trực tiếp.

### 6.1.2 Đăng ký Tài khoản Nhà phát triển

Nếu bạn đã chuẩn bị các bằng cấp cần thiết, hoặc đã quyết định xuất bản ở thị trường trong nước, bước đầu tiên là đăng ký tài khoản. Quy trình tương tự trên các nền tảng lớn, thường yêu cầu xác minh ID cho cá nhân hoặc xác minh giấy phép kinh doanh cho công ty.

Dưới đây là URL nền tảng nhà phát triển cho các thị trường ứng dụng lớn:

Nền tảng Mở Tencent: https://open.tencent.com/

Nền tảng Mở 360: http://dev.360.cn

Nền tảng Nhà phát triển Baidu: http://app.baidu.com

Nền tảng Mở Xiaomi: https://dev.mi.com

Liên minh Nhà phát triển Huawei: http://developer.huawei.com/consumer/cn

Nền tảng Nhà phát triển Alibaba: http://open.uc.cn
Phân phối Alibaba tích hợp Wandoujia, Ali Jiuyou, PP Assistant, UC App Store, Shenma Search và YunOS App Store. Bạn chỉ cần đăng ký một tài khoản nhà phát triển Alibaba.

Nền tảng Nhà phát triển Samsung: http://support-cn.samsung.com/App/DeveloperChina/Home/Index

Liên minh Nhà phát triển OPPO: http://open.oppomobile.com

Liên minh Nhà phát triển vivo: https://dev.vivo.com.cn

Nền tảng Mở Lenovo: http://open.lenovo.com

Liên minh Nhà phát triển Meizu: http://open.flyme.cn

Liên minh Nhà phát triển Gionee: https://open.appgionee.com

**Sử dụng Tencent MyApp làm ví dụ:** truy cập Nền tảng Mở Tencent và nhấp đăng ký. Khuyến nghị đăng nhập trực tiếp bằng tài khoản QQ. Lưu ý rằng khi một tài khoản QQ đã được liên kết, rất khó để hủy liên kết, nên tốt hơn là sử dụng tài khoản QQ công việc chuyên dụng. Làm theo hướng dẫn, chọn "Nhà phát triển Cá nhân" hoặc "Nhà phát triển Doanh nghiệp," tải lên ảnh ID của bạn và hoàn tất xác minh khuôn mặt. Sau khi vượt qua xác minh, nhấp **Tạo Ứng dụng** để bắt đầu.

![](/zh-cn/stage-3/cross-platform/android-app/images/image60.png)![](/zh-cn/stage-3/cross-platform/android-app/images/image61.png)

![](/zh-cn/stage-3/cross-platform/android-app/images/image62.png)

### 6.1.3 Quy trình Nộp và Tài liệu Yêu cầu

Sau khi đánh giá tài khoản được phê duyệt, bạn có thể tạo ứng dụng và nộp để đánh giá. Bạn cần chuẩn bị "bộ bốn món" sau:

1. **Gói cài đặt**: **Release APK** được đóng gói trong Chương 5
2. **Thông tin văn bản**:
3. **Tên ứng dụng**: không được chứa từ nhạy cảm
4. **Giới thiệu một dòng**: trong vòng 20 ký tự Trung Quốc, đơn giản và trực tiếp (ví dụ: "Một ứng dụng mộc ngư điện tử thư giãn")
5. **Mô tả chi tiết**: hơn 200 ký tự Trung Quốc giới thiệu chức năng và kịch bản sử dụng của ứng dụng
6. **Tài liệu trực quan**:
7. **Biểu tượng ứng dụng**: PNG độ nét cao, thường 512x512
8. **Ảnh chụp màn hình ứng dụng**: chuẩn bị 4-5 ảnh chụp màn hình rõ ràng của ứng dụng đang sử dụng, tốt nhất bao phủ các trang chính, thường có kích thước nhất quán như 1080x1920
9. **Tài liệu bằng cấp**: tải lên bản sao quét Giấy chứng nhận Đăng ký Bản quyền Phần mềm của bạn

**Nộp và đánh giá:** sau khi điền đầy đủ thông tin và tải lên APK, nhấp **Nộp để Đánh giá**. Chu kỳ đánh giá thường là 1-3 ngày làm việc. Trong thời gian đó, chú ý email hoặc SMS. Người đánh giá có thể từ chối nộp vì ảnh chụp màn hình không rõ ràng, mô tả không chuẩn hóa hoặc thiếu bằng cấp yêu cầu. Trong trường hợp đó, bạn sửa đổi theo phản hồi và nộp lại.

## 6.2 Xuất bản lên Thị trường Nước ngoài (Google Play)

Nếu bạn không muốn đối phó với sự phức tạp của giấy chứng nhận bản quyền phần mềm và hồ sơ trong các cửa hàng ứng dụng trong nước, hoặc nếu đối tượng mục tiêu của bạn là toàn cầu, Google Play là lựa chọn tốt nhất cho nhà phát triển cá nhân.

### 6.2.1 Chuẩn bị

- **Tài khoản Google**: một tài khoản Gmail bình thường là đủ
- **Phí đăng ký $25**: đây là **phí một lần trọn đời** và yêu cầu thẻ tín dụng hỗ trợ thanh toán USD (Visa / Mastercard)
- **Truy cập mạng đáng tin cậy**: bạn cần có thể truy cập Google Play Console trơn tru
- **Gói cài đặt chính thức**: lưu ý rằng Google Play yêu cầu định dạng **.aab** (Android App Bundle), không phải APK. Trong Android Studio, chọn Android App Bundle khi đóng gói. Các bước gần như giống hệt với đóng gói APK.

![](/zh-cn/stage-3/cross-platform/android-app/images/image63.png)

### 6.2.2 Quy trình Xuất bản Google Play Console (Tổng quan)

Vì việc đăng ký và thanh toán Google Play vẫn có một số rào cản gia nhập (như cần thẻ tín dụng nước ngoài), hướng dẫn này hiện không cung cấp ảnh chụp màn hình từng bước. Nhưng đây là quy trình bốn bước phổ biến:

**Bước 1: Tạo ứng dụng và vào bảng điều khiển**

Nhấp `Create app`, điền tên ứng dụng (`Mộc Ngư Điện tử`), chọn tiếng Anh làm ngôn ngữ, chọn App và Free làm loại ứng dụng, sau đó đánh dấu vào thỏa thuận. Sau đó, bạn sẽ có quyền truy cập vào backend.

**Bước 2: Trang trí trang cửa hàng**

Đây là ấn tượng đầu tiên của người dùng. Bạn cần tải lên **biểu tượng** ứng dụng đã chuẩn bị (512x512) và **hình ảnh nổi bật** (1024x500). Về mô tả tiếng Anh, bạn có thể đơn giản yêu cầu Trae: **"Vui lòng giúp tôi viết mô tả tiếng Anh để xuất bản Mộc Ngư Điện tử trên Google Play, với giọng điệu nhẹ nhàng và thư giãn."** AI thường viết tự nhiên hơn là dịch trực tiếp.

**Bước 3: Chính sách bảo mật và phân loại nội dung**

- Chính sách bảo mật: tìm kiếm "App Privacy Policy Generator" và tạo một liên kết miễn phí để dán vào
- Phân loại nội dung: điền một bảng hỏi đơn giản (ví dụ, có bạo lực hay cờ bạc không). Mộc Ngư Điện tử thường nhận phân loại chung 3+.

**Bước 4: Tải lên và xuất bản**

Trong menu `Production`, nhấp `Create new release`, tải lên tệp `.aab` của bạn, lưu và nộp để đánh giá. Đánh giá Google Play thường nhanh (1-3 ngày). Sau khi được phê duyệt, ứng dụng của bạn có thể được tải xuống trên toàn cầu.

![](/zh-cn/stage-3/cross-platform/android-app/images/image64.png)

_Nếu bạn đã hoàn tất đăng ký tài khoản nhà phát triển, video hướng dẫn này có thể hướng dẫn bạn qua phần còn lại của quy trình:_ [Quy trình làm việc đầy đủ: tải ứng dụng Android lên Google Play](https://www.bilibili.com/video/BV16REQzGEnk/?share_source=weixin&vd_source=b42f227a4f2d413fbde18499d83227cf)

# 7 Lời kết

Điều đó đưa chúng ta đến cuối hướng dẫn. Nhìn vào Mộc Ngư Điện tử mà bạn đích thân tạo ra trên điện thoại, tôi tự hỏi bạn cảm thấy thế nào lúc này.

Là một người được đào tạo trong kỹ thuật phần mềm, tôi thực sự cảm thấy khá xúc động trong kỷ nguyên AI phát triển nhanh chóng ngày nay. Trước đây, chúng ta đã làm việc qua những cuốn sách lập trình dày cộp, học cú pháp phức tạp, vật lộn với thiết lập môi trường và dành nửa ngày để chiến đấu với các thông báo lỗi màu đỏ. Nhưng thời đại đã thay đổi và bây giờ chúng ta ngày càng học cách cách đạo diễn AI.

Thông qua thực hành Vibe Coding này, bạn đã trải nghiệm quy trình phát triển ứng dụng Android đầy đủ. Rào cản kỹ thuật đang giảm xuống. Chúng ta không cần phải mài giũa qua code khô khan liên tục và có thể dành nhiều năng lượng hơn cho việc quyết định **xây dựng cái gì**. Nhưng không matter công cụ mạnh thế nào, chúng vẫn chỉ là công cụ. Đừng để ứng dụng này thu bụi trên điện thoại của bạn. Tiếp tục chỉnh sửa nó, phá vỡ nó và sửa nó lại. Chỉ khi bạn bắt đầu có ý tưởng riêng và biến chúng thành hiện thực, bạn mới thực sự vượt qua ngưỡng.

Nếu hướng dẫn này giúp bạn cảm thấy "xây dựng ứng dụng thực sự không khó đến vậy," thì tôi rất vinh dự đã giúp đưa thêm một nhà xây dựng thế hệ mới vào thế giới phát triển.

Tôi thực sự rất mong chờ tác phẩm tiếp theo của bạn. Hãy tiếp tục!

![](/zh-cn/stage-3/cross-platform/android-app/images/image65.png)

**_Hy vọng bạn sẽ vui vẻ trong thế giới phát triển Android!_**

# Tài liệu Tham khảo

CSDN: [Cách đóng gói/xây dựng dự án Android Studio (2024-03-04)](https://blog.csdn.net/GenuineMonster/article/details/136443130?ops_request_misc=&request_id=&biz_id=102&utm_term=android%20studio%20%E6%89%93%E5%8C%85%20APK%20%E5%B9%B6%E5%88%86%E4%BA%AB&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-136443130.142^v102^pc_search_result_base4&spm=1018.2226.3001.4187)

CSDN: [Cài đặt và cấu hình Android Studio](https://blog.csdn.net/Changersh/article/details/149838228?ops_request_misc=&request_id=&biz_id=102&utm_term=android%20studio%E5%AE%89%E8%A3%85%E5%8F%8A%E9%85%8D%E7%BD%AE&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-149838228.142^v102^pc_search_result_base4&spm=1018.2226.3001.4187)
