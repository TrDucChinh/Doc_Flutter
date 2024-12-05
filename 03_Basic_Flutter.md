# Basic Flutter
## FVM (Flutter Version Managemet)
- là một công cụ quản lý phiên bản Flutter. 
- Nó cho phép cài đặt, quản lý, và chuyển đổi giữa nhiều phiên bản Flutter dễ dàng trên cùng một máy tính. 
- Điều này rất hữu ích khi làm việc với các dự án Flutter sử dụng các phiên bản Flutter khác nhau.
### Lợi ích khi dùng FVM
- **Quản lý đa phiên bản Flutter:**
  - Cài đặt và sử dụng nhiều phiên bản Flutter cùng lúc mà không cần gỡ bỏ phiên bản cũ.
  - Tránh xung đột khi các dự án yêu cầu các phiên bản Flutter khác nhau.
- **Đồng bộ hoá với dự án:**
  - Mỗi dự án có thể sử dụng một phiên bản Flutter riêng biệt mà không ảnh hưởng đến các dự án khác.
- **Chuyển đổi phiên bản nhanh chóng:**
  - Dễ dàng chuyển đổi giữa các phiên bản Flutter chỉ với vài lệnh đơn giản.
- **Tích hợp CI/CD:**
  - Hỗ trợ tự động hóa quá trình build và test trong các pipelines.

### Cách sử dụng
- Cài đặt qua `choco`: `choco install fvm`
- Cài đặt qua `Dart Pub`: `dart pub global activate fvm`
- Cài đặt phiên bản Flutter: `fvm install <flutter_version>`
- Chọn phiên bản Flutter cho dự án: `fvm use <flutter_version>`
- Sử dụng flutter thông qua `FVM`: `fvm flutter <command>`
```bash
fvm flutter doctor
fvm flutter run
```
- Kiểm danh sách các phiên bản Flutter đã cài: `fvm list`
- Gỡ các phiên bản không cần thiết: `fvm remove <flutter_version>`
## Statefull, Stateless, vòng đời của một state
- Trong `Flutter` thì tất cả đều là **Widget** và có 2 Widget quan trọng nhất là **StatefulWidget** và **StatelessWidget**
### Stateless Widget
- **StatelessWidget** là một widget không có trạng thái thay đổi. Nó chỉ nhận dữ liệu qua `constructor` và không tự thay đổi sau khi được dựng lên.
- Dữ liệu hoặc giao diện của nó không thay đổi sau khi được xây dựng.
- **Không có trạng thái nội bộ (state)**: Giao diện của StatelessWidget không thay đổi khi dữ liệu thay đổi.
- **Hiệu năng cao hơn StatefulWidget:** Vì không cần quản lý trạng thái nên quá trình dựng giao diện nhanh hơn.
- **Chỉ tái dựng khi widget cha yêu cầu:** Nếu widget cha thay đổi và tạo lại widget con, nó mới tái dựng giao diện.
#### Vòng đời của Stateless Widget

![](https://cdn.codegym.vn/wp-content/uploads/2020/05/p2-tim-hieu-ve-stateless-stateful-widgets-trong-flutter-8.png)

- **StatelessWidget** có vòng đời rất đơn giản với chỉ một phương thức quan trọng:
  - **Constructor:** Nhận các tham số cần thiết.
  - **build():** Tạo giao diện cho widget.
#### Ưu điểm, nhược điểm
- **Ưu điểm:**
  - Đơn giản, dễ viết và bảo trì.
  - Phù hợp cho các thành phần giao diện không thay đổi, ví dụ: `Text, Icon, Image, hoặc layout tĩnh.`
- **Nhược điểm:**
  - Không thể tự thay đổi dữ liệu bên trong.
  - Mỗi khi dữ liệu cần thay đổi,  phải tạo lại toàn bộ widget.
> Tuy **StatelessWidget** không thể tự thay đổi được chính nó, nhưng khi **Widget** cha thay đổi thì **StatelessWidget** sẽ được khởi tạo lại.
```dart
class GreetingWidget extends StatelessWidget {
  final String name;

  const GreetingWidget({Key? key, required this.name}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Text('Hello, $name!');
  }
}
```
#### Sử dụng khi nào
- Hiển thị dữ liệu cứng. ex: Appbar, Title của màn hình vvv
- Sử dụng trong **StatefulWidget** để khi **StatefulWidget** thay đổi trạng thái thì các item con sẽ được render lại.

![](https://images.viblo.asia/6433b417-aff8-403e-ae05-ef63673b8b6b.png)

```Dart
import 'package:flutter/material.dart';

class WelcomeScreen extends StatelessWidget {
  final String message;

  const WelcomeScreen({Key? key, required this.message}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Welcome Screen'),
      ),
      body: Center(
        child: Text(
          message,
          style: const TextStyle(fontSize: 24),
        ),
      ),
    );
  }
}

void main() {
  runApp(const MaterialApp(
    home: WelcomeScreen(message: 'Hello, Flutter!'),
  ));
}
```
- **StatelessWidget:**
  - Dữ liệu truyền vào qua **constructor *`(message)`*** và không thay đổi.
  - *`build()`* được gọi một lần khi widget được tạo.
- **Không cần trạng thái:**
  - Giao diện tĩnh, không cần thay đổi hay làm mới.
### Stateful Widget
- `StatefulWidget` là một widget có khả năng quản lý trạng thái nội bộ (state) và thay đổi giao diện mỗi khi trạng thái thay đổi.
- **Có trạng thái nội bộ:** Sử dụng lớp `State` để lưu trữ dữ liệu có thể thay đổi.
- **Giao diện thay đổi linh hoạt:** Khi trạng thái thay đổi, Flutter sẽ gọi lại phương thức `build()` để cập nhật giao diện.
- **Quy trình phức tạp hơn StatelessWidget:** Do phải quản lý trạng thái và tài nguyên.

#### Cấu trúc Stateful Widget
- **StatefulWidget** được chia làm hai phần:
  - **Lớp StatefulWidget:** Đóng vai trò `"khung"` của widget, không thay đổi.
  - **Lớp State:** Quản lý trạng thái và logic thay đổi giao diện.

```Dart
class MyStatefulWidget extends StatefulWidget {
  @override
  _MyStatefulWidgetState createState() => _MyStatefulWidgetState();
}

class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  @override
  Widget build(BuildContext context) {
    return Text('This is a StatefulWidget');
  }
}
```

#### Vòng đời StatefulWidget
- **Khởi tạo**
  - `createState():`
    - Tạo và liên kết một instance của lớp State.
    - Chỉ được gọi một lần khi widget được tạo.
  - `initState():`
    - Được gọi một lần khi State được khởi tạo.
    - Dùng để khởi tạo dữ liệu, đăng ký lắng nghe sự kiện.
- **Dựng giao diện**
  - `build():`
  - Dựng lại giao diện mỗi khi trạng thái thay đổi thông qua `setState()`.
- **Cập nhật**
  - `didUpdateWidget():`
    - Được gọi khi widget cha tạo lại widget con với các tham số mới.
  - **didChangeDependencies():**
    - Được gọi khi dependencies thay đổi.
- **Hủy**
  - `dispose():`
    - Được gọi khi widget bị hủy để giải phóng tài nguyên (ví dụ: đóng luồng, hủy lắng nghe sự kiện).

- `createState()` → Tạo state.
- `initState()` → Khởi tạo state.
- `build()` → Dựng giao diện.
- `setState()` → Cập nhật giao diện.
- `didUpdateWidget() / didChangeDependencies()` → Cập nhật widget.
- `dispose()` → Hủy tài nguyên.
#### Ưu, nhược điểm
- **Ưu điểm:**
  - Linh hoạt, có thể thay đổi trạng thái trong runtime.
  - Phù hợp cho các giao diện phức tạp và động.
- **Nhược điểm:**
  - Cần quản lý trạng thái cẩn thận để tránh gây lỗi.
  - Hiệu năng thấp hơn `StatelessWidget` do phải tái dựng giao diện.

```Dart
class CounterWidget extends StatefulWidget {
  @override
  _CounterWidgetState createState() => _CounterWidgetState();
}

class _CounterWidgetState extends State<CounterWidget> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        Text('Counter: $_counter'),
        ElevatedButton(
          onPressed: _incrementCounter,
          child: Text('Increment'),
        ),
      ],
    );
  }
}
```
#### Sử dụng khi nào

![](https://images.viblo.asia/4e09c640-7e14-4d30-b67c-a0d512decbeb.png)

- nên sử dụng **StatefulWidget** trong Widget con của **ListView** để các item của chúng có thể tự động cập nhật trạng thái mà không cần cả **ListView** thay đổi trạng thái.

```Dart
import 'package:flutter/material.dart';

class CounterScreen extends StatefulWidget {
  const CounterScreen({Key? key}) : super(key: key);

  @override
  _CounterScreenState createState() => _CounterScreenState();
}

class _CounterScreenState extends State<CounterScreen> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Counter Screen'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'You have pressed the button this many times:',
              style: TextStyle(fontSize: 18),
            ),
            Text(
              '$_counter',
              style: const TextStyle(fontSize: 36, fontWeight: FontWeight.bold),
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        child: const Icon(Icons.add),
      ),
    );
  }
}

void main() {
  runApp(const MaterialApp(
    home: CounterScreen(),
  ));
}
```

- **StatefulWidget:**
  - Dữ liệu trạng thái được lưu trong biến `_counter` thuộc lớp `State (_CounterScreenState)`.
  - Khi `_incrementCounter()` được gọi, `setState` cập nhật trạng thái và giao diện tự động được dựng lại.
- State thay đổi động:
  - Mỗi lần nhấn nút, giá trị `_counter` tăng và giao diện hiển thị số mới.
### Vòng đời của 1 state
- **Giai đoạn khởi tạo:** 
  - *`createState()`*
    - Được gọi khi **StatefulWidget** được tạo ra.
    - Trả về một `instance` của lớp `State`.
    - Không sử dụng các hàm async hoặc thay đổi giao diện ở đây.
    ```dart
      @override
      _CounterWidgetState createState() => _CounterWidgetState();
    ```
  - *`initState()`*
    - Được gọi một lần duy nhất khi State được khởi tạo.
    - Sử dụng để:
      - Đăng ký lắng nghe sự kiện.
      - Khởi tạo biến hoặc dữ liệu cần thiết.
      - Tương tác với các dependencies ban đầu.
    ```Dart
        @override
        void initState() {
          super.initState();
          print('State initialized');
        }
    ```
- **Giai đoạn dựng giao diện:**
  - *`build()`*
    - Được gọi mỗi khi:
      - Giao diện cần được dựng lại.
      - **State** thay đổi thông qua *`setState()`*.
  - Giao diện được dựng dựa trên trạng thái hiện tại.
  ```Dart
    int _counter = 0;

    void _incrementCounter() {
      setState(() {
        _counter++;
      });
    }

    @override
    Widget build(BuildContext context) {
      return Text('Hello, Flutter!');
    }
  ```
- **Giai đoạn cập nhật:**
  - *`didChangeDependencies()`*
    - Được gọi khi widget phụ thuộc vào các **InheritedWidget** hoặc **dependencies** thay đổi.
    - Ví dụ: Thay đổi `Theme, MediaQuery`.
  ```Dart
      @override
      void didChangeDependencies() {
        super.didChangeDependencies();
        print('Dependencies changed');
      }
  ```
  - *`didUpdateWidget()`*
    - Được gọi khi widget cha tạo lại **StatefulWidget** này với các tham số mới.
    - Dùng để đồng bộ lại dữ liệu hoặc trạng thái.
    ```Dart
        @override
        void didUpdateWidget(covariant CounterWidget oldWidget) {
          super.didUpdateWidget(oldWidget);
          print('Widget updated');
        }
    ```
- **Giai đoạn hủy**
  - *`dispose()`*
    - Được gọi khi widget bị xóa khỏi cây widget.
    - Dùng để:
      - Hủy bỏ lắng nghe sự kiện.
      - Giải phóng bộ nhớ hoặc tài nguyên.
  ```Dart
    @override
    void dispose() {
      print('State disposed');
      super.dispose();
    }
  ```

- **Lưu đồ chi tiết**
  - **StatefulWidget được tạo**
    - *`createState()`* → Trả về instance của State.
  - **State được khởi tạo**
    - *`initState()`* → Chuẩn bị dữ liệu.
  - **Dựng giao diện**
    - *`build()`* → Tạo UI từ dữ liệu.
  - **Cập nhật trạng thái**
    - *`setState()`* → Gọi lại *`build()`*.
  - **Phụ thuộc thay đổi**
    - *`didChangeDependencies()`* → Đồng bộ.
  - **Widget cha thay đổi**
    - *`didUpdateWidget()`* → Cập nhật widget con.
  - **Widget bị hủy**
    - *`dispose()`* → Dọn dẹp tài nguyên.

| Giai đoạn          | Phương thức               | Chức năng                                       |
| ------------------ | ------------------------- | ----------------------------------------------- |
| Khởi tạo           | `createState()`           | Tạo state liên kết với widget.                  |
| Khởi tạo dữ liệu   | `initState()`             | Khởi tạo trạng thái ban đầu.                    |
| Dựng giao diện     | `build()`                 | Xây dựng giao diện UI.                          |
| Cập nhật phụ thuộc | `didChangeDependencies()` | Phụ thuộc thay đổi.                             |
| Cập nhật widget    | `didUpdateWidget()`       | Đồng bộ khi widget cha thay đổi.                |
| Hủy widget         | `dispose()`               | Giải phóng tài nguyên khi không cần widget nữa. |


## MaterialApp
- `MaterialApp` là một `widget` quan trọng trong Flutter
- Được sử dụng để xây dựng các ứng dụng theo phong cách `Material Design (thiết kế do Google phát triển)`. 
- Nó là một phần trong thư viện `Material Components` của Flutter và thường là `widget gốc (root widget)` của ứng dụng.

### Chức năng 
- **MaterialApp** cung cấp cấu trúc và các tiện ích cơ bản cho một ứng dụng Flutter, bao gồm:
  - **Quản lý điều hướng (Routing):**
    - Hỗ trợ điều hướng giữa các màn hình thông qua routes.
    - Cung cấp các phương pháp như `Navigator.push` và `Navigator.pop`.
  - **Cung cấp chủ đề (Theme):**
    - Quản lý giao diện ứng dụng toàn cầu (màu sắc, kiểu chữ, v.v.).
    - Hỗ trợ chủ đề sáng `(ThemeData.light())` và tối `(ThemeData.dark())`.
- **Cấu trúc mặc định:**
  - Định nghĩa ứng dụng với `AppBar, FloatingActionButton, Drawer`, v.v.
  - Dễ dàng tích hợp các widget theo `Material Design`.
- **Quốc tế hóa (Localization):**
  - Hỗ trợ nhiều ngôn ngữ thông qua thuộc tính `localizationsDelegates` và `supportedLocales`.
- **Hỗ trợ các tiện ích chính:**
  - Định nghĩa màn hình chính với thuộc tính `home`.
  - Quản lý `debugBanner, navigatorKey`, v.v.

### Các thuộc tính quan trọng của MaterialApp

| Thuộc tính                     | Mô tả                                                                                   |
| ------------------------------ | --------------------------------------------------------------------------------------- |
| *`home`*                       | Widget hiển thị đầu tiên khi ứng dụng khởi chạy.                                        |
| *`routes`*                     | Định nghĩa các đường dẫn cho điều hướng (routing).                                      |
| *`theme`*                      | Xác định giao diện chủ đề cho toàn bộ ứng dụng.                                         |
| *`darkTheme`*                  | Chủ đề ứng dụng khi ở chế độ tối (Dark Mode).                                           |
| *`themeMode`*                  | Chuyển đổi giữa chế độ sáng, tối, hoặc theo hệ thống (ThemeMode.light, ThemeMode.dark). |
| *`locale`*                     | Đặt ngôn ngữ cụ thể cho ứng dụng.                                                       |
| *`localizationsDelegates`*     | Cung cấp các tài nguyên bản địa hóa (Localization).                                     |
| *`debugShowCheckedModeBanner`* | Hiển thị/hủy bỏ banner "DEBUG" ở góc trên cùng bên phải màn hình.                       |
| *`navigatorKey`*               | Quản lý navigator toàn cục cho ứng dụng.                                                |

> MaterialApp là cần thiết nếu muốn sử dụng các thành phần *`Material Design`*.
> Nếu không cần *`Material Design`*,  có thể sử dụng *`WidgetsApp`*hoặc *`CupertinoApp`* thay thế.
> *`themeMode`* rất hữu ích để áp dụng chế độ tối/sáng tự động.
#### Ví dụ cơ bản
```Dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'My Flutter App',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const HomeScreen(),
    );
  }
}

class HomeScreen extends StatelessWidget {
  const HomeScreen({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home Screen'),
      ),
      body: const Center(
        child: Text('Welcome to My App!'),
      ),
    );
  }
}
```
#### Ví dụ routing
- Sử dụng `routes` để điều hướng giữa các màn hình.
```Dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Routing Example',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.green,
      ),
      initialRoute: '/',
      routes: {
        '/': (context) => const HomeScreen(),
        '/details': (context) => const DetailScreen(),
      },
    );
  }
}

class HomeScreen extends StatelessWidget {
  const HomeScreen({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home Screen'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pushNamed(context, '/details');
          },
          child: const Text('Go to Details'),
        ),
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  const DetailScreen({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Detail Screen'),
      ),
      body: const Center(
        child: Text('This is the details page!'),
      ),
    );
  }
}
```
#### Ví dụ Dartk Mode**
- Sử dụng `themeMode` để hỗ trợ chế độ sáng và tối.
```Dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Dark Mode Example',
      debugShowCheckedModeBanner: false,
      theme: ThemeData.light(),
      darkTheme: ThemeData.dark(),
      themeMode: ThemeMode.system, // Theo cài đặt hệ thống
      home: const HomeScreen(),
    );
  }
}

class HomeScreen extends StatelessWidget {
  const HomeScreen({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home Screen'),
      ),
      body: const Center(
        child: Text('Hello, Flutter with Dark Mode!'),
      ),
    );
  }
}
```

### Tất cả thuộc tính
#### Basic

| Thuộc tính                   | Kiểu dữ liệu                 | Mô tả                                                                                         |
| ---------------------------- | ---------------------------- | --------------------------------------------------------------------------------------------- |
| `home`                       | `Widget?`                    | Widget đầu tiên được hiển thị khi ứng dụng khởi chạy.                                         |
| `title`                      | `String`                     | Tiêu đề của ứng dụng. Hiển thị trên **task switcher** của hệ điều hành.                       |
| d`ebugShowCheckedModeBanner` | `bool`                       | Hiển thị/hủy banner "DEBUG" ở góc trên cùng bên phải màn hình (mặc định: `true`).             |
| `color`                      | `Color?`                     | Màu chính của ứng dụng khi hiển thị trên **task switcher**.                                   |
| `builder`                    | `TransitionBuilder?`         | Hàm để bọc widget gốc bằng một widget khác, ví dụ: `MediaQuery`.                              |
| `navigatorKey`               | `GlobalKey<NavigatorState>?` | Cung cấp một `Key` để quản lý `Navigator` toàn cục.                                           |
| `navigatorObservers`         | `List<NavigatorObserver>`    | Danh sách các observer để lắng nghe hoặc theo dõi các sự kiện điều hướng (navigation events). |

#### UI & Theme
| Thuộc tính              | Kiểu dữ liệu | Mô tả                                                                                |
| ----------------------- | ------------ | ------------------------------------------------------------------------------------ |
| `theme`                 | `ThemeData?` | Chủ đề mặc định (theme) của ứng dụng.                                                |
| `darkTheme`             | `ThemeData?` | Chủ đề tối (dark theme) của ứng dụng.                                                |
| `highContrastTheme`     | `ThemeData?` | Chủ đề dành cho người dùng cần hỗ trợ độ tương phản cao.                             |
| `highContrastDarkTheme` | `ThemeData?` | Chủ đề tối dành cho người dùng cần hỗ trợ độ tương phản cao.                         |
| `themeMode`             | `ThemeMode`  | Xác định chế độ chủ đề: sáng (`light`), tối (`dark`), hoặc theo hệ thống (`system`). |

#### Localization
| Thuộc tính                     | Kiểu dữ liệu                                | Mô tả                                                                         |
| ------------------------------ | ------------------------------------------- | ----------------------------------------------------------------------------- |
| `locale`                       | `Locale?`                                   | Ngôn ngữ của ứng dụng.                                                        |
| `localeListResolutionCallback` | `LocaleListResolutionCallback?`             | Hàm gọi lại để chọn locale dựa trên danh sách ưu tiên của hệ điều hành.       |
| `localeResolutionCallback`     | `LocaleResolutionCallback?`                 | Hàm gọi lại để chọn locale dựa trên ngôn ngữ và vùng được hỗ trợ.             |
| `supportedLocales`             | `Iterable<Locale>`                          | Danh sách các ngôn ngữ mà ứng dụng hỗ trợ (mặc định: `[Locale('en', 'US')]`). |
| `localizationsDelegates`       | `Iterable<LocalizationsDelegate<dynamic>>?` | Danh sách các delegate để cung cấp dữ liệu bản địa hóa.                       |
#### Navigation & Routing
| Thuộc tính        | Kiểu dữ liệu                 | Mô tả                                                            |
| ----------------- | ---------------------------- | ---------------------------------------------------------------- |
| `initialRoute`    | `String?`                    | Đường dẫn khởi đầu của ứng dụng (nếu không dùng `home`).         |
| `routes`          | `Map<String, WidgetBuilder>` | Bản đồ các đường dẫn đến các widget tương ứng.                   |
| `onGenerateRoute` | `RouteFactory?`              | Hàm gọi lại để tạo route động khi không có route trong `routes.` |
| `onUnknownRoute`  | `RouteFactory?`              | Hàm gọi lại khi không tìm thấy route tương ứng.                  |
#### Animation & Transition
| Thuộc tính                | Kiểu dữ liệu               | Mô tả                                                                                     |
| ------------------------- | -------------------------- | ----------------------------------------------------------------------------------------- |
| `builder`                 | `TransitionBuilder?`       | Cho phép bọc giao diện bằng widget khác (ví dụ: để thêm hiệu ứng hoặc thay đổi toàn cục). |
| `onGenerateInitialRoutes` | `InitialRouteListFactory?` | Tùy chỉnh danh sách các route ban đầu (khi ứng dụng khởi động).                           |
#### Cài đặt Debug & Test
| Thuộc tính                    | Kiểu dữ liệu | Mô tả                                                                           |
| ----------------------------- | ------------ | ------------------------------------------------------------------------------- |
| `debugShowMaterialGrid`       | `bool`       | Hiển thị lưới Material Design để hỗ trợ kiểm tra giao diện (mặc định: `false`). |
| `debugCheckElevationsEnabled` | `bool?`      | Hiển thị cảnh báo khi các shadow elevation không hợp lệ (mặc định: `null`).     |
#### Platform Integration
| Thuộc tính  | Kiểu dữ liệu                  | Mô tả                                                                  |
| ----------- | ----------------------------- | ---------------------------------------------------------------------- |
| `shortcuts` | `Map<LogicalKeySet, Intent>?` | Định nghĩa các phím tắt cho ứng dụng (thường dùng cho desktop và web). |
| `actions`   | `Map<Type, Action<Intent>>?`  | Định nghĩa các hành động tương ứng với phím tắt.                       |

## Scaffold
- `Scaffold` là một widget quan trọng trong Flutter
- Được thiết kế để cung cấp bố cục cơ bản cho một ứng dụng tuân theo Material Design. 
- Nó thường được sử dụng để định nghĩa giao diện của một màn hình cụ thể với các thành phần giao diện như `AppBar`, `Drawer`, `BottomNavigationBar`, và các widget khác.
### Chức năng
- `Scaffold` tạo ra một khung giao diện chứa các thành phần` Material Design` phổ biến, bao gồm:
  - **AppBar:** Thanh tiêu đề ở đầu màn hình.
  - **Drawer:** Menu điều hướng ở bên trái hoặc phải màn hình.
  - **FloatingActionButton:** Nút nổi để thực hiện các hành động chính.
  - **BottomNavigationBar:** Thanh điều hướng ở dưới cùng màn hình.
  - **SnackBar:** Hiển thị thông báo tạm thời.
  - **Body:** Phần nội dung chính của màn hình.
### Các thuộc tính quan trọng của Scaffold

| Thuộc tính                       | Kiểu dữ liệu                  | Mô tả                                                                       |
| -------------------------------- | ----------------------------- | --------------------------------------------------------------------------- |
| `appBar`                         | `PreferredSizeWidget?`        | Thanh công cụ hoặc tiêu đề của màn hình.                                    |
| `body`                           | `Widget?`                     | Nội dung chính của màn hình.                                                |
| `floatingActionButton`           | `Widget?`                     | Nút nổi để thực hiện các hành động chính.                                   |
| `floatingActionButtonLocation`   | FloatingActionButtonLocation? | Vị trí của `FloatingActionButton`.                                          |
| `floatingActionButtonAnimator`   | FloatingActionButtonAnimator? | Hiệu ứng chuyển động của `FloatingActionButton`.                            |
| `drawer`                         | `Widget?	`                    | Menu điều hướng trượt từ bên trái màn hình.                                 |
| `endDrawer`                      | `Widget?`                     | Menu điều hướng trượt từ bên phải màn hình.                                 |
| `bottomNavigationBar`            | `Widget?`                     | Thanh điều hướng ở dưới cùng màn hình.                                      |
| `bottomSheet`                    | `Widget?`                     | Widget hiển thị như một bảng trượt từ dưới lên (Bottom Sheet).              |
| `backgroundColor`                | C`olor?`                      | Màu nền của Scaffold.                                                       |
| `resizeToAvoidBottomInset`       | `bool?`                       | Tự động thay đổi kích thước body khi bàn phím xuất hiện (mặc định: `true`). |
| `drawerEnableOpenDragGesture`    | `bool`                        | Có cho phép kéo để mở drawer hay không (mặc định: `true`).                  |
| `endDrawerEnableOpenDragGesture` | `bool`                        | Có cho phép kéo để mở `endDrawer`hay không (mặc định: `true`).              |
| `onDrawerChanged`                | `ValueChanged<bool>?`         | Gọi lại khi trạng thái `drawer` thay đổi (đóng/mở).                         |
| `onEndDrawerChanged`             | `ValueChanged<bool>?`         | Gọi lại khi trạng thái `endDrawer` thay đổi (đóng/mở).                      |
| `persistentFooterButtons`        | `List<Widget>?	`              | Danh sách các nút hiển thị cố định ở cuối màn hình.                         |

## Container
- `Container` là một trong những widget cơ bản và phổ biến nhất trong Flutter.
- Nó được sử dụng để chứa các widget khác và cung cấp nhiều khả năng tùy chỉnh, bao gồm khả năng thay đổi `kích thước, màu sắc, bo góc, padding, margin`, và nhiều thuộc tính khác. 
- `Container` giúp  dễ dàng xây dựng giao diện phức tạp với các phần tử `UI` dễ dàng và linh hoạt.

### Các thuộc tính
| Thuộc tính    | Kiểu dữ liệu     | Mô tả                                                                                                                                   |
| ------------- | ---------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `color`       | `Color`          | Thiết lập màu nền của `Container`. Nếu không đặt, mặc định là trong suốt.                                                               |
| `width`       | `double`         | Xác định chiều rộng của `Container`. Nếu không đặt, `Container` sẽ có chiều rộng tự động dựa vào widget con.                            |
| `height`      | `double`         | Xác định chiều cao của `Container`. Nếu không đặt, `Container` sẽ có chiều cao tự động dựa vào widget con.                              |
| `padding`     | `EdgeInsets`     | Tạo khoảng cách bên trong `Container`, giữa `Container` và widget con.                                                                  |
| `margin`      | `EdgeInsets`     | Tạo khoảng cách bên ngoài `Container`, giữa `Container` và các widget khác trong giao diện.                                             |
| `decoration`  | `BoxDecoration`  | Dùng để trang trí `Container`, bao gồm các thuộc tính như màu nền, viền, bo góc, bóng đổ.                                               |
| a`lignment`   | `Alignment`      | Căn chỉnh widget con bên trong `Container` (ví dụ: `Alignment.center, Alignment.topLeft`, v.v.).                                        |
| `constraints` | `BoxConstraints` | Đặt các ràng buộc kích thước cho `Container`, bao gồm chiều rộng và chiều cao tối thiểu và tối đa.                                      |
| `transform`   | `Matrix4`        | Áp dụng phép biến đổi (như xoay, thu phóng, di chuyển) cho `Container`.                                                                 |
| `child`       | `Widget`         | Widget con bên trong `Container`. Chỉ có thể chứa một widget con, nhưng có thể là bất kỳ widget nào (ví dụ: `Text, Column, Row`, v.v.). |

### Khác nhau Container và Scaffold
- `Container` là widget dùng để tạo một phần tử chứa (vùng không gian) với các thuộc tính tùy chỉnh như kích thước, màu sắc, padding, margin, và trang trí. Nó chỉ chứa một widget con duy nhất.
- `Scaffold` là widget cung cấp cấu trúc chuẩn cho ứng dụng, bao gồm các thành phần cơ bản như `AppBar, Drawer, Body, BottomNavigationBar`, v.v. Nó hỗ trợ xây dựng một màn hình ứng dụng hoàn chỉnh.
> **Container** thường dùng để bố trí các widget nhỏ hoặc tạo không gian cho các widget con
> **Scaffold** là một widget cao cấp hơn, phục vụ cho việc xây dựng cấu trúc giao diện màn hình ứng dụng đầy đủ.

## Text
- `Text` là một widget được sử dụng để hiển thị chuỗi văn bản trên giao diện người dùng.
-  Đây là widget cơ bản và phổ biến nhất trong Flutter để hiển thị nội dung văn bản.

### Các thuộc tính

| Thuộc tính        | Kiểu dữ liệu     | Mô tả                                                                                                              |
| ----------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------ |
| `data`            | `String`         | Chuỗi văn bản cần hiển thị. Đây là thuộc tính bắt buộc khi sử dụng widget `Text`.                                  |
| `style`           | `TextStyle`      | Cung cấp các thuộc tính để tùy chỉnh kiểu chữ (font, kích thước, màu sắc, v.v.).                                   |
| `textAlign`       | `TextAlign`      | Định nghĩa cách căn chỉnh văn bản (trái, phải, giữa, v.v.).                                                        |
| `overflow`        | `TextOverflow`   | Xác định cách xử lý khi văn bản dài hơn không gian hiển thị. Các giá trị phổ biến: `clip, fade, ellipsis`.         |
| `maxLines	`       | `int`            | Giới hạn số dòng tối đa mà văn bản có thể chiếm.                                                                   |
| `softWrap`        | `bool`           | Xác định xem văn bản có thể xuống dòng hay không. Nếu là `true`, văn bản sẽ tự động xuống dòng khi hết không gian. |
| `textScaleFactor` | `double`         | Điều chỉnh tỷ lệ phóng đại của văn bản (ví dụ: làm cho chữ lớn hơn hoặc nhỏ hơn).                                  |
| `strutStyle`      | `StrutStyle`     | Xác định chiều cao của dòng trong văn bản, giúp điều chỉnh khoảng cách giữa các dòng chữ.                          |
| `textWidthBasis`  | `TextWidthBasis` | Xác định cách tính chiều rộng của văn bản (tính theo văn bản hoặc theo toàn bộ widget chứa nó).                    |
| `locale`          | `Locale`         | Cung cấp ngôn ngữ và quốc gia mà văn bản cần hiển thị.                                                             |

## Text Field
- `TextField` là một widget cho phép người dùng nhập dữ liệu văn bản vào ứng dụng. 
- Đây là widget cơ bản được sử dụng để xây dựng các trường nhập liệu trong ứng dụng.

### Các thuộc tính

| Thuộc tính        | Kiểu dữ liệu               | Mô tả                                                                                           |
| ----------------- | -------------------------- | ----------------------------------------------------------------------------------------------- |
| `controller`      | `TextEditingController`    | Điều khiển và theo dõi giá trị nhập vào của trường văn bản.                                     |
| `focusNode`       | `FocusNode`                | Dùng để theo dõi trạng thái focus của `TextField`.                                              |
| `decoration`      | `InputDecoration`          | Trang trí trường văn bản, như viền, biểu tượng, nhãn, v.v.                                      |
| `keyboardType`    | `TextInputType`            | Xác định loại bàn phím sẽ hiển thị (ví dụ: `TextInputType.text`, `TextInputType.number`, v.v.). |
| `obscureText`     | `bool`                     | Quyết định xem văn bản có được mã hóa (ẩn đi) không (ví dụ: dùng cho mật khẩu).                 |
| `maxLines`        | `int`                      | Giới hạn số dòng tối đa trong `TextField`.                                                      |
| `minLines`        | `int`                      | Thiết lập số dòng tối thiểu trong `TextField`.                                                  |
| maxLength         | int                        | Giới hạn số ký tự tối đa người dùng có thể nhập.                                                |
| `onChanged`       | `ValueChanged<String>?`    | Hàm callback được gọi mỗi khi giá trị trong `TextField` thay đổi.                               |
| `onSubmitted`     | `ValueChanged<String>?`    | Hàm callback được gọi khi người dùng nhấn `Enter` hoặc `Submit` trên bàn phím.                  |
| `enabled`         | `bool`                     | Xác định xem `TextField` có thể chỉnh sửa hay không.                                            |
| `readOnly`        | `bool`                     | Nếu được đặt là `true`, người dùng không thể chỉnh sửa nội dung.                                |
| `textInputAction` | `TextInputAction`          | Hành động của nút trên bàn phím (ví dụ: `TextInputAction.done`, `TextInputAction.next`).        |
| `style`           | `TextStyle`                | Chỉnh sửa kiểu chữ của văn bản trong `TextField`.                                               |
| `autofocus`       | `bool`                     | Nếu `true`, `TextField` sẽ tự động `focus` khi được hiển thị.                                   |
| `cursorColor`     | `Color`                    | Xác định màu của con trỏ trong TextField.                                                       |
| `cursorWidth`     | `double`                   | Thiết lập chiều rộng của con trỏ trong TextField.                                               |
| `scrollPadding`   | `EdgeInsets`               | Điều chỉnh khoảng cách giữa TextField và vùng cuộn khi bàn phím xuất hiện.                      |
| `inputFormatters` | `List<TextInputFormatter>` | Sử dụng để kiểm soát và sửa đổi văn bản nhập vào, như hạn chế ký tự hoặc định dạng văn bản.     |

## Button
- `Button` là một widget phổ biến dùng để tạo các nút tương tác với người dùng.
- Flutter cung cấp nhiều loại nút khác nhau, mỗi loại nút có các đặc điểm và cách sử dụng riêng.

### Các loại Button
- **Text Button**
  - Là nút đơn giản nhất, thường được sử dụng khi không muốn có nền, nhưng vẫn muốn người dùng có thể tương tác với văn bản.
  - Thường được sử dụng khi muốn có một nút mà không muốn làm rối mắt giao diện, phù hợp với các nút phụ hoặc liên kết.
- **ElevatedButton**
  - Là nút có nền nổi bật và bóng đổ, thường được sử dụng cho các hành động quan trọng hoặc nổi bật trên giao diện.
  - Sử dụng `ElevatedButton` khi muốn thu hút sự chú ý của người dùng vào nút này.
- **OutlinedButton**
  - Là nút có viền xung quanh, không có nền. Thường được sử dụng trong các tình huống cần nhấn mạnh sự lựa chọn của người dùng mà không cần làm nó quá nổi bật.
  - Thường được dùng trong các trường hợp có các nút `"hủy"` hoặc các lựa chọn không quan trọng bằng `ElevatedButton`.
- **IconButton**
  - Là một nút có biểu tượng `(icon)` mà không có văn bản. Thường được dùng trong thanh công cụ `(app bar)` hoặc bất kỳ nơi nào  chỉ cần biểu tượng để thể hiện hành động.
  - Thích hợp cho các ứng dụng cần thao tác nhanh, như quay lại, lưu trữ, chia sẻ, v.v.
- **FloatingActionButton**
  - Là nút nổi bật, thường được sử dụng để thực hiện hành động chính trong ứng dụng. 
  - `FloatingActionButton` thường được đặt ở góc dưới bên phải của màn hình.
  - Thường dùng để thêm mới hoặc thực hiện hành động quan trọng.

### Các thuộc tính của Button

| Thuộc tính    | Kiểu dữ liệu     | Mô tả                                                                                    |
| ------------- | ---------------- | ---------------------------------------------------------------------------------------- |
| `onPressed`   | `	VoidCallback?` | Hàm callback được gọi khi nút được nhấn. Nếu là `null`, nút sẽ không thể nhấn được.      |
| `child`       | `Widget`         | Nội dung của nút, có thể là văn bản (`Text`), biểu tượng (`Icon`), hoặc các widget khác. |
| `style`       | `ButtonStyle`    | Tùy chỉnh kiểu dáng của nút, ví dụ: màu nền, viền, kích thước, v.v.                      |
| `onLongPress` | `VoidCallback?`  | Hàm callback được gọi khi người dùng nhấn lâu vào nút.                                   |
| `focusNode`   | `FocusNode?`     | Dùng để theo dõi và điều khiển trạng thái focus của nút.                                 |
| `autofocus`   | `bool`           | Nếu được đặt là `true`, nút sẽ tự động nhận focus khi màn hình được hiển thị.            |

## Column, Row, Stack
### Column
- `Column` là widget cho phép  sắp xếp các widget con theo chiều dọc (theo trục y). 
- Tất cả các widget con trong Column sẽ được hiển thị từ trên xuống dưới, một cách tuần tự.
- Dùng khi muốn các widget được xếp theo chiều dọc, ví dụ như danh sách, form nhập liệu.

- **Các thuộc tính của Column**

| Thuộc tính           | Kiểu dữ liệu         | Mô tả                                                                                                                 |
| -------------------- | -------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `children`           | `List<Widget>`       | Danh sách các widget con sẽ được xếp theo chiều dọc.                                                                  |
| `mainAxisAlignment`  | `MainAxisAlignment`  | Căn chỉnh các widget theo trục chính (chiều dọc).                                                                     |
| `crossAxisAlignment` | `CrossAxisAlignment` | Căn chỉnh các widget theo trục phụ (chiều ngang).                                                                     |
| `mainAxisSize`       | `MainAxisSize`       | Quyết định chiều dài của `Column` (nếu nhỏ hơn hoặc bằng `MainAxisSize.min`, chiều cao sẽ được tối ưu theo nội dung). |

### Row
- `Row` là widget cho phép sắp xếp các widget con theo chiều ngang (theo trục x).
- Tất cả các widget con trong `Row` sẽ được hiển thị từ trái qua phải.
- Dùng khi muốn các widget được xếp theo chiều ngang, ví dụ như các nút bấm trong một thanh công cụ.

- **Các thuộc tính của Row**

| Thuộc tính           | Kiểu dữ liệu                                                            | Mô tả                                                                                                             |
| -------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `children`           | `List<Widget>`                                                          | Danh sách các widget con sẽ được xếp theo chiều ngang.                                                            |
| `mainAxisAlignment`  | `MainAxisAlignment`	Căn chỉnh các widget theo trục chính (chiều ngang). |
| `crossAxisAlignment` | `CrossAxisAlignment`	Căn chỉnh các widget theo trục phụ (chiều dọc).    |
| `mainAxisSize`       | `MainAxisSize`                                                          | Quyết định chiều dài của Row (nếu nhỏ hơn hoặc bằng `MainAxisSize.min`, chiều rộng sẽ được tối ưu theo nội dung). |

### Stack
- `Stack` là widget cho phép  xếp các widget chồng lên nhau (theo trục z). 
- Các widget con trong Stack sẽ được vẽ chồng lên nhau theo thứ tự của danh sách children. Có thể sử dụng Positioned để thay đổi vị trí của các widget trong Stack.
- Dùng khi  muốn tạo các giao diện có các phần tử chồng lên nhau, ví dụ như hình nền, các phần tử trong game, hoặc các nút nổi.

- **Các thuộc tính của Stack**

| Thuộc tính  | Kiểu dữ liệu	|Mô tả |
| ----------- | ------------------ | -------------------------------------------------------------------------------------------------- |
| `children`  | `List<Widget>`     | Danh sách các widget con trong `Stack`.                                                            |
| `alignment` | `Alignment`        | Căn chỉnh các widget con trong `Stack` (có thể căn giữa, trái, phải, v.v.).                        |
| `fit`       | `StackFit`         | Điều chỉnh kích thước của các widget con trong Stack (ví dụ: `StackFit.expand`, `StackFit.loose`). |
| `overflow`  | `Overflow`         | Xác định cách xử lý khi các widget con vượt qua kích thước của `Stack`.                            |

### So sánh giữa Column, Row, và Stack

| Widget   | Mô tả	Sử dụng	Căn chỉnh                                                                                                                                                             |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Column` | Xếp các widget theo chiều dọc (trục y).	Khi muốn các widget sắp xếp từ trên xuống dưới.	Căn chỉnh theo chiều dọc (trục y).                                                          |
| `Row`    | Xếp các widget theo chiều ngang (trục x).	Khi muốn các widget sắp xếp từ trái qua phải.	Căn chỉnh theo chiều ngang (trục x).                                                        |
| `Stack`  | Xếp các widget chồng lên nhau (trục z).	Khi muốn các widget chồng lên nhau, ví dụ: hình nền hoặc các phần tử nổi.	Căn chỉnh theo trục z, sử dụng `Positioned` để điều chỉnh vị trí. |