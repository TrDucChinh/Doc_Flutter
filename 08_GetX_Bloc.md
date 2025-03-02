# GetX và Bloc
## GetX
- `dependencies: get`
### State Management trong GetX
- **GetX** hỗ trợ nhiều cách quản lý state khác nhau, trong đó có `Reactive State, Simple State và Mix State`

#### Reactive State (Rx)
- Cách sử dụng phổ biến nhất, giúp UI tự động cập nhật khi giá trị thay đổi mà không cần `setState()`.
 
- **Ví dụ: Counter App với Rx**

```dart
class CounterController extends GetxController {
  var count = 0.obs; // .obs biến này thành reactive

  void increment() {
    count++; // UI sẽ tự động cập nhật
  }
}
```

```dart
class CounterScreen extends StatelessWidget {
  final CounterController controller = Get.put(CounterController());

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Counter App")),
      body: Center(
        child: Obx(() => Text("Count: ${controller.count}", style: TextStyle(fontSize: 30))),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: controller.increment,
        child: Icon(Icons.add),
      ),
    );
  }
}
```

- count = 0.obs; làm biến này thành reactive.
- Obx(() => Text(...)) tự động cập nhật UI khi count thay đổi.
- controller.increment(); cập nhật giá trị và UI sẽ tự động thay đổi.

#### Simple State (GetBuilder)
- Không dùng `obs`, cập nhật UI bằng `update()`

- **Ví dụ: Counter App với GetBuilder**

```Dart
class CounterController extends GetxController {
  int count = 0;

  void increment() {
    count++;
    update(); // Cập nhật UI
  }
}
```

```dart
class CounterScreen extends StatelessWidget {
  final CounterController controller = Get.put(CounterController());

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Counter App")),
      body: Center(
        child: GetBuilder<CounterController>(
          builder: (controller) => Text("Count: ${controller.count}", style: TextStyle(fontSize: 30)),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: controller.increment,
        child: Icon(Icons.add),
      ),
    );
  }
}
```
-  **Khi nào dùng?**
   - Khi không cần theo dõi liên tục (ít cập nhật).
   - Khi muốn tối ưu hiệu suất (không rebuild toàn bộ widget).

#### Mix State (Rx + update())
- Có thể kết hợp cả hai cách trên, nhưng nên dùng một cách chính để tránh rối code.

```dart
class CounterController extends GetxController {
  var count = 0.obs;

  void increment() {
    count++;
    update(); // Vẫn có thể cập nhật UI thủ công nếu cần
  }
}
```

### Navigation trong GetX
- GetX giúp điều hướng giữa các màn hình mà không cần `context`

- **Chuyển màn hình**

```dart
Get.to(SecondScreen());  // Chuyển màn hình mới
Get.off(SecondScreen()); // Chuyển và xóa màn hình trước
Get.offAll(SecondScreen()); // Xóa tất cả màn hình trước và mở màn hình mới
```

- **Chuyền dữ liệu giữa các màn**

```dart
Get.to(SecondScreen(), arguments: "Hello from HomeScreen");

// Nhận
var data = Get.arguments;
```

- **Điều hướng có Named Route**

```dart
void main() {
  runApp(GetMaterialApp(
    initialRoute: "/",
    getPages: [
      GetPage(name: "/", page: () => HomeScreen()),
      GetPage(name: "/second", page: () => SecondScreen()),
    ],
  ));
}

Get.toNamed("/second");

```

### Themes trong GetX
- GetX hỗ trợ thay đổi theme động mà không cần restart app.

```dart
// Cấu hình theme

void main() {
  runApp(GetMaterialApp(
    theme: ThemeData.light(),
    darkTheme: ThemeData.dark(),
    themeMode: ThemeMode.system,
    home: HomeScreen(),
  ));
}

// chuyển đổi theme động

void changeTheme() {
  Get.isDarkMode ? Get.changeTheme(ThemeData.light()) : Get.changeTheme(ThemeData.dark());
}

// Dùng  trong UI
ElevatedButton(
  onPressed: changeTheme,
  child: Text("Change Theme"),
)

```

### Localizations trong GetX

- GetX hỗ trợ đa ngôn ngữ mà không cần MaterialApp.

```Dart
// Tạo file dịch

class MyTranslations extends Translations {
  @override
  Map<String, Map<String, String>> get keys => {
    'en_US': {'hello': 'Hello'},
    'vi_VN': {'hello': 'Xin chào'},
  };
}


// Cấu hình GetMaterialApp

void main() {
  runApp(GetMaterialApp(
    translations: MyTranslations(),
    locale: Locale('en', 'US'),
    fallbackLocale: Locale('en', 'US'),
    home: HomeScreen(),
  ));
}

// Sử dụng trong UI

Text('hello'.tr) // Sẽ tự động dịch

// Chuyển đổi da ngôn ngữ
Get.updateLocale(Locale('vi', 'VN'));
```

|Tính năng|GetX hỗ trợ|
|---|---|
|State Management	|Reactive (Rx), Simple (GetBuilder), Mix|
|Navigation	|Không cần `context`, hỗ trợ Named Routes|
|Themes	|Thay đổi theme động bằng `Get.changeTheme()`|
|Localizations	|Dịch UI dễ dàng với `.tr` và `Get.updateLocale()`|

## Bloc
### Bloc được xây dựng dựa trên cái gì?

- **Bloc (Business Logic Component)** được xây dựng dựa trên `Streams` và `Events` trong Dart.
- Nó giúp quản lý trạng thái ứng dụng một cách tách biệt, reactive và dễ kiểm soát.

- BLoC là một mẫu thiết kế cho phép tách biệt logic (business logic) của ứng dụng khỏi giao diện người dùng (UI). 
- BLoC sử dụng các luồng (stream) để truyền tải các sự kiện (event) từ UI đến logic và các trạng thái (state) từ logic đến UI. 
- Bằng cách này, có thể tái sử dụng được logic của ứng dụng cho nhiều nền tảng khác nhau, ví dụ như web hay desktop, mà không cần thay đổi UI.

#### Ưu điểm
- BLoC giúp giảm sự phụ thuộc giữa UI và logic, làm cho code của dễ bảo trì, mở rộng và kiểm thử hơn.
- BLoC cho phép tái sử dụng được logic của ứng dụng cho nhiều screen khác nhau, tiết kiệm thời gian và công sức phát triển.
- BLoC sử dụng các luồng để truyền tải các sự kiện và trạng thái, giúp xử lý các trường hợp phức tạp như debounce, throttle, async-await, error handling, ... một cách đơn giản và hiệu quả.

#### Nhược điểm
- BLoC có thể làm cho code trở nên phức tạp hơn nếu không tuân thủ các nguyên tắc thiết kế và tổ chức code hợp lý.
- BLoC có thể gây ra hiện tượng memory leak nếu không đóng các luồng khi không cần thiết hoặc không sử dụng các widget quản lý vòng đời như `BlocProvider` hay `BlocBuilder`.
- BLoC có thể khiến mất nhiều thời gian để học và làm quen với cách sử dụng các luồng và các công cụ hỗ trợ như `bloc`library hay `flutter_bloc` library.
###  Nguyên lý hoạt động
- **Mô hình hoạt động của Bloc:**
  - **Sự kiện (Event)** xảy ra (người dùng nhấn nút, nhận dữ liệu từ API, v.v.).
  - **Bloc nhận sự kiện**, xử lý logic và phát ra một trạng thái mới (State).
  - **UI lắng nghe Bloc**, khi trạng thái thay đổi, UI được cập nhật.

- **Bloc gồm các thành phần sau:**
  - **Event:** Là những sự kiện được gửi từ UI, như nhấn nút, vuốt màn hình, nhập liệu... Event được tiếp nhận bởi Bloc và xử lý theo nghiệp vụ.
  - **State:** Là những trạng thái của ứng dụng, được gửi lên bởi Bloc. Khi State thay đổi, UI sẽ được thông báo và cập nhật lại theo State mới.
  - **Transition:** Là sự chuyển đổi từ một State sang một State khác, bao gồm State hiện tại, Event và State tiếp theo.
  - **Bloc:** Là lớp trung gian giữa UI và Data Layer. Bloc có nhiệm vụ nhận Event từ UI, xử lý logic nghiệp vụ và trả về State mới cho UI. Bloc cũng có thể gọi đến Data Layer để lấy dữ liệu từ các nguồn khác nhau, như network, database, shared preferences...
  - **BlocProvider:** Là một widget giúp cung cấp Bloc cho các widget con trong cây widget. BlocProvider giúp tái sử dụng Bloc ở nhiều màn hình khác nhau và giải phóng tài nguyên khi không cần thiết.

- **So sánh Bloc với setState()**

|Bloc|	setState()|
|---|---|
|Quản lý |state	Tách biệt logic và UI|	Trực tiếp trong UI
|Reactive	|Dựa trên Stream	|Không reactive|
|Hiệu suất	|Tối ưu, chỉ rebuild khi state thay đổi	|Dễ gây rebuild toàn bộ UI

- **Để sử dụng Bloc trong Flutter, ta cần làm các bước sau:**
  - Tạo Event cho mỗi chức năng cần xử lý logic.
  - Tạo State cho mỗi trạng thái của ứng dụng.
  - Tạo Bloc kế thừa từ lớp Bloc của bloc package. Định nghĩa State ban đầu và implement hàm on<Event> để ánh xạ từ Event sang State mới.
  - Tạo BlocProvider trong main.dart để cung cấp Bloc cho toàn bộ ứng dụng hoặc cho một phần của ứng dụng.
  - Sử dụng BlocBuilder hoặc BlocConsumer để xây dựng UI dựa trên State của Bloc. Sử dụng hàm context.read<Bloc>() hoặc context.watch<Bloc>() để lấy tham chiếu đến Bloc và gửi Event.

### Bloc Pattern
- **Bloc pattern** là kiến trúc chia tách business logic ra khỏi UI, gồm ba phần:
  - **Event:** Người dùng tương tác với app.
  - **State:** Trạng thái ứng dụng sau khi Bloc xử lý Event.
  - **Bloc:** Nhận Event, xử lý logic, rồi phát ra State mới.
- **Mô hình Bloc Pattern:** `User Action → Event → Bloc → State → UI
`
- Ví dụ: Người dùng nhấn nút (+) → Gửi `IncrementEvent` → Bloc nhận Event, tăng biến `count` → Phát `CounterState(count)` → UI cập nhật.

### Các Widget của Bloc

|Widget	|Chức năng|
|---|---|
|BlocProvider	|Cung cấp một instance Bloc/Cubit cho widget con
|BlocBuilder|	Lắng nghe state và build UI khi state thay đổi
|BlocListener	|Lắng nghe state nhưng không rebuild UI (dùng cho Side Effects)
|BlocConsumer	|Kết hợp cả BlocBuilder và BlocListener
|MultiBlocProvider|	Cung cấp nhiều Bloc/Cubit cùng lúc
|MultiBlocListener	|Lắng nghe nhiều Bloc cùng lúc

#### BlocProvider – Cung cấp Bloc cho widget con
- `BlocProvider` giúp cung cấp một instance của Bloc hoặc Cubit xuống các widget con trong cây widget.
- **Lưu ý:** BlocProvider sẽ tự động hủy Bloc khi widget bị dispose.
- **Khi nào dùng:** Khi muốn khởi tạo và cung cấp một instance Bloc/Cubit cho widget con.

```dart
BlocProvider(
  create: (context) => CounterBloc(), // Tạo một instance của CounterBloc
  child: CounterScreen(), // Widget con có thể truy cập CounterBloc
)
```

#### BlocBuilder – Build UI dựa trên state
- `BlocBuilder` giúp theo dõi và rebuild UI khi state thay đổi.
- **Lưu ý:**
  - BlocBuilder chỉ rebuild UI khi state thực sự thay đổi.
  - Nó không gây re-build toàn bộ widget tree, chỉ re-build widget bên trong nó.
- **Khi nào dùng:** Khi UI cần thay đổi dựa trên state (ví dụ: hiển thị danh sách, số đếm, trạng thái tải dữ liệu).

```dart
BlocBuilder<CounterBloc, CounterState>(
  builder: (context, state) {
    return Text(
      '${state.count}', // Hiển thị giá trị đếm từ state
      style: TextStyle(fontSize: 24),
    );
  },
)
```

#### BlocListener – Lắng nghe state nhưng không build UI
- `BlocListener` không build UI, nhưng theo dõi state và thực hiện Side Effects như hiển thị SnackBar, điều hướng trang, hoặc hiển thị Dialog.
- **Lưu ý:**
  - Chỉ chạy khi state thay đổi.
  - Không làm ảnh hưởng đến việc build UI.
- **Khi nào dùng:** Khi cần thực hiện tác vụ phụ (Side Effects) mà không làm ảnh hưởng đến UI, như hiển thị thông báo, popup, hoặc điều hướng màn hình.

```dart
BlocListener<CounterBloc, CounterState>(
  listener: (context, state) {
    if (state.count == 5) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text("Bạn đã đạt đến 5!"))
      );
    }
  },
  child: Container(), // Không build UI
)
```

#### BlocConsumer – Kết hợp BlocBuilder và BlocListener
- BlocConsumer kết hợp cả BlocBuilder và BlocListener để vừa lắng nghe state, vừa build UI.

```dart
BlocConsumer<CounterBloc, CounterState>(
  listener: (context, state) {
    if (state.count == 5) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text("Đã đạt 5!"))
      );
    }
  },
  builder: (context, state) {
    return Text('${state.count}');
  },
)
```

- **Lưu ý:**
  - **listener** xử lý Side Effects.
  - **builder** dùng để cập nhật UI.

- **Khi nào dùng:** Khi vừa cần cập nhật UI, vừa cần thực hiện Side Effects.

#### MultiBlocProvider – Cung cấp nhiều Bloc cùng lúc
- Khi ứng dụng có nhiều Bloc/Cubit, ta có thể dùng `MultiBlocProvider` để cung cấp nhiều Bloc mà không bị lặp code.

```dart
MultiBlocProvider(
  providers: [
    BlocProvider(create: (context) => CounterBloc()),
    BlocProvider(create: (context) => AuthBloc()),
  ],
  child: MyApp(),
)

```
- **Lưu ý:** `MultiBlocProvider` giúp giảm lặp code khi có nhiều Bloc/Cubit.
- **Khi nào dùng:** Khi ứng dụng có nhiều Bloc cần được cung cấp cùng lúc.

#### MultiBlocListener – Lắng nghe nhiều Bloc cùng lúc
- `MultiBlocListener` giúp lắng nghe nhiều Bloc mà không cần viết nhiều `BlocListener`.

```dart
MultiBlocListener(
  listeners: [
    BlocListener<AuthBloc, AuthState>(
      listener: (context, state) {
        if (state is Authenticated) {
          Navigator.pushNamed(context, "/home");
        }
      },
    ),
    BlocListener<CartBloc, CartState>(
      listener: (context, state) {
        if (state is CartUpdated) {
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(content: Text("Giỏ hàng đã cập nhật!"))
          );
        }
      },
    ),
  ],
  child: MyApp(),
)
```

- **Lưu ý:** Giúp quản lý nhiều Bloc Listener một cách gọn gàng.
- **Khi nào dùng:** Khi cần lắng nghe nhiều Bloc và xử lý Side Effects.

- **Khi nào dùng cái nào?**

|Trường hợp	|Dùng Widget nào?|
|---|---|
|Cung cấp Bloc cho widget con	|BlocProvider|
|UI cần cập nhật khi state thay đổi	|BlocBuilder|
|Thực hiện Side Effects (hiển thị thông báo, điều hướng)|	BlocListener|
|Cả cập nhật UI + Side Effects|	BlocConsumer|
|Cung cấp nhiều Bloc|	MultiBlocProvider|
|Lắng nghe nhiều Bloc cùng lúc|	MultiBlocListener|

### Single State, Multi State
#### Single State
- Bloc chỉ có **một state duy nhất** chứa toàn bộ dữ liệu cần thiết.
- Mỗi lần state thay đổi, **toàn bộ state** sẽ được thay thế bằng một đối tượng mới.
- Thường được dùng khi ứng dụng có dữ liệu **liên quan chặt chẽ** và không thay đổi quá nhiều.

```dart
import 'package:flutter_bloc/flutter_bloc.dart';

// Định nghĩa State duy nhất
class CounterState {
  final int count;
  CounterState(this.count);
}

// Định nghĩa Event
abstract class CounterEvent {}
class Increment extends CounterEvent {}
class Decrement extends CounterEvent {}

// Bloc sử dụng Single State
class CounterBloc extends Bloc<CounterEvent, CounterState> {
  CounterBloc() : super(CounterState(0)) {
    on<Increment>((event, emit) {
      emit(CounterState(state.count + 1));
    });

    on<Decrement>((event, emit) {
      emit(CounterState(state.count - 1));
    });
  }
}
```
- **Ưu điểm**
  -  Đơn giản, dễ hiểu, dễ quản lý
  -   Tránh được các vấn đề khi chuyển đổi giữa nhiều state

- **Nhược điểm**
  - Mỗi lần state thay đổi, **toàn bộ state bị thay thế**, có thể gây hiệu suất kém nếu state chứa nhiều dữ liệu.
  - Không phù hợp khi có nhiều trạng thái khác nhau, vì sẽ cần kiểm tra điều kiện hoặc tạo các flag trong state.

#### Multi State Bloc
- **Khái niệm**
  - Bloc có nhiều state khác nhau, mỗi state biểu diễn một trạng thái riêng biệt.
  - Thích hợp khi có nhiều trạng thái rõ ràng, ví dụ như `loading, success, error`.
  - Mỗi khi có sự thay đổi, `chỉ cần phát state mới phù hợp`.

```dart
import 'package:flutter_bloc/flutter_bloc.dart';

// Định nghĩa các state khác nhau
abstract class CounterState {}
class CounterInitial extends CounterState {}
class CounterLoading extends CounterState {}
class CounterLoaded extends CounterState {
  final int count;
  CounterLoaded(this.count);
}
class CounterError extends CounterState {}

// Định nghĩa Event
abstract class CounterEvent {}
class Increment extends CounterEvent {}
class Decrement extends CounterEvent {}

// Bloc sử dụng Multi State
class CounterBloc extends Bloc<CounterEvent, CounterState> {
  int _count = 0;

  CounterBloc() : super(CounterInitial()) {
    on<Increment>((event, emit) async {
      emit(CounterLoading()); // Hiển thị trạng thái loading
      await Future.delayed(Duration(seconds: 1)); // Giả lập xử lý
      _count++;
      emit(CounterLoaded(_count)); // Cập nhật state mới
    });

    on<Decrement>((event, emit) async {
      emit(CounterLoading());
      await Future.delayed(Duration(seconds: 1));
      _count--;
      emit(CounterLoaded(_count));
    });
  }
}
```
- **Ưu điểm**
  - Tối ưu hiệu suất, chỉ thay đổi từng phần nhỏ của state.
  - Dễ quản lý khi ứng dụng có nhiều trạng thái khác nhau.
  - Tránh lỗi do state không rõ ràng.

- **Nhược điểm**
  - Phức tạp hơn single state
  - Cần xác định rõ từng state để tránh rối khi quản lý

#### Khi nào dùng Single State và Multi State?

|Tiêu chí	|Single State	|Multi State|
|---|---|---|
|Ứng dụng nhỏ, đơn giản|	Phù hợp	| Không cần thiết|
|Ứng dụng có nhiều trạng thái khác nhau (loading, success, error, etc.)|	 Không phù hợp|	 Nên sử dụng|
|Hiệu suất cao|	 Không tối ưu nếu state lớn	| Tối ưu hơn|
|Dễ triển khai, dễ hiểu	| Đơn giản hơn| Phức tạp hơn|

### context.read<T>(); context.watch<T>(); context.select<T, R>((T bloc) => R property)
#### context.read<T>()

- Dùng để lấy **instance của Bloc/Provider** nhưng **không lắng nghe thay đổi state**.
- Chỉ nên dùng khi muốn gọi một **method hoặc dispatch** một event đến Bloc mà không cần rebuild UI.

```dart
ElevatedButton(
  onPressed: () {
    context.read<CounterBloc>().add(Increment());
  },
  child: Text("Tăng"),
)
```
>context.read<T>() không rebuild widget khi state thay đổi.
>Nếu dùng trong build(), nó chỉ lấy Bloc một lần và không theo dõi sự thay đổi.

#### context.watch<T>()
- Dùng để lắng nghe state và rebuild widget mỗi khi state thay đổi.
- Nên dùng khi UI phụ thuộc vào giá trị state.

```dart
Text(
  'Giá trị: ${context.watch<CounterBloc>().state.count}',
  style: TextStyle(fontSize: 20),
)
```
>Khi state thay đổi, widget chứa context.watch sẽ được rebuild.
>Tránh gọi context.watch() trong onPressed hoặc logic xử lý sự kiện vì nó gây lỗi "Looking up a scoped widget was called outside of a build method".

#### context.select<T, R>((T bloc) => R property)
- Giúp lắng nghe một phần nhỏ của state, thay vì lắng nghe toàn bộ state như context.watch().
- Dùng để tránh rebuild không cần thiết nếu chỉ quan tâm đến một giá trị cụ thể trong state.

```dart
Text(
  'Giá trị: ${context.select<CounterBloc, int>((bloc) => bloc.state.count)}',
  style: TextStyle(fontSize: 20),
)
```
>Chỉ rebuild khi giá trị count thay đổi, không bị ảnh hưởng bởi các phần khác của state.
>Dùng để tối ưu hiệu suất khi state có nhiều thuộc tính nhưng chỉ cần theo dõi một giá trị.

#### Khi nào dùng gì?

|Phương thức	|Mục đích	|Rebuild UI?|
|---|---|---|
|context.read<T>()	|Lấy instance của Bloc/Provider để gọi event/method	| Không|
|context.watch<T>()	|Theo dõi toàn bộ state, rebuild UI khi state thay đổi	 |Có|
|context.select<T, R>	|Theo dõi một phần của state, chỉ rebuild khi giá trị đó thay đổi	| Có (nhưng tối ưu hơn)|