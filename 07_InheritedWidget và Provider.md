# InheritedWidget và Provider
## InheritedWidget
- [Sample](https://dartpad.dev/?id=157b19bb180a0d7ea4d738946c4e9bf1)
- Cách truyền data trước kia là truyền qua `constructor`
- Kỹ thuật truyền data từ widget cha xuống Widget con thông qua constructor như vậy được gọi là `"Passing state down".`

![](https://images.viblo.asia/1e93a0db-f337-4104-8f63-436021e1189c.png)

- Nếu chúng ta extract widget `Center` ra một `Widget` đặt tên là `MyCenterWidget` thì ta cũng phải tạo một `constructor` với 2 property là counter và isLoading để nó có thể nhận data từ `MyHomePage` truyền xuống cho `CounterWidget.`

![](https://images.viblo.asia/511bb6d8-20fb-4f02-ba97-b14ff1bc9bd1.png)

![](https://images.viblo.asia/1bef725b-50dc-4488-86aa-549d09164213.png)

- Nếu cứ làm theo cách như vậy, từ một widget ông muốn truyền data xuống widget cháu, ta phải truyền sang tay từng người một, `từ ông → ba → con → cháu`

- Vấn đề xảy ra:
  - Nếu khoảng cách giữa `MyHomePage` và `CounterWidget` trên Widget tree càng xa thì chúng ta sẽ càng cực khổ để truyền được data từ `MyHomePage` xuống `CounterWidget` qua constructor của nhiều Widget trung gian.
  - Khi `MyHomePage` gọi hàm `setState`, nó sẽ gọi lại hàm build khiến cho cả `MyCenterWidget` và `CounterWidget` đều được khởi tạo lại và gọi hàm build. Đây là 1 sự lãng phí vì ta chỉ cần widget `CounterWidget` được rebuild mà thôi.

=> Có **InheritedWidget** giúp làm điều đó
- Là một nơi lưu trữ data và cung cấp data cho widget con trong widget tree.
- Tất cả widget con của `InheritedWidget` đều có thể truy cập vào `InheritedWidget` để lấy data
- Tức là từ vị trí `InheritedWidget`, bạn không cần thiết phải truyền data xuống từng 1 widget con một nữa mà Widget con ở bất kỳ vị trí nào trên widget tree muốn lấy data từ `InheritedWidget`

![](https://images.viblo.asia/98e3c9de-5655-4e5a-9311-b7bee943a4d6.PNG)

### Cách sử dụng

- Tạo class `InheritedWidget`

```dart
class [name] extends InheritedWidget {
  final Widget child;
  [attributes]

  constructor

  [name] ({
    required attributes

    required this.child,
  }) : super(child: child);

  @override
  bool updateShouldNotify(MyInheritedWidget oldWidget) {
    return false;
  }
  // Để các widget con tìm thấy InheritedWidget thì ta tạo 1 hàm of(context).Các Widget con sẽ cần gọi MyInheritedWidget.of(context) để dựa vào context đó (tức vị trí của widget con), hàm of sẽ đi tìm thằng widget cha có type là MyInheritedWidget và trả về MyInheritedWidget đó.

  static [name]? of(BuildContext context){
    // Để tìm kiếm chúng ta sử dụng hàm context.dependOnInheritedWidgetOfExactType<MyInheritedWidget>. Nó sẽ giúp ta get được Widget cha gần vị trí context nhất có type là MyInheritedWidget
    return context.dependOnInheritedWidgetOfExactType<[name]>();
  }
}
```

- Ví dụ:

```Dart
class MyInheritedWidget extends InheritedWidget {
  final int counter;
  final bool isLoading;
  final Widget child;
  
  MyInheritedWidget({
    required this.isLoading,
    required this.counter,
    required this.child,
  }) : super(child: child);  
  
  @override
  bool updateShouldNotify(MyInheritedWidget oldWidget) {
    return true;
  }
  
  static MyInheritedWidget? of(BuildContext context) {
    return context.dependOnInheritedWidgetOfExactType<MyInheritedWidget>();
  }
}

```
- Hàm `updateShouldNotify`:
  - Hàm `updateShouldNotify` được gọi ngay sau khi `InheritedWidget` bị `rebuild`
  - Nếu hàm `updateShouldNotify return true` thì một khi `InheritedWidget rebuild`, nó cũng bắt các widget con đang phụ thuộc vào nó phải rebuild. 
  - Như thế nào được gọi là phụ thuộc?. Các widget con của `MyInheritedWidget`, nếu sử dụng hàm `MyInheritedWidget.of(context)` thì ta sẽ nói Widget đó sẽ phụ thuộc vào `MyInheritedWidget`.



## Provider Là Gì?
Provider là một thư viện quản lý trạng thái phổ biến trong Flutter, giúp chia sẻ dữ liệu giữa các widget một cách hiệu quả mà không cần truyền qua constructor.

## Các Loại Provider Và Cách Sử Dụng
### Provider (Dùng Cho Dữ Liệu Không Thay Đổi)
- Dùng để cung cấp một giá trị cố định.
- Không hỗ trợ cập nhật UI.
```dart
Provider<String>(
  create: (_) => "Nguyễn Văn A",
  child: MyApp(),
);
```

### 2.2. ChangeNotifierProvider (Quản Lý Trạng Thái Thay Đổi)
- Dùng để cập nhật UI khi trạng thái thay đổi.
- Cần sử dụng `ChangeNotifier`.
```dart
class CounterProvider extends ChangeNotifier {
  int _count = 0;
  int get count => _count;
  void increment() {
    _count++;
    notifyListeners();
  }
}
```
```dart
ChangeNotifierProvider(
  create: (context) => CounterProvider(),
  child: MyApp(),
);
```

### FutureProvider (Dữ Liệu Bất Đồng Bộ - API, Future)
- Dùng khi cần lấy dữ liệu từ API hoặc xử lý bất đồng bộ.
- Tự động cập nhật UI khi dữ liệu sẵn sàng.
```dart
FutureProvider<String>(
  create: (_) async => fetchData(),
  initialData: "Đang tải...",
);
```

### StreamProvider (Dữ Liệu Luôn Cập Nhật - Firebase, WebSocket)
- Dùng khi cần lắng nghe thay đổi từ `Stream`.
```dart
StreamProvider<int>(
  create: (_) => numberStream(),
  initialData: 0,
);
```

### MultiProvider (Dùng Nhiều Provider Cùng Lúc)
- Khi có nhiều Provider cần sử dụng trong ứng dụng.
```dart
MultiProvider(
  providers: [
    ChangeNotifierProvider(create: (_) => CounterProvider()),
    FutureProvider<String>(create: (_) async => fetchData(), initialData: "Đang tải..."),
  ],
  child: MyApp(),
);
```

## Cách Lấy Dữ Liệu Từ Provider
Flutter cung cấp nhiều cách để lấy dữ liệu từ Provider, tùy thuộc vào nhu cầu sử dụng:

### Sử dụng `Provider.of<T>(context)`
- **Khi nào nên dùng?** Khi cần truy cập dữ liệu và tự động rebuild UI khi dữ liệu thay đổi.
- **Nhược điểm:** Có thể gây rebuild không cần thiết nếu gọi ở widget cấp cao.
```dart
final counter = Provider.of<CounterProvider>(context);
Text('Giá trị: ${counter.count}');
```

### Sử dụng `context.watch<T>()`
- **Khi nào nên dùng?** Khi cần theo dõi thay đổi của dữ liệu và rebuild UI tự động.
- **Ưu điểm:** Viết gọn hơn so với `Provider.of<T>(context)`.
```dart
Text('Giá trị: ${context.watch<CounterProvider>().count}');
```

### Sử dụng `context.read<T>()`
- **Khi nào nên dùng?** Khi chỉ cần truy cập dữ liệu một lần hoặc gọi một phương thức mà không cần rebuild UI.
- **Ưu điểm:** Tránh rebuild UI không cần thiết.
```dart
ElevatedButton(
  onPressed: () => context.read<CounterProvider>().increment(),
  child: Text("Tăng"),
);
```

### Sử dụng `Consumer<T>()`
- **Khi nào nên dùng?** Khi muốn chỉ rebuild một phần nhỏ của UI thay vì toàn bộ widget.
- **Ưu điểm:** Tối ưu hiệu suất, tránh rebuild không cần thiết.
```dart
Consumer<CounterProvider>(
  builder: (context, counter, child) {
    return Text('Giá trị: ${counter.count}');
  },
);
```

### Sử dụng `Selector<T, R>()`
- **Khi nào nên dùng?** Khi chỉ muốn lắng nghe một phần dữ liệu của Provider thay vì toàn bộ.
- **Ưu điểm:** Tăng hiệu suất bằng cách tránh rebuild khi dữ liệu không liên quan thay đổi.
```dart
Selector<CounterProvider, int>(
  selector: (context, provider) => provider.count,
  builder: (context, count, child) {
    return Text('Giá trị: $count');
  },
);
```

##  Khi Nào Nên Dùng Provider?
| Trường hợp | Dùng Provider nào? |
|------------|-------------------|
| Chia sẻ dữ liệu tĩnh | `Provider` |
| Trạng thái thay đổi thường xuyên | `ChangeNotifierProvider` |
| Dữ liệu bất đồng bộ (API, Future) | `FutureProvider` |
| Dữ liệu liên tục thay đổi (Firebase, WebSocket) | `StreamProvider` |
| Dùng nhiều Provider cùng lúc | `MultiProvider` |

##  Kết Luận
- **Provider** giúp quản lý trạng thái dễ dàng, tách biệt logic và UI.
- **ChangeNotifierProvider** là lựa chọn phổ biến nhất.
- **FutureProvider** và **StreamProvider** phù hợp cho dữ liệu bất đồng bộ.
- **MultiProvider** giúp tổ chức code tốt hơn khi có nhiều trạng thái.

Provider là một giải pháp mạnh mẽ và dễ sử dụng, phù hợp cho hầu hết các ứng dụng Flutter!


