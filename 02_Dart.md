# Syntax, Function, Asynchronous, Class, OOP, With && Mixin
## Syntax
### Biến và kiểu dữ liệu
- Trong **Dart** các từ khóa `var, dynamic, final, const` đều được dùng khai báo biến
  - `var`: Tự động xác định kiểu dữ liệu khi khởi tạo
    - Biến có thể thay đổi giá trị, nhưng phải cùng kiểu dữ liệu đã được gán ban đầu.
```dart
var name = "Dart"; // Tự hiểu là String
name = "Flutter";  // Hợp lệ
// name = 42;      // Lỗi vì 42 không phải là String
```
  - `dynamic`: **Kiểu dữ liệu động**, cho phép thay đổi kiểu dữ liệu trong quá trình thực thi. 
    - Sử dụng `dynamic` sẽ làm mất tính an toàn kiểu (type safety), vì vậy cần hạn chế sử dụng.
  - `final`: Không thể thay đổi giá trị sau khi được khởi tạo. (Gán tại runtime)
    - Giá trị được khởi tạo một lần duy nhất khi chương trình chạy.
  -  `const`: Hằng số bất biến (compile-time constant).
     - Giá trị phải được xác định tại thời điểm biên dịch (compile time).
#### Kiểu dữ liệu
- **Kiểu dữ liệu cơ bản:**
  - `int:` Số nguyên.
  - `double:` Số thực.
  - `String:` Chuỗi.
  - `bool:` true/false
  - `List:` Danh sách 
  - `Map:` Key- value.
```dart
int number = 10;
double pi = 3.14;
String name = "Dart";
bool isActive = true;
List<int> numbers = [1, 2, 3];
Map<String, int> scores = {"Alice": 90, "Bob": 85};
```
### Nullable
- **Non-nullable (int, String, ...):** Mặc định, các kiểu dữ liệu không thể nhận giá trị null.
- **Nullable (int?, String?, ...):** Thêm dấu ? sau kiểu dữ liệu để biến có thể nhận giá trị null.
```dart
int a = 10;       // Non-nullable: Không thể gán null
// a = null;      // Lỗi: Không thể gán null

int? b;           // Nullable: Có thể nhận null
b = null;         // Hợp lệ
b = 20;           // Hợp lệ
```
#### Xử Lý Biến Nullable
- **Kiểm tra null trước khi sử dụng**: Dùng if hoặc `?`
```Dart
String? name  // Nullable type. Can be `null` or string.

String name   // Non-nullable type. Cannot be `null` but can be string.
```
- **Toán tử gán giá trị mặc định `??`**: Sử dụng để cung cấp giá trị thay thế nếu biến có giá trị null.
```Dart
int? x;
int y = x ?? 10; // Nếu x là null, y sẽ là 10.
```
- **Null-aware method call `(?.)`**: Sử dụng để gọi phương thức trên đối tượng có thể null.
```Dart
String? name;
print(name?.toUpperCase()); // Không lỗi, trả về null nếu name là null.
```
- **Null assertion operator `(!):`** Dùng để thông báo với Dart rằng một biến không phải null 
```Dart
String? name;
print(name!); // Nếu name là null, sẽ ném lỗi tại runtime.
```
- **Null-aware cascade `(?..)`**: Dùng để gọi chuỗi các phương thức nếu đối tượng không null.
```Dart
myObject?..doSomething()..doAnotherThing();
```
### Late variables
- Sử dụng trong 2 trường hợp:
  - Khai báo 1 biến non-nullable được khởi tạo sau khi khai báo
  - Lazy init variable
- Khai báo: `late` [data type] data_name;
- Truy cập biến late trước khi khởi tạo sẽ lỗi
```Dart
late String description;
print(description) // Lỗi
description = 'This is a late variable'; // Gán giá trị sau
print(description); // Kết quả: This is a late variable
```
### Function
- `Dart` là ngôn ngữ hướng đối tượng, vì vậy hàm cũng là object với kiểu dữ liệu Function. Có thể
- Gán hàm vào biến
```Dart
void sayHello(String name) {
  print('Hello, $name!');
}

void main() {
  // Gán hàm vào một biến
  var greet = sayHello;

  // Gọi hàm thông qua biến
  greet('Alice'); // Output: Hello, Alice!
}
```
- Truyền hàm làm tham số cho hàm khác
```Dart
void printMessage(Function(String) messageFunction, String message) {
  messageFunction(message);
}

void customPrint(String msg) {
  print('Custom Message: $msg');
}

void main() {
  // Truyền hàm customPrint làm tham số
  printMessage(customPrint, 'Dart is awesome!'); 
  // Output: Custom Message: Dart is awesome!
}
```
  - Gọi 1 instance của lớp như 1 hàm 
```Dart
class Adder {
  final int value;
  Adder(this.value);

  // Gọi instance như một hàm
  int call(int anotherValue) {
    return value + anotherValue;
  }
}

void main() {
  var addFive = Adder(5);

  // Gọi instance như một hàm
  print(addFive(10)); // Output: 15
}
```
- Khai báo hàm: `[data type] func_name(param)` hoặc `func_name(param)`
```Dart
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}

isNoble(atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```
- Hàm biểu thức có thể rút gọn nhờ `=>`
```Dart
bool isOdd(int value) => value % 2 == 1;
```
#### Required Positional Parameters
- Đây là những tham số `bắt buộc` phải được truyền khi gọi hàm 
- Thứ tự truyền vào phải đúng theo định nghĩa 
```Dart
void greet(String name, int age) {
  print('Hello $name, you are $age years old.');
}
void main(){
  greet('Alice', 25);
}
```
#### Optional Positional Parameters
- Tham số tùy chọn được truyền theo vị trí, có thể bỏ qua.
- **Cách dùng**:
  - Các tham số này được khai báo trong cặp dấu [].
- **Đặc điểm**:
  - Có thể không truyền giá trị.
  - Nếu không truyền, giá trị mặc định là null (hoặc bạn có thể gán giá trị mặc định).
```Dart
void greet(String name, [int? age]) {
  if (age != null) {
    print('Hello, $name! You are $age years old.');
  } else {
    print('Hello, $name!');
  }
}

void main() {
  greet('Alice');      // Không truyền age. 
  greet('Bob', 30);    // Truyền cả hai tham số.
}

Hello, Alice!
Hello, Bob! You are 30 years old.
```
- Với giá trị mặc định
```Dart
void greet(String name, [int age = 18]) {
  print('Hello, $name! You are $age years old.');
}

void main() {
  greet('Alice');      // age mặc định là 18.
  greet('Bob', 30);    // Truyền giá trị cho age.
}
```
#### Optional Named Parameters
- Tham số tùy chọn được truyền theo tên, giúp tăng tính rõ ràng.
- **Cách dùng**:
  - Các tham số được khai báo trong `{}`.
- **Đặc điểm**:
  - Khi gọi hàm, cần chỉ định tên tham số.
  - Có thể không truyền giá trị (giá trị mặc định là null hoặc được định nghĩa trước).
```Dart
void greet({String? name, int? age}) {
  print('Hello, ${name ?? 'Guest'}! You are ${age ?? 'unknown'} years old.');
}

void main() {
  greet();                // Không truyền tham số nào.
  greet(name: 'Alice');   // Chỉ truyền name.
  greet(age: 25);         // Chỉ truyền age.
  greet(name: 'Bob', age: 30); // Truyền cả hai.
}

Hello, Guest! You are unknown years old.
Hello, Alice! You are unknown years old.
Hello, Guest! You are 25 years old.
Hello, Bob! You are 30 years old.
```
- Với giá trị mặc định
```Dart
void greet({String name = 'Guest', int age = 18}) {
  print('Hello, $name! You are $age years old.');
}

void main() {
  greet();                // Sử dụng giá trị mặc định.
  greet(name: 'Alice');   // Ghi đè giá trị mặc định cho name.
  greet(age: 25);         // Ghi đè giá trị mặc định cho age.
}
Hello, Guest! You are 18 years old.
Hello, Alice! You are 18 years old.
Hello, Guest! You are 25 years old.
```
#### Required Named Parameters
- Tham số bắt buộc truyền theo tên, đảm bảo được truyền khi gọi hàm.
- **Cách dùng**:
  - Thêm từ khóa required vào tham số trong `{}`.
- **Đặc điểm**:
  - Bắt buộc phải truyền khi gọi hàm.
  - Truyền bằng cách chỉ định tên tham số.

```Dart
void greet({required String name, required int age}) {
  print('Hello, $name! You are $age years old.');
}

void main() {
  // greet();                // Lỗi: thiếu tham số bắt buộc.
  greet(name: 'Alice', age: 25); // OK
}
Hello, Alice! You are 25 years old.
```
#### Combination of Parameters
- Kết hợp nhiều loại param:  Nếu kết hợp cả `Positional` và `Named`, tham số `Positional` phải xuất hiện trước.
```Dart
void describe(String title, {int? year, String author = 'Anonymous'}) {
  print('$title was written by $author in ${year ?? 'an unknown year'}.');
}

void main() {
  describe('Flutter for Beginners');
  describe('Dart Essentials', year: 2021);
  describe('Learning Dart', year: 2023, author: 'John Doe');
}
```
|Loại Tham Số|	Cách Khai Báo|	Truyền Tham Số Khi Gọi|
|---|---|---|
|Required Positional|	void func(String a)|	func('value')|
|Optional Positional	|void func(String a, [int? b])|	func('value') hoặc func('value', 10)|
|Optional Named|	void func({int? b})	func(b: 10) |hoặc func()|
|Required Named|	void func({required int b})|	func(b: 10)|
## Asynchronous
- Trong Dart, bất đồng bộ (`asynchronous programming`) được sử dụng để xử lý các tác vụ không đồng bộ, `như gọi API, đọc/ghi file`, hoặc `xử lý luồng dữ liệu liên tục` mà không chặn luồng chính.
- **Dart hỗ trợ lập trình bất đồng bộ thông qua:**
  - `Future`: Đại diện cho một giá trị hoặc lỗi sẽ sẵn sàng trong tương lai.
  - `Stream`: Đại diện cho một chuỗi các giá trị bất đồng bộ.
  - Từ khóa `async` và `await`: Giúp mã bất đồng bộ trông giống đồng bộ hơn.
  - Phương thức `then()`: Xử lý kết quả khi một Future hoàn thành.
- **Kết quả của các tác vụ bất đồng bộ**:
  - Những tính toán bất đồng bộ này thường trả về kết quả dưới dạng một `Future` nếu chỉ có một kết quả.
  - Nếu có nhiều kết quả (ví dụ như một chuỗi các sự kiện), chúng sẽ được trả về dưới dạng `Stream`.
### Future
- Một `future` (chữ thường) là một instance của lớp `Future` (chữ in hoa) trong Dart.
- Một `future` đại diện cho kết quả của một hoạt động bất đồng bộ và có thể tồn tại trong hai trạng thái:
  - Chưa hoàn thành `(Uncompleted)`: Khi hoạt động chưa kết thúc.
  - Đã hoàn thành `(Completed)`: Khi hoạt động hoàn thành với:
    - Một giá trị (nếu thành công).
    - Một lỗi (nếu thất bại).
```Dart
Future<String> fetchData() {
  return Future.delayed(Duration(seconds: 2), () {
    return 'Fetched Data';
  });
}

void main() {
  print('Fetching...');
  fetchData().then((data) {
    print(data); // In ra "Fetched Data" sau 2 giây
  });
}
```
#### Future Chaining
- Khi có 1 chuỗi các task liên tiếp, có thể dùng Future Chaining
```Dart
Future<int> calculate() {
  return Future.delayed(Duration(seconds: 1), () => 42);
}

void main() {
  calculate()
      .then((result) {
        print('First result: $result');
        return result * 2;
      })
      .then((finalResult) {
        print('Final result: $finalResult');
      });
}
```
### Async và Await
- `async` và `await` giúp viết mã bất đồng bộ theo cách tuần tự, dễ đọc và giống như lập trình đồng bộ.
- **Cách sử dụng**
  - Thêm từ khóa `async` vào hàm để khai báo rằng nó là hàm bất đồng bộ. Hàm sẽ trả về một `Future`.
  - Dùng `await` trước một biểu thức trả về Future để "đợi" giá trị của nó.
```Dart
Future<String> fetchData() async {
  await Future.delayed(Duration(seconds: 2)); // Chờ 2 giây
  return 'Fetched Data';
}

void main() async {
  print('Start fetching...');
  String data = await fetchData(); // Chờ kết quả từ fetchData
  print(data); // In "Fetched Data"
}
```
- **`Async/Await` với xử lý lỗi:**
```Dart
Future<String> fetchData() async {
  throw Exception('Something went wrong!');
}

void main() async {
  try {
    String data = await fetchData();
    print(data);
  } catch (e) {
    print('Error: $e');
  }
}
```
### Then
- `then` được dùng để xử lý một `Future` sau khi nó hoàn thành. Thay vì dùng await, có thể nối các bước xử lý trong chuỗi.
```Dart
Future<String> fetchData() {
  return Future.delayed(Duration(seconds: 2), () => 'Fetched Data');
}

void main() {
  fetchData().then((data) {
    print(data); // In "Fetched Data"
  }).catchError((error) {
    print('Error: $error'); // Xử lý lỗi nếu có
  });
}
```
- **Ưu và nhược điểm của then:**
  - **Ưu điểm:** Rõ ràng với các tác vụ xử lý chuỗi, giúp dễ quản lý các bước tiếp theo.
  - **Nhược điểm:** Khi có nhiều tầng then, mã nguồn trở nên phức tạp (callback hell).
### Strem
- `Stream` đại diện cho luồng dữ liệu hoặc sự kiện bất đồng bộ. Nó khác với `Future` ở chỗ `Stream` có thể phát ra nhiều giá trị theo thời gian.

- **Cách sử dụng**
  - **Single-subscription Stream**: Chỉ một listener có thể lắng nghe.
  - **Broadcast Stream**: Nhiều listener có thể đăng ký cùng lúc.
```Dart
Stream<int> countStream() async* {
  for (int i = 1; i <= 5; i++) {
    await Future.delayed(Duration(seconds: 1)); // Chờ 1 giây
    yield i; // Phát ra giá trị
  }
}

void main() {
  countStream().listen((data) {
    print('Received: $data'); // In từng giá trị sau mỗi giây
  });
}
```
|Tính năng	|async|	async*|
|---|---|---|
|Trả về|	Future (một giá trị hoặc lỗi duy nhất).|	Stream (nhiều giá trị hoặc lỗi liên tục).|
|Từ khóa phát giá trị|	return|	yield hoặc yield*.|
|Ứng dụng|	Các tác vụ bất đồng bộ với kết quả đơn lẻ.	|Dữ liệu hoặc sự kiện liên tục.|

#### Stream Subscription
- có thể đăng ký để nghe dữ liệu từ Stream bằng phương thức `listen`:
```Dart
Stream<int> countStream() async* {
  for (int i = 1; i <= 5; i++) {
    await Future.delayed(Duration(seconds: 1)); // Chờ 1 giây
    yield i; // Phát ra giá trị
  }
}

void main() {
  Stream<int> stream = countStream();

  stream.listen(
        (data) => print('Data: $data'), // Xử lý dữ liệu
    onError: (err) => print('Error: $err'), // Xử lý lỗi
    onDone: () => print('Stream closed'), // Khi Stream kết thúc
    cancelOnError: true, // Dừng lắng nghe nếu có lỗi
  );
}
```
#### Các toán tử trên Stream
|Toán tử	|Mô tả|
|---|---|
|map	|Chuyển đổi từng phần tử trong Stream.|
|where	|Lọc phần tử dựa trên điều kiện.|
|take	|Lấy số lượng phần tử giới hạn từ Stream.|
|skip	|Bỏ qua một số phần tử đầu tiên.|
|reduce	|Gộp tất cả các phần tử lại.|
```Dart
void main() {
  Stream<int> stream = countStream();

  stream
      .where((number) => number % 2 == 0) // Lọc số chẵn
      .map((number) => 'Even: $number') // Thêm mô tả
      .listen((data) {
        print(data); // In "Even: 2", "Even: 4"
      });
}
```
|Đặc điểm	|Future|	Stream|
|---|---|---|
|Số giá trị	|Một giá trị hoặc lỗi duy nhất.|	Nhiều giá trị hoặc sự kiện liên tục.|
|Cách sử dụng	|then, await	|listen, các toán tử Stream.|
|Ứng dụng phổ biến	|Gọi API, đọc file.|	Lắng nghe sự kiện, dữ liệu liên tục.|
### Xử lý lỗi 
- Với **Future:** Dùng `catchError` hoặc dùng `try-catch với async/await:`
```Dart
fetchData().then((data) {
  print(data);
}).catchError((error) {
  print('Error: $error');
});

//----------
try {
  String data = await fetchData();
  print(data);
} catch (error) {
  print('Error: $error');
}
```
- Với `Stream`: xử lý lỗi trong `listen`:
```Dart
stream.listen(
  (data) => print(data),
  onError: (error) => print('Error: $error'),
);
```
## Class
- Trong Dart, `class` là một thành phần cơ bản dùng để định nghĩa các đối tượng (object) trong lập trình hướng đối tượng. Class bao gồm các thuộc tính `(properties)` và phương thức `(methods)` để mô tả hành vi và trạng thái của đối tượng.
- **Khai báo 1 class**
```Dart
class Person {
  String name = ''; // Thuộc tính
  int age = 0;      // Thuộc tính

  void sayHello() { // Phương thức
    print('Hello, my name is $name and I am $age years old.');
  }
}
```
- **Tạo 1 object từ class**: Sử dụng từ khóa `new` (hoặc bỏ qua, vì Dart cho phép) để tạo đối tượng từ class.
```Dart
void main() {
  Person person = Person(); // Tạo đối tượng
  person.name = 'John'; // Gán giá trị thuộc tính
  person.age = 30;

  person.sayHello(); // Gọi phương thức
}
```
### Contructor
- **Khai báo contructor mặc định**
```
class Person {
  String name;
  int age;

  // Constructor
  Person(this.name, this.age);
}

void main() {
  Person person = Person('Alice', 25);
  print('Name: ${person.name}, Age: ${person.age}');
}
```
- **Constructor Named (Hàm khởi tạo có tên)**
```Dart
class Person {
  String name;
  int age;

  Person(this.name, this.age);

  // Constructor Named
  Person.unnamed() {
    name = 'Unnamed';
    age = 0;
  }
}

void main() {
  Person person1 = Person('Bob', 22);
  Person person2 = Person.unnamed();

  print(person1.name); // Bob
  print(person2.name); // Unnamed
}
```
- **Initializer List (Danh sách khởi tạo)**: Dùng để khởi tạo các thuộc tính trước khi body của constructor chạy.

```Dart
class Person {
  final String name;
  final int age;

  // Initializer List
  Person(String name, int age)
      : name = name,
        age = age;

  void showInfo() {
    print('Name: $name, Age: $age');
  }
}

void main() {
  Person person = Person('Charlie', 28);
  person.showInfo();
}
```
### Getter và Setter
- Dùng để kiểm soát quyền truy cập và thay đổi các thuộc tính.
- **Getter**:
```Dart
class Circle {
  double radius;

  Circle(this.radius);

  double get area => 3.14 * radius * radius; // Getter
}

void main() {
  Circle circle = Circle(10);
  print('Area: ${circle.area}'); // Area: 314.0
}
```
- **Setter**
```Dart
class Circle {
  double _radius = 0;

  double get radius => _radius;

  set radius(double value) {
    if (value > 0) {
      _radius = value;
    } else {
      print('Radius must be positive');
    }
  }
}

void main() {
  Circle circle = Circle();
  circle.radius = 5; // Gán giá trị
  print(circle.radius); // 5
  circle.radius = -3; // Lỗi: Radius must be positive
}
```
### Static Members
- Các thành phần static thuộc về `class` thay vì `object.`
```Dart
class Math {
  static const double pi = 3.14;

  static double calculateArea(double radius) {
    return pi * radius * radius;
  }
}

void main() {
  print('Pi: ${Math.pi}');
  print('Area: ${Math.calculateArea(10)}');
}
```
## OOP
### Tính đóng gói
- Đóng gói là khả năng che giấu thông tin chi tiết của đối tượng và chỉ cho phép truy cập thông qua các phương thức hoặc `getter/setter.`
- Trong Dart, các thành phần private được định nghĩa bằng cách sử dụng dấu gạch dưới `(_)` trước tên của thuộc tính hoặc phương thức.
```Dart
class BankAccount {
  String _accountNumber; // Thuộc tính private
  double _balance;

  BankAccount(this._accountNumber, this._balance);

  // Getter để truy cập số dư
  double get balance => _balance;

  // Phương thức public để thêm tiền
  void deposit(double amount) {
    if (amount > 0) {
      _balance += amount;
    }
  }

  // Phương thức public để rút tiền
  void withdraw(double amount) {
    if (amount > 0 && amount <= _balance) {
      _balance -= amount;
    }
  }
}

void main() {
  BankAccount account = BankAccount('12345678', 1000);
  account.deposit(500); // Nạp tiền
  account.withdraw(200); // Rút tiền
  print('Balance: ${account.balance}'); // In số dư
}
```

- Dấu `_` làm cho thuộc tính hoặc phương thức chỉ có thể truy cập được trong cùng file.
- `Getter/Setter` hoặc các phương thức public được dùng để kiểm soát truy cập.
### Kế thừa
- Kế thừa thông qua `extends`
```Dart
class Animal {
  String name;

  Animal(this.name);

  void eat() {
    print('$name is eating');
  }
}

class Dog extends Animal {
  Dog(String name) : super(name);

  void bark() {
    print('$name is barking');
  }
}

void main() {
  Dog dog = Dog('Buddy');
  dog.eat(); // Buddy is eating
  dog.bark(); // Buddy is barking
}
```

### Đa hình
```Dart
class Animal {
  void makeSound() {
    print('Animal makes a sound');
  }
}

class Dog extends Animal {
  @override
  void makeSound() {
    print('Dog barks');
  }
}

class Cat extends Animal {
  @override
  void makeSound() {
    print('Cat meows');
  }
}

void main() {
  Animal animal = Dog(); // Lớp Dog được sử dụng qua lớp Animal
  animal.makeSound(); // Dog barks

  animal = Cat(); // Lớp Cat được sử dụng qua lớp Animal
  animal.makeSound(); // Cat meows
}
```
- **Đặc điểm:**
  - **Override:** Lớp con có thể cung cấp triển khai khác cho các phương thức của lớp cha.
  - **Dynamic Binding:** Loại của đối tượng chỉ được xác định tại runtime.

### Trừu tượng
- Dart không có từ khóa `interface`, nhưng tất cả các lớp đều có thể hoạt động như một `interface`.
#### Abstract Class
- Dùng từ khóa `abstract` để định nghĩa lớp trừu tượng. Lớp này không thể khởi tạo và chỉ dùng để kế thừa.
```Dart
abstract class Animal {
  void makeSound(); // Phương thức trừu tượng
}

class Dog extends Animal {
  @override
  void makeSound() {
    print('Dog barks');
  }
}

void main() {
  Dog dog = Dog();
  dog.makeSound(); // Dog barks
}
```
#### Interface
- Dart không có từ khóa riêng cho `interface`, nhưng có thể thực hiện `interface` bằng cách `implement` một lớp.
```Dart
class Flyable {
  void fly() {
    print('Flying...');
  }
}

class Bird implements Flyable {
  @override
  void fly() {
    print('Bird is flying');
  }
}

void main() {
  Bird bird = Bird();
  bird.fly(); // Bird is flying
}
```
|Tính Chất|	Ý Nghĩa|	Ví Dụ|
|---|---|---|
|Đóng gói|	Che giấu thông tin, chỉ cho phép truy cập thông qua getter/setter.|	Dấu _ trước thuộc tính và sử dụng getter/setter.
|Kế thừa|	Lớp con thừa hưởng thuộc tính/phương thức từ lớp cha.|	Lớp Dog kế thừa lớp Animal.
|Đa hình|	Một đối tượng có thể mang nhiều hình dạng, thực hiện ghi đè phương thức.	|Gọi phương thức makeSound trên các đối tượng Dog, Cat.
|Trừu tượng|	Định nghĩa hành vi chung mà không cần cung cấp chi tiết.	|Sử dụng abstract class hoặc implements.|

## With && Mixin
- `with` và `mixin` là các tính năng mạnh mẽ trong Dart, được sử dụng để chia sẻ logic giữa các lớp mà không cần kế thừa trực tiếp từ lớp khác. 
- Điều này giúp tái sử dụng code dễ dàng hơn mà không bị ràng buộc bởi quan hệ kế thừa.
- Quy tắc khi sử dụng Mixin:
  - Một lớp `mixin` không thể được khởi tạo trực tiếp (không thể tạo object từ mixin).
  - `Mixin` không thể chứa constructor.
  - Nếu `mixin` yêu cầu các thuộc tính hoặc phương thức cụ thể từ lớp sử dụng, có thể thêm từ khóa `on` để giới hạn lớp nào có thể sử dụng mixin.

- Lợi ích:
  - Tái sử dụng mã: Mixin giúp chia sẻ logic mà không cần quan hệ kế thừa.
  - Tăng tính linh hoạt: Một lớp có thể kết hợp nhiều mixin để thêm các chức năng khác nhau.
  - Giảm trùng lặp: Bạn có thể chia sẻ các phương thức chung cho nhiều lớp mà không cần sao chép mã.
### Mixin
- `Mixin` là một cách để thêm các thuộc tính và phương thức vào một lớp khác mà không cần thông qua kế thừa.
- Một lớp được sử dụng làm mixin không cần khai báo từ khóa đặc biệt, nhưng thông thường sẽ `không có constructor`.
- `Dart` cung cấp từ khóa `mixin` (kể từ Dart 2.1) để định nghĩa một `mixin`.
#### Khai báo một mixin
- Dùng từ khóa `mixin`
```Dart
mixin Flyable {
  void fly() {
    print("I can fly!");
  }
}

mixin Walkable {
  void walk() {
    print("I can walk!");
  }
}
```
#### Sử dụng mixin với with
- Dùng từ khóa `with` để sử dụng `mixin` trong class. Một lớp có thể sử dụng nhiều `mixin`
```Dart
class Bird with Flyable, Walkable {}

void main() {
  Bird bird = Bird();
  bird.fly();  // I can fly!
  bird.walk(); // I can walk!
}
```
- `with` không yêu cầu quan hệ kế thừa giữa lớp chứa mixin và lớp sử dụng `mixin`.
- Mỗi lớp đều có thể sử dụng các phương thức và thuộc tính được định nghĩa trong` mixin`.

#### Mixin kết hợp kế thừa
- Khi một lớp kế thừa một lớp cha, nó vẫn có thể sử dụng `mixin`.
```Dart
class Animal {
  void breathe() {
    print("I can breathe!");
  }
}

class Bird extends Animal with Flyable {}

void main() {
  Bird bird = Bird();
  bird.breathe(); // I can breathe!
  bird.fly();     // I can fly!
}
```

#### Ràng buộc mixin với `on`
- Dùng `on` để chỉ định rằng mixin chỉ có thể được sử dụng với một số lớp cụ thể (hoặc lớp con của chúng).
```Dart
class Animal {
  void breathe() {
    print("I can breathe!");
  }
}

mixin Swimmable on Animal {
  void swim() {
    print("I can swim!");
  }
}

class Fish extends Animal with Swimmable {}

void main() {
  Fish fish = Fish();
  fish.breathe(); // I can breathe!
  fish.swim();    // I can swim!
}
```
- `mixin Swimmable` chỉ có thể được sử dụng với lớp `Anima`l hoặc các lớp con của nó. 
- Nếu cố sử dụng Swimmable với một lớp không thỏa mãn điều kiện, Dart sẽ báo lỗi.

#### Thứ tự ưu tiên trong mixin
- Nếu có nhiều `mixin` với các phương thức trùng tên, thứ tự `mixin` trong từ khóa with sẽ quyết định phương thức nào được ưu tiên.
```Dart
mixin A {
  void display() {
    print("Mixin A");
  }
}

mixin B {
  void display() {
    print("Mixin B");
  }
}

class MyClass with A, B {}

void main() {
  MyClass myClass = MyClass();
  myClass.display(); // Mixin B (ưu tiên mixin đứng sau)
}
```
|Mixin	|Kế thừa|
|---|---|
|Một lớp có thể sử dụng nhiều mixin.|	Một lớp chỉ có thể kế thừa từ một lớp cha.|
|Không mang ý nghĩa "là một".	|Quan hệ kế thừa thể hiện "là một".|
|Không thể chứa constructor.|	Có thể chứa constructor.|
|Linh hoạt và tái sử dụng logic dễ dàng hơn.|	Hạn chế bởi cấu trúc kế thừa cố định.|
