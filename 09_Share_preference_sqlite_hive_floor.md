# Phương Pháp Lưu Trữ Dữ Liệu Trong Flutter

## 1. SharedPreferences

SharedPreferences là một cách đơn giản để lưu trữ dữ liệu dưới dạng cặp khóa-giá trị. Nó thích hợp cho các dữ liệu nhỏ như cài đặt người dùng, thông tin đăng nhập, hoặc trạng thái ứng dụng.

### Cài đặt

Thêm dependency vào file `pubspec.yaml`:

```yaml
dependencies:
  shared_preferences: ^2.2.2
```

### Cách sử dụng

```dart
import 'package:shared_preferences/shared_preferences.dart';

// Lưu dữ liệu
Future<void> saveData() async {
  final prefs = await SharedPreferences.getInstance();
  
  // Lưu các loại dữ liệu khác nhau
  await prefs.setString('username', 'nguyenvanA');
  await prefs.setInt('age', 25);
  await prefs.setBool('isLoggedIn', true);
  await prefs.setStringList('favorites', ['Flutter', 'Dart', 'Mobile']);
}

// Đọc dữ liệu
Future<void> readData() async {
  final prefs = await SharedPreferences.getInstance();
  
  final username = prefs.getString('username') ?? 'Guest';
  final age = prefs.getInt('age') ?? 0;
  final isLoggedIn = prefs.getBool('isLoggedIn') ?? false;
  final favorites = prefs.getStringList('favorites') ?? [];
  
  print('Username: $username');
  print('Age: $age');
  print('Logged in: $isLoggedIn');
  print('Favorites: $favorites');
}

// Xóa dữ liệu
Future<void> removeData() async {
  final prefs = await SharedPreferences.getInstance();
  
  await prefs.remove('username'); // Xóa một mục cụ thể
  await prefs.clear(); // Xóa tất cả dữ liệu
}
```

### Ưu điểm
- Đơn giản, dễ sử dụng
- Tích hợp sẵn trong Flutter SDK
- Phù hợp cho lưu trữ cài đặt đơn giản

### Nhược điểm
- Chỉ hỗ trợ các kiểu dữ liệu cơ bản (String, int, double, bool, List<String>)
- Không phù hợp cho dữ liệu phức tạp hoặc dữ liệu lớn
- Không hỗ trợ truy vấn

## 2. SQLite (sqflite)

SQLite là một cơ sở dữ liệu quan hệ nhẹ được nhúng trực tiếp vào ứng dụng. Nó thích hợp cho việc lưu trữ dữ liệu có cấu trúc và phức tạp.

### Cài đặt

Thêm dependency vào file `pubspec.yaml`:

```yaml
dependencies:
  sqflite: ^2.3.0
  path: ^1.8.3
```

### Cách sử dụng

#### Định nghĩa mô hình

```dart
class User {
  final int? id;
  final String name;
  final int age;
  
  User({this.id, required this.name, required this.age});
  
  Map<String, dynamic> toMap() {
    return {
      'id': id,
      'name': name,
      'age': age,
    };
  }
  
  factory User.fromMap(Map<String, dynamic> map) {
    return User(
      id: map['id'],
      name: map['name'],
      age: map['age'],
    );
  }
}
```

#### Tạo và quản lý cơ sở dữ liệu

```dart
import 'package:sqflite/sqflite.dart';
import 'package:path/path.dart';

class DatabaseHelper {
  static Database? _database;
  
  Future<Database> get database async {
    if (_database != null) return _database!;
    _database = await _initDB();
    return _database!;
  }
  
  Future<Database> _initDB() async {
    final dbPath = await getDatabasesPath();
    final path = join(dbPath, 'users.db');
    
    return await openDatabase(
      path,
      version: 1,
      onCreate: (db, version) async {
        await db.execute('''
          CREATE TABLE users(
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT,
            age INTEGER
          )
        ''');
      },
    );
  }
  
  // Thêm user mới
  Future<int> insertUser(User user) async {
    final db = await database;
    return await db.insert('users', user.toMap());
  }
  
  // Lấy tất cả users
  Future<List<User>> getUsers() async {
    final db = await database;
    final maps = await db.query('users');
    
    return List.generate(maps.length, (i) {
      return User.fromMap(maps[i]);
    });
  }
  
  // Lấy user theo id
  Future<User?> getUser(int id) async {
    final db = await database;
    final maps = await db.query(
      'users',
      where: 'id = ?',
      whereArgs: [id],
    );
    
    if (maps.isNotEmpty) {
      return User.fromMap(maps.first);
    }
    return null;
  }
  
  // Cập nhật user
  Future<int> updateUser(User user) async {
    final db = await database;
    return await db.update(
      'users',
      user.toMap(),
      where: 'id = ?',
      whereArgs: [user.id],
    );
  }
  
  // Xóa user
  Future<int> deleteUser(int id) async {
    final db = await database;
    return await db.delete(
      'users',
      where: 'id = ?',
      whereArgs: [id],
    );
  }
}
```

#### Sử dụng DatabaseHelper

```dart
final dbHelper = DatabaseHelper();

// Thêm user
await dbHelper.insertUser(User(name: 'Nguyen Van A', age: 25));
await dbHelper.insertUser(User(name: 'Tran Thi B', age: 30));

// Lấy danh sách users
final users = await dbHelper.getUsers();
for (var user in users) {
  print('ID: ${user.id}, Name: ${user.name}, Age: ${user.age}');
}

// Cập nhật user
await dbHelper.updateUser(User(id: 1, name: 'Nguyen Van A (Updated)', age: 26));

// Xóa user
await dbHelper.deleteUser(2);
```

### Ưu điểm
- Hỗ trợ đầy đủ SQL với các truy vấn phức tạp
- Hiệu suất tốt cho dữ liệu có cấu trúc
- Phù hợp cho các ứng dụng cần quản lý dữ liệu phức tạp
- Hỗ trợ giao dịch (transactions)

### Nhược điểm
- Cú pháp phức tạp hơn các phương pháp khác
- Cần viết nhiều code để thiết lập và quản lý
- Không hỗ trợ cơ chế reactive (cần thêm thư viện khác)

## 3. Hive

Hive là cơ sở dữ liệu NoSQL nhẹ, nhanh, được viết bằng Dart thuần. Nó được tối ưu hóa cho Flutter và có hiệu suất cao.

### Cài đặt

Thêm dependencies vào file `pubspec.yaml`:

```yaml
dependencies:
  hive: ^2.2.3
  hive_flutter: ^1.1.0

dev_dependencies:
  hive_generator: ^2.0.1
  build_runner: ^2.4.6
```

### Cách sử dụng

#### Khởi tạo Hive

```dart
import 'package:hive_flutter/hive_flutter.dart';

void main() async {
  await Hive.initFlutter();
  runApp(MyApp());
}
```

#### Định nghĩa model với Hive

```dart
import 'package:hive/hive.dart';

part 'person.g.dart';

@HiveType(typeId: 0)
class Person {
  @HiveField(0)
  final String name;

  @HiveField(1)
  final int age;

  @HiveField(2)
  final List<String> hobbies;

  Person({required this.name, required this.age, required this.hobbies});
}
```

Sau đó, chạy lệnh sau để tạo file adapter:

```
flutter pub run build_runner build
```

#### Đăng ký adapter và mở box

```dart
void main() async {
  await Hive.initFlutter();
  
  // Đăng ký adapter
  Hive.registerAdapter(PersonAdapter());
  
  // Mở box
  await Hive.openBox<Person>('personBox');
  
  runApp(MyApp());
}
```

#### Thao tác với dữ liệu

```dart
// Lấy box
final personBox = Hive.box<Person>('personBox');

// Thêm dữ liệu
personBox.add(Person(
  name: 'Nguyen Van A',
  age: 25,
  hobbies: ['Đọc sách', 'Chơi thể thao']
));

// Thêm với key cụ thể
personBox.put('person1', Person(
  name: 'Tran Thi B',
  age: 30,
  hobbies: ['Du lịch', 'Âm nhạc']
));

// Lấy dữ liệu theo index
final person1 = personBox.getAt(0);
print('Person 1: ${person1?.name}, ${person1?.age}');

// Lấy dữ liệu theo key
final person2 = personBox.get('person1');
print('Person 2: ${person2?.name}, ${person2?.age}');

// Cập nhật dữ liệu
personBox.putAt(0, Person(
  name: 'Nguyen Van A (Updated)',
  age: 26,
  hobbies: ['Đọc sách', 'Chơi thể thao', 'Lập trình']
));

// Xóa dữ liệu
personBox.deleteAt(0);
personBox.delete('person1');

// Lắng nghe thay đổi (reactive)
personBox.listenable().addListener(() {
  print('Box changed!');
});
```

#### Sử dụng Hive với widget Flutter

```dart
import 'package:flutter/material.dart';
import 'package:hive_flutter/hive_flutter.dart';

class PersonListScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Danh sách người dùng')),
      body: ValueListenableBuilder(
        valueListenable: Hive.box<Person>('personBox').listenable(),
        builder: (context, Box<Person> box, _) {
          if (box.isEmpty) {
            return Center(child: Text('Không có dữ liệu'));
          }
          
          return ListView.builder(
            itemCount: box.length,
            itemBuilder: (context, index) {
              final person = box.getAt(index);
              return ListTile(
                title: Text(person!.name),
                subtitle: Text('${person.age} tuổi'),
                trailing: Text(person.hobbies.join(', ')),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.add),
        onPressed: () {
          // Thêm person mới
          Hive.box<Person>('personBox').add(Person(
            name: 'Người dùng mới ${DateTime.now()}',
            age: 20,
            hobbies: ['Flutter', 'Dart']
          ));
        },
      ),
    );
  }
}
```

#### Đóng Hive khi không sử dụng

```dart
@override
void dispose() {
  Hive.close();
  super.dispose();
}
```

### Ưu điểm
- Hiệu suất rất cao (nhanh hơn so với SQLite và SharedPreferences)
- API đơn giản và trực quan
- Hỗ trợ reactive programming với ValueListenable
- Hỗ trợ mã hóa
- Không cần JSON serialization/deserialization
- Viết bằng Dart thuần

### Nhược điểm
- Không hỗ trợ truy vấn phức tạp như SQL
- Cần tạo adapter cho mỗi model
- Không phù hợp cho các mối quan hệ phức tạp

## 4. Floor (SQLite Wrapper)

Floor là một wrapper cho SQLite, cung cấp lớp trừu tượng và giảm boilerplate code. Nó sử dụng annotation processing để tạo code.

### Cài đặt

Thêm dependencies vào file `pubspec.yaml`:

```yaml
dependencies:
  floor: ^1.4.2
  sqflite: ^2.3.0

dev_dependencies:
  floor_generator: ^1.4.2
  build_runner: ^2.4.6
```

### Cách sử dụng

#### Định nghĩa Entity

```dart
import 'package:floor/floor.dart';

@entity
class Task {
  @PrimaryKey(autoGenerate: true)
  final int? id;
  
  final String title;
  final String description;
  final bool isCompleted;
  
  Task({
    this.id,
    required this.title,
    required this.description,
    this.isCompleted = false,
  });
}
```

#### Định nghĩa DAO (Data Access Object)

```dart
import 'package:floor/floor.dart';
import 'package:your_app/models/task.dart';

@dao
abstract class TaskDao {
  @Query('SELECT * FROM Task')
  Future<List<Task>> findAllTasks();
  
  @Query('SELECT * FROM Task WHERE id = :id')
  Future<Task?> findTaskById(int id);
  
  @Query('SELECT * FROM Task WHERE isCompleted = :isCompleted')
  Future<List<Task>> findTasksByStatus(bool isCompleted);
  
  @insert
  Future<void> insertTask(Task task);
  
  @insert
  Future<void> insertTasks(List<Task> tasks);
  
  @update
  Future<void> updateTask(Task task);
  
  @delete
  Future<void> deleteTask(Task task);
  
  @Query('DELETE FROM Task')
  Future<void> deleteAllTasks();
}
```

#### Định nghĩa Database

```dart
import 'dart:async';
import 'package:floor/floor.dart';
import 'package:sqflite/sqflite.dart' as sqflite;

import 'package:your_app/models/task.dart';
import 'package:your_app/dao/task_dao.dart';

part 'app_database.g.dart'; // Sẽ được tạo tự động bởi Floor

@Database(version: 1, entities: [Task])
abstract class AppDatabase extends FloorDatabase {
  TaskDao get taskDao;
  
  // Singleton pattern
  static Future<AppDatabase> getInstance() async {
    return await $FloorAppDatabase.databaseBuilder('app_database.db').build();
  }
}
```

Sau đó, chạy lệnh để tạo các file cần thiết:

```
flutter pub run build_runner build
```

#### Sử dụng Database

```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  
  final database = await AppDatabase.getInstance();
  final taskDao = database.taskDao;
  
  runApp(MyApp(taskDao: taskDao));
}

class MyApp extends StatelessWidget {
  final TaskDao taskDao;
  
  const MyApp({Key? key, required this.taskDao}) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    // ... xây dựng ứng dụng
  }
}
```

#### Thao tác với dữ liệu

```dart
// Thêm Task
await taskDao.insertTask(Task(
  title: 'Học Flutter',
  description: 'Tìm hiểu về Floor Database',
));

// Lấy tất cả Task
final tasks = await taskDao.findAllTasks();
for (var task in tasks) {
  print('${task.id}: ${task.title} - ${task.isCompleted ? 'Hoàn thành' : 'Chưa hoàn thành'}');
}

// Cập nhật Task
final taskToUpdate = tasks.first;
await taskDao.updateTask(Task(
  id: taskToUpdate.id,
  title: taskToUpdate.title,
  description: taskToUpdate.description,
  isCompleted: true,
));

// Lấy task theo trạng thái
final completedTasks = await taskDao.findTasksByStatus(true);
print('Số task đã hoàn thành: ${completedTasks.length}');

// Xóa task
await taskDao.deleteTask(tasks.last);
```

#### Sử dụng với Stream (Reactive)

Floor cũng hỗ trợ streams để làm việc với dữ liệu theo cách reactive:

```dart
@dao
abstract class ReactiveTaskDao {
  @Query('SELECT * FROM Task')
  Stream<List<Task>> watchAllTasks();
  
  @Query('SELECT * FROM Task WHERE isCompleted = :isCompleted')
  Stream<List<Task>> watchTasksByStatus(bool isCompleted);
  
  // Các phương thức khác như trên...
}
```

Sử dụng Stream trong widget:

```dart
StreamBuilder<List<Task>>(
  stream: taskDao.watchAllTasks(),
  builder: (context, snapshot) {
    if (snapshot.hasError) {
      return Text('Đã xảy ra lỗi: ${snapshot.error}');
    }
    
    if (!snapshot.hasData) {
      return CircularProgressIndicator();
    }
    
    final tasks = snapshot.data!;
    return ListView.builder(
      itemCount: tasks.length,
      itemBuilder: (context, index) {
        final task = tasks[index];
        return ListTile(
          title: Text(task.title),
          subtitle: Text(task.description),
          trailing: Checkbox(
            value: task.isCompleted,
            onChanged: (value) async {
              await taskDao.updateTask(Task(
                id: task.id,
                title: task.title,
                description: task.description,
                isCompleted: value ?? false,
              ));
            },
          ),
        );
      },
    );
  },
),
```

### Ưu điểm
- Giảm boilerplate code so với SQLite thuần
- Sử dụng annotations làm cho code sạch hơn
- Hỗ trợ reactive programming với Stream
- Tự động tạo code thông qua code generation
- Mạnh mẽ với khả năng truy vấn SQL đầy đủ

### Nhược điểm
- Cần sử dụng code generation (build_runner)
- Không đơn giản như Hive hoặc SharedPreferences
- Vẫn cần hiểu về SQL

## 5. So sánh các phương pháp lưu trữ dữ liệu

| Tính năng | SharedPreferences | SQLite | Hive | Floor |
|-----------|------------------|--------|------|-------|
| **Loại lưu trữ** | Key-Value | Cơ sở dữ liệu quan hệ | NoSQL, Key-Value | Wrapper cho SQLite |
| **Độ phức tạp** | Thấp | Cao | Trung bình | Trung bình-Cao |
| **Hiệu suất** | Tốt cho dữ liệu nhỏ | Tốt cho dữ liệu có cấu trúc | Rất tốt | Tốt |
| **Truy vấn phức tạp** | Không | Có (SQL) | Hạn chế | Có (SQL) |
| **Reactive** | Không | Không (cần thư viện bổ sung) | Có sẵn | Có sẵn |
| **Kiểu dữ liệu** | Cơ bản | Tất cả (với chuyển đổi) | Tất cả (với adapter) | Tất cả (với chuyển đổi) |
| **Code generation** | Không | Không | Có | Có |
| **Phù hợp cho** | Cài đặt đơn giản | Dữ liệu phức tạp, quan hệ | Dữ liệu phức tạp, hiệu suất cao | Dữ liệu có cấu trúc, dễ quản lý |

## 6. Khuyến nghị sử dụng

1. **SharedPreferences**:
   - Lưu trữ cài đặt người dùng đơn giản
   - Lưu trạng thái đăng nhập
   - Lưu cờ đánh dấu (flags)
   - Lưu trữ nhỏ, không cấu trúc

2. **SQLite (với sqflite)**:
   - Ứng dụng cần lưu trữ dữ liệu có cấu trúc phức tạp
   - Cần truy vấn phức tạp với SQL
   - Dữ liệu có quan hệ
   - Cần giao dịch (transactions)

3. **Hive**:
   - Cần hiệu suất cao
   - Dữ liệu không quá phức tạp về quan hệ
   - Muốn API đơn giản, trực quan
   - Cần reactive programming
   - Ưu tiên tốc độ

4. **Floor**:
   - Muốn sử dụng SQLite nhưng với ít boilerplate code hơn
   - Muốn kết hợp sức mạnh của SQLite với API hiện đại
   - Cần reactive programming với Stream
   - Thích sử dụng annotations và code generation

## 7. Các thư viện khác đáng chú ý

1. **Moor**: Thư viện ORM (Object-Relational Mapping) cho SQLite với nhiều tính năng mạnh mẽ (có thể thay thế cho Floor).

2. **Drift**: Phiên bản tiếp theo của Moor, với hỗ trợ null safety và nhiều tính năng mới.

3. **ObjectBox**: Cơ sở dữ liệu NoSQL với hiệu suất rất cao, là đối thủ cạnh tranh của Hive.

4. **Isar**: Cơ sở dữ liệu NoSQL hiệu suất cao, hỗ trợ đa nền tảng, là lựa chọn thay thế mới cho Hive.

5. **Sembast**: Cơ sở dữ liệu NoSQL đơn giản cho Dart/Flutter với khả năng truy vấn tốt.

## 8. Kết luận

Flutter cung cấp nhiều phương pháp lưu trữ dữ liệu, mỗi phương pháp có những ưu và nhược điểm riêng. Việc lựa chọn phương pháp phù hợp phụ thuộc vào yêu cầu cụ thể của ứng dụng:

- **SharedPreferences** cho dữ liệu đơn giản, nhỏ
- **SQLite (sqflite)** cho dữ liệu có cấu trúc, quan hệ phức tạp
- **Hive** cho hiệu suất cao và API đơn giản
- **Floor** cho việc sử dụng SQLite với ít code hơn

Trong nhiều trường hợp, bạn có thể kết hợp các phương pháp lưu trữ khác nhau trong cùng một ứng dụng: sử dụng SharedPreferences cho cài đặt người dùng, SQLite hoặc Hive cho dữ liệu chính của ứng dụng.
 