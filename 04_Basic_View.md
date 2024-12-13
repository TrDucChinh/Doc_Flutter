# SingleScrollView, ListView, GridView, Expanded, Flexible, Navigate, dialog, bottom sheet, Tìm hiểu về pub dev
## SingleChildScrollView
- **SingleChildScrollView** là một widget cho phép nội dung cuộn trong một chiều (dọc hoặc ngang).
- Được thiết kế để xử lý các tình huống mà  có một **widget đơn lẻ** vượt quá kích thước màn hình.

### Đặc điểm chính
- **Chỉ có một widget con (single child):**
  - cần đặt một widget làm con trực tiếp của `SingleChildScrollView`. 
  - Nếu cần thêm nhiều widget,  có thể bọc chúng trong một widget như `Column, Row, hoặc Stack`.
- **Hỗ trợ cuộn theo một chiều:**
  - Mặc định, `SingleChildScrollView` cuộn theo chiều dọc.
  - Nếu muốn cuộn ngang, sử dụng thuộc tính `scrollDirection: Axis.horizontal`.
- **Không tối ưu với danh sách lớn:**
  - Không nên sử dụng cho danh sách dài, vì tất cả các phần tử sẽ được dựng ngay lập tức, gây tiêu tốn bộ nhớ. 
  - Thay vào đó, sử dụng `ListView` hoặc `CustomScrollView`.

- **Các thuộc tính chính**

| Thuộc tính        | Mô tả                                                       |
| ----------------- | ----------------------------------------------------------- |
| `scrollDirection` | Xác định hướng cuộn (`Axis.vertical hoặc Axis.horizontal`). |
| `reverse`         | Đảo ngược hướng cuộn.                                       |
| `padding`         | Khoảng cách giữa nội dung và biên của vùng cuộn.            |
| `physics`         | Kiểm soát hành vi cuộn (ví dụ: `BouncingScrollPhysics`).    |
| `controller`      | Điều khiển cuộn thông qua `ScrollController`.               |
| `clipBehavior`    | Kiểm soát việc cắt bớt nội dung vượt ngoài biên.            |

- **Lưu ý khi sử dụng:**
  - **Giới hạn kích thước của widget con:** Nếu widget con không có kích thước cố định hoặc chiếm quá nhiều không gian, `SingleChildScrollView` có thể gây lỗi `Viewport was given unbounded height`.
    - Dùng `Expanded, Flexible`, hoặc giới hạn chiều cao cụ thể cho các widget.
  - Không thay thế được `ListView:`
    - Dùng `SingleChildScrollView` khi nội dung ít hoặc không cần quản lý bộ nhớ động.
    - Dùng `ListView` hoặc `CustomScrollView` khi có danh sách dài hoặc nội dung động.

## ListView
- Là một widget được sử dụng để tạo danh sách cuộn trong Flutter. 
- Nó rất mạnh mẽ và linh hoạt, đặc biệt phù hợp để hiển thị các danh sách dài hoặc dữ liệu động
- ListView tự động tối ưu hóa bộ nhớ bằng cách chỉ hiển thị các phần tử trong vùng nhìn thấy được `(viewport)`, giúp tiết kiệm tài nguyên hơn so với `SingleChildScrollView`.

### Khi nào nên sử dụng
- Hiển thị danh sách các phần tử có độ dài không cố định.
- Dữ liệu cần tải động (ví dụ: từ API hoặc cơ sở dữ liệu).
- Danh sách lớn mà hiệu năng là yếu tố quan trọng.

### Các chế độ
#### ListView thông thường
- Dùng để tạo danh sách từ một số lượng phần tử nhỏ, định nghĩa rõ ràng.

```dart
ListView(
  children: [
    ListTile(
      leading: Icon(Icons.map),
      title: Text('Map'),
    ),
    ListTile(
      leading: Icon(Icons.photo),
      title: Text('Photos'),
    ),
    ListTile(
      leading: Icon(Icons.phone),
      title: Text('Phone'),
    ),
  ],
)
```
#### ListView.builder
- Sử dụng khi có một danh sách dài hoặc cần tạo phần tử một cách động.

```dart
ListView.builder(
  itemCount: 100, // Số lượng phần tử
  itemBuilder: (context, index) {
    return ListTile(
      leading: Icon(Icons.star),
      title: Text('Item $index'),
    );
  },
)
```
- Chỉ dựng các phần tử được hiển thị, tiết kiệm bộ nhớ.
- Rất phù hợp với danh sách lớn.

#### ListView.separated
- Dùng để tạo danh sách có các phần tử ngăn cách bởi một widget (ví dụ: đường kẻ).
- 
```dart
ListView.separated(
  itemCount: 10,
  separatorBuilder: (context, index) => Divider(
    color: Colors.grey,
    thickness: 1,
  ),
  itemBuilder: (context, index) {
    return ListTile(
      leading: Icon(Icons.person),
      title: Text('Person $index'),
    );
  },
)
```

#### ListView.custom
- Dành cho trường hợp cần tùy chỉnh cao, có thể cung cấp `SliverChildDelegate` để quản lý danh sách.

```Dart
ListView.custom(
  childrenDelegate: SliverChildBuilderDelegate(
    (context, index) => ListTile(
      title: Text('Custom Item $index'),
    ),
    childCount: 50, // Số lượng phần tử
  ),
)
```

- Thuộc tính chính của ListView

| Thuộc tính        | Mô tả                                                                                           |
| ----------------- | ----------------------------------------------------------------------------------------------- |
| `scrollDirection` | Xác định hướng cuộn (`Axis.vertical` hoặc `Axis.horizontal`).                                   |
| `reverse`         | Đảo ngược hướng cuộn (nếu `true`).                                                              |
| `padding`         | Thêm khoảng cách giữa nội dung và biên của `ListView`.                                          |
| `itemExtent`      | Chiều cao/cao của mỗi phần tử (dùng để tối ưu hóa nếu tất cả phần tử có kích thước giống nhau). |
| `physics`         | Điều chỉnh hành vi cuộn (ví dụ: `BouncingScrollPhysics`, `NeverScrollableScrollPhysics`).       |
| `shrinkWrap`      | Nếu `true`, kích thước của danh sách sẽ được điều chỉnh để vừa nội dung.                        |
| `primary`         | Nếu `true`, `ListView` sẽ sử dụng controller chính mặc định cho cuộn.                           |
| `controller`      | Cung cấp một `ScrollController` để điều khiển cuộn.                                             |

#### So sánh ListView và SingleChildScrollView

| Đặc điểm     | ListView                     | SingleChildScrollView                 |
| ------------ | ---------------------------- | ------------------------------------- |
| Hiệu năng    | Tốt hơn với danh sách dài    | Không tối ưu cho danh sách dài        |
| Sử dụng động | Tương thích với dữ liệu động | Không tối ưu khi cần tải dữ liệu động |
| Tự động cuộn | Tích hợp sẵn                 | Phải tùy chỉnh                        |

## GridView
- Là một widget mạnh mẽ trong Flutter, cho phép  tạo các lưới (grid) cuộn với các phần tử có thể thay đổi kích thước hoặc số lượng tùy thuộc vào dữ liệu
- Đây là một trong những widget rất hữu ích khi cần hiển thị nhiều phần tử theo dạng lưới, ví dụ như trong các ứng dụng như thư viện ảnh, danh sách sản phẩm, hay các mục menu.

### Lợi ích của GridView
- **Hiệu suất cao**: Tối ưu hóa bộ nhớ và tốc độ khi  làm việc với danh sách các phần tử lớn.
- **Dễ dàng cấu hình:** Có thể tạo các lưới tùy chỉnh bằng cách sử dụng các delegate như `SliverGridDelegateWithFixedCrossAxisCount` hoặc `SliverGridDelegateWithMaxCrossAxisExtent`.
- **Tùy chỉnh linh hoạt:**  có thể dễ dàng thay đổi số lượng cột, khoảng cách giữa các phần tử, và cách thức các phần tử được hiển thị.
### Khi nào  dùng
- **Hiển thị các mục theo dạng lưới:** Ví dụ như ảnh, sản phẩm, hoặc các mục menu.
- **Dữ liệu động hoặc tải từ API:** Khi cần tạo các phần tử từ dữ liệu tải động.
- **Khi cần tối ưu hóa bộ nhớ:** `GridView.builder` giúp tạo các phần tử chỉ khi chúng xuất hiện trên màn hình, tiết kiệm bộ nhớ khi làm việc với danh sách dài.

### Các loại GridView
- `GridView` có ba cách sử dụng phổ biến:
    - `GridView.count` - Dùng khi biết trước số lượng cột trong lưới.
    - `GridView.builder` - Dùng khi danh sách phần tử có thể thay đổi hoặc tải động từ API.
    - `GridView.extent` - Dùng khi muốn xác định kích thước tối đa cho mỗi phần tử thay vì số lượng cột.

- **Cấu trúc cơ bản của GridView**

```dart
GridView(
  gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
    crossAxisCount: 2, // Số lượng cột
    crossAxisSpacing: 10, // Khoảng cách giữa các cột
    mainAxisSpacing: 10,  // Khoảng cách giữa các dòng
  ),
  children: [
    Container(color: Colors.red),
    Container(color: Colors.green),
    Container(color: Colors.blue),
    // Các widget khác...
  ],
)
```

#### GridView.count
- Sử dụng khi  biết trước số lượng cột trong lưới.

```dart
GridView.count(
  crossAxisCount: 3, // Số cột
  crossAxisSpacing: 10, // Khoảng cách giữa các cột
  mainAxisSpacing: 10,  // Khoảng cách giữa các hàng
  padding: EdgeInsets.all(8.0), // Padding chung cho GridView
  children: List.generate(20, (index) {
    return Container(
      color: Colors.blue[(index % 9) * 100], // Tạo màu sắc khác nhau cho các phần tử
      child: Center(
        child: Text(
          'Item $index',
          style: TextStyle(color: Colors.white),
        ),
      ),
    );
  }),
)
```
#### GridView.builder
- Dùng khi cần tạo một danh sách các phần tử từ dữ liệu động, giúp tối ưu hóa bộ nhớ bằng cách chỉ tạo phần tử khi chúng được cuộn vào màn hình.

```dart
GridView.builder(
  gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
    crossAxisCount: 3,  // Số cột
    crossAxisSpacing: 10, 
    mainAxisSpacing: 10, 
  ),
  itemCount: 50,  // Số lượng phần tử
  itemBuilder: (context, index) {
    return Container(
      color: Colors.orange[(index % 9) * 100], 
      child: Center(
        child: Text(
          'Item $index',
          style: TextStyle(color: Colors.white),
        ),
      ),
    );
  },
)
```

- **Ưu điểm của GridView.builder:**
  - Tối ưu hóa bộ nhớ vì chỉ tạo các phần tử cần thiết khi chúng xuất hiện trong viewport.
  - Rất phù hợp cho dữ liệu động hoặc khi  không biết trước số lượng phần tử.

#### GridView.extent
- Sử dụng khi muốn xác định kích thước tối đa cho mỗi phần tử, thay vì số lượng cột. 
- Nó sẽ tự động chia không gian thành các cột sao cho mỗi cột có chiều rộng tối đa.

```dart
GridView.extent(
  maxCrossAxisExtent: 200,  // Kích thước tối đa cho mỗi phần tử
  crossAxisSpacing: 10,     // Khoảng cách giữa các cột
  mainAxisSpacing: 10,      // Khoảng cách giữa các dòng
  children: List.generate(20, (index) {
    return Container(
      color: Colors.purple[(index % 9) * 100], 
      child: Center(
        child: Text(
          'Item $index',
          style: TextStyle(color: Colors.white),
        ),
      ),
    );
  }),
)
```

#### Thuộc tính chính của GridView

| Thuộc tính           | Mô tả                                                                                                                                      |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `gridDelegate`       | Điều khiển cách lưới được phân chia. Có thể sử dụng `SliverGridDelegateWithFixedCrossAxisCount, SliverGridDelegateWithMaxCrossAxisExtent`. |
| `crossAxisCount`     | Số lượng cột trong `GridView` (chỉ sử dụng trong `SliverGridDelegateWithFixedCrossAxisCount`).                                             |
| `maxCrossAxisExtent` | Chiều rộng tối đa của mỗi phần tử trong GridView (chỉ sử dụng trong `SliverGridDelegateWithMaxCrossAxisExtent`).                           |
| `crossAxisSpacing`   | Khoảng cách giữa các phần tử theo chiều ngang.                                                                                             |
| `mainAxisSpacing`    | Khoảng cách giữa các phần tử theo chiều dọc.                                                                                               |
| `children`           | Danh sách các widget con sẽ được hiển thị trong lưới.                                                                                      |
| `itemCount`          | Số lượng phần tử trong `GridView.builder`.                                                                                                 |
| `itemBuilder`        | Hàm callback để tạo phần tử tại mỗi chỉ mục trong `GridView.builder`.                                                                      |

#### So sánh vs ListView

| Đặc điểm          | GridView                              | ListView                                           |
| ----------------- | ------------------------------------- | -------------------------------------------------- |
| Cách bố trí       | Lưới (Grid), có thể tùy chỉnh số cột  | Danh sách theo chiều dọc hoặc ngang                |
| Sử dụng           | Hiển thị nhiều phần tử theo dạng lưới | Hiển thị các mục trong danh sách                   |
| Tối ưu hóa bộ nhớ | Tốt hơn cho danh sách lớn             | Tốt cho danh sách nhỏ hoặc khi số lượng phần tử ít |

## Expanded, Flexible
### Expanded
- Là một widget rất hữu ích trong Flutter, được sử dụng để mở rộng một widget con trong một Column, Row, hoặc Flex. 
- Nó giúp widget con chiếm tất cả không gian còn lại trong một widget cha (có thể là Column, Row, hoặc Flex) sau khi các widget khác đã chiếm không gian cần thiết.
- **Công dụng chính:**
  - **Phân bổ không gian tự động:** Khi sử dụng `Expanded`, widget sẽ mở rộng để chiếm không gian còn lại sau khi các phần tử khác đã được sắp xếp.
  - **Hỗ trợ bố trí linh hoạt:** Giúp sắp xếp các phần tử trong các layout như `Column, Row` theo cách linh hoạt.
  - Có 1 thuộc tính duy nhất là `child`

- **Lưu ý khi sử dụng:**
  - **Chỉ sử dụng với các widget có thể mở rộng:** Expanded chỉ có hiệu quả khi sử dụng trong các widget như `Column, Row, hoặc Flex`. Nó không hoạt động tốt nếu bạn sử dụng với widget không thể tự động thay đổi kích thước, như `Container` có kích thước cố định.
  - **Không nên lồng nhiều Expanded trong một Row hoặc Column mà không xác định rõ không gian cho chúng:** Nếu có nhiều widget Expanded mà không tính toán không gian hợp lý, chúng có thể làm cho layout trở nên khó kiểm soát hoặc tràn ra ngoài màn hình.


### Sự khác biệt với Flexible
- `Flexible:` Tương tự như `Expanded`, nhưng có thể chỉ định tỷ lệ phần trăm của không gian mà một widget con chiếm được trong `Column, Row, hoặc Flex`. 
- Trong khi E`xpanded` sẽ chiếm tất cả không gian còn lại, `Flexible` có thể chiếm một phần của không gian đó dựa trên `flex value`.

```dart
Row(
  children: [
    Flexible(
      flex: 2, // Chiếm gấp 2 lần không gian so với phần tử Flexible khác
      child: Container(color: Colors.red),
    ),
    Flexible(
      flex: 1, // Chiếm 1 phần không gian
      child: Container(color: Colors.green),
    ),
  ],
)
```

## Navigate, dialog, bottom sheet
### Navigate
- Điều hướng giúp chuyển giữa các màn hình trong ứng dụng. 
- Flutter cung cấp `Navigator` để thực hiện điều này.

#### Dùng Navigator.push để điều hướng tới màn mới

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: FirstScreen(),
    );
  }
}

class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("First Screen")),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // Điều hướng đến SecondScreen
            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => SecondScreen()),
            );
          },
          child: Text('Go to Second Screen'),
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Second Screen")),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // Quay lại màn hình trước
            Navigator.pop(context);
          },
          child: Text('Back to First Screen'),
        ),
      ),
    );
  }
}
```
- `Navigator.push`: Điều hướng đến màn hình mới.
- `Navigator.pop`: Quay lại màn hình trước

#### Dùng Navigator.pushReplacement để thay thế màn hiện tại
```dart
Navigator.pushReplacement(
  context,
  MaterialPageRoute(builder: (context) => NewScreen()),
);
```

### Dialog
- Hộp thoại là một giao diện nhỏ xuất hiện trên màn hình hiện tại để người dùng có thể thực hiện hành động hoặc cung cấp thông tin.
- **Hiển thị Dialog đơn giản:**

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: FirstScreen(),
    );
  }
}

class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Dialog Example")),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            showDialog(
              context: context,
              builder: (context) {
                return AlertDialog(
                  title: Text('This is a dialog'),
                  content: Text('Do you want to proceed?'),
                  actions: <Widget>[
                    TextButton(
                      onPressed: () {
                        Navigator.of(context).pop(); // Đóng dialog
                      },
                      child: Text('Cancel'),
                    ),
                    TextButton(
                      onPressed: () {
                        Navigator.of(context).pop(); // Đóng dialog
                        // Thực hiện hành động khác
                      },
                      child: Text('Proceed'),
                    ),
                  ],
                );
              },
            );
          },
          child: Text('Show Dialog'),
        ),
      ),
    );
  }
}
```
- `showDialog:` Hiển thị một hộp thoại.
- `AlertDialog:` Hộp thoại chuẩn, có tiêu đề, nội dung và các nút hành động.

- **Custom Dialog**

```dart
showDialog(
  context: context,
  builder: (context) {
    return Dialog(
      shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(20)),
      child: Padding(
        padding: const EdgeInsets.all(20),
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: <Widget>[
            Text('Custom Dialog'),
            ElevatedButton(
              onPressed: () {
                Navigator.of(context).pop(); // Đóng dialog
              },
              child: Text('Close'),
            ),
          ],
        ),
      ),
    );
  },
);
```

### Bottom Sheet
- `Bottom Sheet` là một giao diện hiển thị ở phần dưới màn hình. 
- Có thể sử dụng `showModalBottomSheet` hoặc `showBottomSheet` để hiển thị.

- **Hiển thị Bottom Sheet (Modal Bottom Sheet):**

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: FirstScreen(),
    );
  }
}

class FirstScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Bottom Sheet Example")),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            showModalBottomSheet(
              context: context,
              builder: (context) {
                return Container(
                  height: 200,
                  color: Colors.blueAccent,
                  child: Center(
                    child: Text('This is a Bottom Sheet', style: TextStyle(color: Colors.white)),
                  ),
                );
              },
            );
          },
          child: Text('Show Bottom Sheet'),
        ),
      ),
    );
  }
}
```
- `showModalBottomSheet:` Hiển thị Bottom Sheet ở phía dưới màn hình. Đây là một dạng bảng thông báo có thể kéo lên và kéo xuống.

- **Hiển thị Bottom Sheet cố định (Persistent Bottom Sheet):**

```dart
Scaffold(
  body: Center(child: Text('Main Screen')),
  bottomSheet: Container(
    color: Colors.green,
    height: 100,
    child: Center(child: Text('This is a Persistent Bottom Sheet')),
  ),
);
```
- `bottomSheet` trong `Scaffold` là một `Bottom Sheet` cố định, không có khả năng kéo lên xuống.

> **Navigate:** Điều hướng giữa các màn hình với `Navigator.push, Navigator.pop,` hoặc thay thế màn hình với `Navigator.pushReplacement.`
> 
> **Dialog:** Hiển thị các hộp thoại (cảnh báo, xác nhận, tùy chỉnh) với `showDialog và AlertDialog.`
> 
> **Bottom Sheet:** Hiển thị bảng thông báo ở phía dưới màn hình với `showModalBottomSheet` hoặc sử dụng `bottomSheet` trong `Scaffold` để hiển thị `Bottom Sheet` cố định.

## Pub.dev
- `pub.dev` là một kho lưu trữ gói (package repository) chính thức của Flutter và Dart, nơi có thể tìm kiếm và tải về các gói thư viện để sử dụng trong các dự án của mình.
- Mỗi gói thư viện trên `pub.dev` có thể cung cấp các chức năng như UI components, API integration, state management, và nhiều hơn nữa.
- **Chức năng chính:**
  - **Tìm kiếm và khám phá gói thư viện:** Có thể tìm kiếm gói thư viện theo tên, mục đích sử dụng, hoặc theo độ phổ biến.
  - **Thông tin về gói thư viện:** Mỗi gói thư viện trên `pub.dev` đều có thông tin chi tiết về cách sử dụng, phiên bản hiện tại, sự hỗ trợ và tài liệu liên quan.
  - **Cập nhật và quản lý phiên bản gói:** Bạn có thể cập nhật gói của mình thông qua file `pubspec.yaml` hoặc sử dụng các lệnh của `Flutter` hoặc `Dart`.
- **Các thành phần của pub.dev**
  - `Install (Cài đặt):` Hướng dẫn cài đặt gói thư viện vào dự, bao gồm việc thêm vào file pubspec.yaml.
  - `Example:` Mẫu code minh họa cách sử dụng gói thư viện.
  - `Changelog:` Các thay đổi, bản vá, tính năng mới trong các phiên bản của gói.
  - `Versions:` Liệt kê tất cả các phiên bản của gói.