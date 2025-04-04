- [Networking](#networking)
  - [Tổng quan về networking](#tổng-quan-về-networking)
    - [Kiến trúc client - sever](#kiến-trúc-client---sever)
      - [Nguyên tắc hoạt động](#nguyên-tắc-hoạt-động)
      - [Ưu và nhược điểm của mô hình mạng khách chủ](#ưu-và-nhược-điểm-của-mô-hình-mạng-khách-chủ)
      - [Ví dụ về mô hình Client – Server](#ví-dụ-về-mô-hình-client--server)
    - [Giao thức HTTP/HTTPS](#giao-thức-httphttps)
      - [HTTP (Hypertext Transfer Protocol)](#http-hypertext-transfer-protocol)
      - [HTTPS (Hypertext Transfer Protocol Secure)](#https-hypertext-transfer-protocol-secure)
    - [IP, Port, URL, và DNS](#ip-port-url-và-dns)
      - [Địa chỉ IP](#địa-chỉ-ip)
      - [Cổng Port](#cổng-port)
      - [URL](#url)
      - [DNS](#dns)
    - [Phân biệt Uri và Url. Lấy ví dụ](#phân-biệt-uri-và-url-lấy-ví-dụ)
      - [URI](#uri)
  - [RESTful API](#restful-api)
    - [API là gì](#api-là-gì)
    - [RESTful API là gì? Nguyên tắc của RESTful API?](#restful-api-là-gì-nguyên-tắc-của-restful-api)
      - [Nguyên tắc của RESTful API](#nguyên-tắc-của-restful-api)
      - [Các phương thức HTTP tong RESTful API](#các-phương-thức-http-tong-restful-api)
      - [API endpoint](#api-endpoint)
    - [Request: Header, Body, Method](#request-header-body-method)
      - [Request Header (Tiêu đề yêu cầu)](#request-header-tiêu-đề-yêu-cầu)
      - [Request Body](#request-body)
    - [Response: Status Code, Body](#response-status-code-body)
      - [Response Body](#response-body)
    - [JSON và cách nó được sử dụng trong RESTful API](#json-và-cách-nó-được-sử-dụng-trong-restful-api)
      - [Cấu trúc cơ bản của Json](#cấu-trúc-cơ-bản-của-json)
      - [Json trong RESTful API](#json-trong-restful-api)
      - [Ưu điểm của JSON trong RESTful API](#ưu-điểm-của-json-trong-restful-api)
      - [Cách xử lý JSON trong RESTful API](#cách-xử-lý-json-trong-restful-api)
  - [cURL và Postman](#curl-và-postman)
    - [cURL](#curl)
    - [Postman](#postman)
  - [Lib HTTP](#lib-http)
    - [Cài đặt thư viện http](#cài-đặt-thư-viện-http)
    - [Các phương thức HTTP cơ bản](#các-phương-thức-http-cơ-bản)
      - [Gửi yêu cầu GET](#gửi-yêu-cầu-get)
      - [Gửi yêu cầu POST](#gửi-yêu-cầu-post)
      - [Gửi yêu cầu PUT](#gửi-yêu-cầu-put)
      - [Gửi yêu cầu DELETE](#gửi-yêu-cầu-delete)
    - [Xử lý lỗi](#xử-lý-lỗi)
    - [Sử dụng với FutureBuilder trong UI](#sử-dụng-với-futurebuilder-trong-ui)
    - [Sử dụng http.Client để tối ưu hóa](#sử-dụng-httpclient-để-tối-ưu-hóa)
    - [Sử dụng http với Bloc](#sử-dụng-http-với-bloc)
    - [Kết hợp http với StreamBuilder trong Flutter](#kết-hợp-http-với-streambuilder-trong-flutter)

# Networking
## Tổng quan về networking
### Kiến trúc client - sever
- Mô hình `client-server` là một mô hình mạng máy tính gồm hai thành phần chính là `client và server`
- `Client` sẽ là bên yêu cầu dịch vụ cài đặt cũng như lưu trữ tài nguyên từ `server`.
- Khi `client` gửi yêu cầu dữ liệu đến `server` qua Internet, `server` sẽ xử lý yêu cầu và gửi các gói dữ liệu cho client. 
- Mô hình `Client Server` sử dụng cấu trúc phân tán để phân chia nhiệm vụ giữa `client và server`

#### Nguyên tắc hoạt động
- **Client:** Trong mô hình `Client Server`, `Client` là các máy tính truy cập để sử dụng dịch vụ (`còn gọi là Host`) và có khả năng nhận thông tin cụ thể từ nhà cung cấp dịch vụ (`Server`).
- **Server:** `Server` là một máy chủ hay phương tiện để cung cấp các dịch vụ đến client. Thường cấu hình của `server` rất mạnh và luôn kết nối mạng để có thể  phục vụ client 24/7.
- **Cách thứ hoạt động cụ thể:** 
  - Đầu tiên, `client` sẽ gửi cầu đến server thông qua một giao thức mạng.
  - Tiếp theo, `server` sẽ xử lý các yêu cầu từ `client`
  - Cuối cùng, `server` sẽ trả kết quả về cho `client` thường là một trang web, email, file hoặc thông báo lỗi.
  
  ![](https://static-xf1.vietnix.vn/wp-content/uploads/2021/06/client-server-1.webp)
#### Ưu và nhược điểm của mô hình mạng khách chủ
- **Ưu điểm**:
  - **Khả năng tập trung:** Client server có tích hợp Centralization (Tập trung hóa) giúp quản trị viên mạng dễ dàng quản lý và giải quyết vấn đề.
  - **Tính bảo mật:** Dữ liệu được bảo vệ tốt hơn và tránh truy cập trái phép nhờ vào cấu trúc tập trung và các biện pháp hiện đại như `firewall, mã hóa…`
  - **Khả năng mở rộng:** Người dùng có thể tăng số lượng tài nguyên như số lượng client hoặc server mà không bị gián đoạn nhờ khả năng mở rộng tốt của mô hình.
  - **Khả năng truy cập:** Mô hình `client-server` không phân biệt mà cho phép người dùng truy cập thông tin từ bất kỳ vị trí hoặc nền tảng nào.
  - **Hiệu quả cao:** Server có thể xử lý nhiều yêu cầu cùng lúc, giúp tăng hiệu suất tài nguyên cho client.
- **Nhược điểm:**
  - **Tắc nghẽn lưu lượng:** Khi có quá nhiều client gửi request từ cùng 1 server sẽ dẫn đến kết nối chậm và gây tắc nghẽn.
  - **Chi phí:** Bởi vì hệ thống mạng có sức mạnh lớn nên việc bảo trì và thiết lập thường khá cao mà không phải người dùng nào cũng chi trả được.
  - **Độ bền:** Vì `client – server` là mạng tập trung nên server chỉ cần xảy ra sự cố hay bị nhiễu thì toàn bộ hệ thống mạng sẽ bị gián đoạn. Do vậy, các mạng `client – server` sẽ thiếu tính ổn định và độ bền.
  - **Phức tạp:** Mô hình `client-server` có cấu trúc phức tạp và yêu cầu bảo trì thường xuyên. Vì vậy, luôn cần có quản trị viên mạng chuyên dụng để đảm bảo server hoạt động ổn định.
  - **Tài nguyên:** Không phải tất cả tài nguyên trên server đều có thể truy cập từ client. Ví dụ, bạn không thể in tài liệu trực tiếp từ web hoặc chỉnh sửa bất kỳ thông tin nào trên ổ cứng của Client.
#### Ví dụ về mô hình Client – Server
- **Email server:** Các thư điện tử được gửi từ client sẽ được server tiếp nhận email, lưu trữ và gửi email đó đến địa chỉ nhận.
- **File server:** Khi client chia sẻ thông tin đến file server sẽ được lưu trữ và truyền file đi. Ngoài ra, người dùng có thể upload hoặc download các file lên server thông qua giao thức `FTP/web browser.`
- **World wide web (WWW):** Thông qua trình duyệt web người dùng có thể truy cập vào các trang web đã được lưu trữ trên server.
- **Giao dịch online:** Các giao dịch giữa người mua và bán như mua bán hàng hóa, dịch vụ đều được thực hiện thông qua server.
- **Game online:** Các game trực tuyến sẽ được lưu trữ trên server và người chơi sẽ truy cập vào để chơi.

![](https://static-xf1.vietnix.vn/wp-content/uploads/2021/06/client-server-2-2048x1459.webp)

### Giao thức HTTP/HTTPS
#### HTTP (Hypertext Transfer Protocol) 
- Là giao thức truyền tải siêu văn bản. 
- Đây là giao thức tiêu chuẩn cho `World Wide Web (www)` để truyền tải dữ liệu dưới dạng văn bản, âm thanh, hình ảnh, video từ Web Server tới trình duyệt web của người dùng và ngược lại.

>**HTTP** là một giao thức ứng dụng của bộ giao thức `TCP/IP` (các giao thức nền tảng cho Internet). Bộ giao thức `TCP/IP` là một bộ các giao thức truyền thông cài đặt chồng giao thức mà Internet và hầu hết các mạng máy tính thương mại đang chạy trên đó. Bộ giao thức này được đặt theo tên hai giao thức chính là `TCP (Transmission Control Protocol – Giao thức điều khiển truyền vận)` và `IP (Internet Protocol – Giao thức Internet)`.

- Hoạt động theo mô hình `Client - Server` 
- Trong quá trình kết nối và trao đổi thông tin, trình duyệt của bạn sẽ mặc nhiên thừa nhận địa chỉ IP đó đến từ server của chính website mà bạn muốn truy cập mà không hề có biện pháp xác thực nào. 
- Các thông tin được gửi đi qua giao thức `HTTP` (bao gồm địa chỉ IP, các thông tin mà bạn nhập vào website…) cũng không hề được mã hóa và bảo mật.
>Đây chính là kẽ hở mà nhiều hacker đã lợi dụng để đánh cắp thông tin người dùng, thường được gọi là tấn công sniffing.

#### HTTPS (Hypertext Transfer Protocol Secure)
- Là giao thức truyền tải siêu văn bản an toàn.
- Thực chất, đây chính là giao thức `HTTP nhưng tích hợp thêm Chứng chỉ bảo mật SSL` nhằm mã hóa các thông điệp giao tiếp để tăng tính bảo mật. Có thể hiểu, `HTTPS là phiên bản HTTP an toàn, bảo mật hơn.`
- HTTPS hoạt động tương tự như HTTP, tuy nhiên được bổ sung thêm chứng chỉ `SSL (Secure Sockets Layer – tầng ổ bảo mật)` hoặc `TLS (Transport Layer Security – bảo mật tầng truyền tải)`.
- Cả `SSL` và `TLS` đều sử dụng hệ thống `PKI (Public Key Infrastructure -hạ tầng khóa công khai)` không đối xứng.
- Hệ thống này sử dụng hai `“khóa”` để mã hóa thông tin liên lạc, `“khóa công khai” (public key)` và `“khóa riêng” (private key)`
- Bất cứ thứ gì được mã hóa bằng khóa công khai chỉ có thể được giải mã bởi khóa riêng và ngược lại. 
- Các tiêu chuẩn này đảm bảo các nội dung sẽ được mã hóa trước khi truyền đi, và giải mã khi nhận. Điều này khiến hacker dù có chen ngang lấy được thông tin cũng không thể `“hiểu”` được thông tin đó.

- **HTTP và HTTPS khác nhau như thế nào?:** Sự khác biệt lớn nhất giữa `HTTP và HTTPS` là chứng chỉ SSL. Về cơ bản, `HTTPS` là một giao thức `HTTP` với bảo mật bổ sung.

- **Port trên HTTP và HTTPS:** Port là cổng xác định thông tin trên máy khách, sau đó phân loại để gửi đến máy chủ
  -  Mỗi Port mang một số hiệu riêng với chức năng riêng biệt
  -  Giao thức `HTTP` sử dụng `Port 80`
  -  `HTTPS` sử dụng `Port 443` – đây chính là cổng hỗ trợ mã hóa kết nối từ máy tính client đến server, nhằm bảo vệ gói dữ liệu đang được truyền đi.

### IP, Port, URL, và DNS
#### Địa chỉ IP
- Địa chỉ `IP là viết tắt của Internet Protocol – giao thức Internet`.
- Đây là một địa chỉ đơn nhất mà những thiết bị điện tử hiện nay đang sử dụng để nhận diện và liên lạc với nhau trên mạng máy tính bằng cách sử dụng giao thức `Internet`
- Về bản chất, địa chỉ `IP` là mã định danh cho phép gửi thông tin giữa các thiết bị trên mạng: `IP` chứa thông tin vị trí và giúp các thiết bị điện tử có thể truy cập được để liên lạc.
- `IP` chứa thông tin vị trí và giúp các thiết bị điện tử có thể truy cập được để liên lạc.

#### Cổng Port
- `Port` là giao thức bit 16 đứng đầu các tập tin thuộc hai giao thức `UDP` và `TCP`
- Để thiết bị của có thể nhận và xuất tập tin thì phải cần đăng ký port – thuật toán đã được lập trình trước. 
- Hiện nay, với máy tính sẽ có đến 65535 port được phân làm 3 phần:
  - `WKP: Well Known Port` quy định từ 0 đến 1032.
  - `RP: Registered Port` quy định từ 1024 đến 49151.
  - `D/PP: Dynamic/Private Port` quy định từ 49152 đến 65535.
#### URL
- `URL (Uniform Resource Locator)` là một dãy ký tự độc nhất dùng để xác định vị trí của một nguồn dữ liệu cụ thể trên không gian mạng
- Nó là một địa chỉ duy nhất chỉ đến một tài nguyên cụ thể, chẳng hạn như một trang web, hình ảnh, video, tệp tin, hoặc tài liệu.

- Thông thường, một URL sẽ gồm những thành phần cơ bản như sau:
  - **Giao thức:** http, https, FTP…
  - **World Wide Web:** www (có thể không có thường gọi là non-www)
  - **Tên miền (domain):** www.youtube.com/
  - **Cổng giao tiếp (port):** 443, 80, 2082, 2222…
#### DNS
- `DNS` viết tắt của `Domain Name System` có nghĩa là hệ thống phân giải tên miền. DNS là hệ thống cho phép thiết lập tương ứng giữa địa chỉ IP và tên miền trên Internet.
- **Chức năng**: DNS đóng vai trò như một “biên dịch viên” giữa tên miền và địa chỉ IP
  - Tên miền là chuỗi ký tự dễ nhớ, trong khi địa chỉ IP là chuỗi số khó nhớ
  - DNS giúp chuyển đổi tên miền thành địa chỉ IP, từ đó giúp máy tính có thể truy cập vào các trang web trên internet.
  ![](https://longvan.net/hinhanh/tintuc/dnsserverlahethongnhuthenao.png)
- Để lấy được IP của 1 tên miền thì có thể có sự tham gia của 4 loại DNS server:
  - ***DNS Recursor:*** đảm nhận việc đi tìm địa chỉ IP và trả về kết quả cho client. Để tìm địa chỉ IP thì nó sẽ dùng các DNS server khác ở dưới. Ngoài ra thì DNS Recursor sẽ **có cơ chế cache để tăng tốc độ phản hồi** thay vì lúc nào cũng đi tìm IP.
  - ***Root Name Server:*** Là nơi xử lý bước đầu tiên trong việc dịch (phân giải) các tên máy chủ thành địa chỉ IP. Nó có thể được coi giống như một mục lục trong thư viện trỏ đến các giá sách khác nhau - đóng vai trò như một tham chiếu đến các DNS Server khác. Vd: Ta request địa chỉ IP của [google.com](http://google.com/) thì Root name server sẽ trả về IP máy chủ DNS của .com
  - ***TLD Name Server (Top level domain name server):*** Có thể được coi như một giá sách cụ thể trong thư viện. Máy chủ định danh này là bước tiếp theo trong quá trình tìm kiếm địa chỉ IP cụ thể và nó lưu trữ phần cuối cùng của tên máy chủ (Vd [google.com](http://google.com/), máy chủ TLD sẽ trả về IP của DNS chứa .google”).
  - ***Authoritative Name Server:*** Có thể được coi như một cuốn từ điển trên giá sách. Là điểm dừng cuối cùng trong truy vấn địa và sẽ trả về địa chỉ IP của tên miền được yêu cầu cho **DNS Recursor**.
  ![](https://cloud-patch-e32.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F01ebe1db-9e71-42de-919f-82ad19395582%2F8ff70563-eb5a-43e0-a70b-185806fa5db9%2Fimage.png?table=block&id=14d6be34-044c-80f3-9c21-c8a23f806258&spaceId=01ebe1db-9e71-42de-919f-82ad19395582&width=1160&userId=&cache=v2)
### Phân biệt Uri và Url. Lấy ví dụ
#### URI
- `URI (Uniform Resource Identifier)` là một chuỗi chứa các ký tự xác định một tài nguyên vật lý hoặc logic. URI tuân theo các quy tắc cú pháp để đảm bảo tính đồng nhất. Hơn nữa, nó cũng duy trì khả năng mở rộng thông qua một lược đồ đặt tên phân cấp.
![](https://images.viblo.asia/fe56dc7d-ee5d-4d62-bdf6-2cefe493a001.png)
- Có 2 loại URI
  - URL: URL chỉ định một vị trí trên mạng máy tính và kỹ thuật để truy xuất nó.
  - `URN: (Uniform Resource Name) `là một tài nguyên internet chỉ định lược đồ URN.
  
|URL	|URI|
|---|---|
URL là viết tắt của Uniform Resource Locator.	|URI là viết tắt của Uniform Resource Identity.|
|URL là một tập hợp con của URI chỉ định nơi tài nguyên tồn tại và cơ chế để truy xuất tài nguyên đó.	|URI là một tập hợp siêu URL xác định tài nguyên bằng URL hoặc URN (Tên tài nguyên thống nhất) hoặc cả hai.
|Mục đích chính là lấy vị trí hoặc địa chỉ của tài nguyên.	|Mục đích chính của URI là tìm một tài nguyên và phân biệt nó với các tài nguyên khác bằng cách sử dụng tên hoặc vị trí.|
|URL chỉ được sử dụng để định vị các trang web	|Được sử dụng trong HTML, XML và các tệp khác XSLT (Extensible Stylesheet Language Transformations) và hơn thế nữa.|
|Lược đồ phải là một giao thức như HTTP, FTP, HTTPS, v.v.	|Trong URI, lược đồ có thể là bất kỳ thứ gì như giao thức, đặc tả, tên, v.v.|
|Thông tin giao thức được cung cấp trong URL.	|Không có thông tin giao thức được cung cấp trong URI.|
|Ví dụ về URL: https://google.com	|Ví dụ về URI: urn: isbn: 0-486-27557-4|
|Nó chứa các thành phần như giao thức, miền, đường dẫn, hash, chuỗi truy vấn, v.v.	|Nó chứa các thành phần như lược đồ, quyền hạn, đường dẫn, truy vấn, thành phần phân mảnh, v.v.|
|Tất cả các URL có thể là URI	|Không phải tất cả các URI đều là URL vì URI có thể là một name thay vì một locator.|
## RESTful API
### API là gì
- **API - Application Programming Interface**: là các phương thức, giao thức kết nối với các thư viện và ứng dụng khác
- Cung cấp khả năng truy xuất đến một tập các hàm hay dùng. Và từ đó có thể trao đổi dữ liệu giữa các ứng dụng.

![](https://topdev.vn/blog/wp-content/uploads/2019/06/API-768x402.png)
- ***API SOAP:*** Các API này sử dụng Giao thức truy cập đối tượng đơn giản. Máy chủ và máy khách trao đổi thông đệp bằng XML. Đây là loại API kém linh hoạt được dùng phổ biến trước đây.
- ***API RPC:*** Những API này được gọi là Lệnh gọi thủ tục từ xa. Máy khách hoàn thành một hàm (hoặc thủ tục) trên máy chủ còn máy chủ gửi kết quả về cho máy khách.
- ***API Websocket:*** là một bản phát triển API web hiện đại khác sử dụng các đối tượng JSON để chuyển dữ liệu. API WebSocket hỗ trợ hoạt động giao tiếp hai chiều giữa ứng dụng máy khách và máy chủ. Máy chủ có thể gửi thông điệp gọi lại cho các máy khách được kết nối, điều này khiến loại API này hiệu quả hơn ***API REST***.
- ***API REST:*** Đây là loại API phổ biến và linh hoạt nhất trên web hiện nay. Máy khách gửi yêu cầu đến máy chủ dưới dạng dữ liệu. Máy chủ dùng dữ liệu đầu vào từ máy khách này để bắt đầu các hàm nội bộ và trả lại dữ liệu đầu ra cho máy khách. Nó sử dụng phương thức HTTP đơn giản để tạo cho giao tiếp giữa các máy. Vì vậy, thay vì sử dụng một URL cho việc xử lý một số thông tin người dùng, ***REST*** gửi một yêu cầu HTTP như `GET, POST, DELETE,...` đến một URL để xử lý dữ liệu.
### RESTful API là gì? Nguyên tắc của RESTful API?
- **RESTful API** là một tiêu chuẩn dùng trong việc thiết kế API cho các ứng dụng web (thiết kế Web services) để tiện cho việc quản lý các resource. Nó chú trọng vào tài nguyên hệ thống (tệp văn bản, ảnh, âm thanh, video, hoặc dữ liệu động…), bao gồm các trạng thái tài nguyên được định dạng và được truyền tải qua HTTP.
  - **`API (Application Programming Interface)`** là một tập các quy tắc và cơ chế mà theo đó, một ứng dụng hay một thành phần sẽ tương tác với một ứng dụng hay thành phần khác. `API` có thể trả về dữ liệu mà bạn cần cho ứng dụng của mình ở những kiểu dữ liệu phổ biến như `JSON hay XML.`
  - **`REST (REpresentational State Transfer)`** là một dạng chuyển đổi cấu trúc dữ liệu, một kiểu kiến trúc để viết API. Nó sử dụng phương thức `HTTP` đơn giản để tạo cho giao tiếp giữa các máy. Vì vậy, thay vì sử dụng một URL cho việc xử lý một số thông tin người dùng, `REST` gửi một yêu cầu `HTTP` như `GET, POST, DELETE, `vv đến một URL để xử lý dữ liệu.

![](https://topdev.vn/blog/wp-content/uploads/2019/04/restful-rest-diagram-api.jpg)

- **REST** hoạt động chủ yếu dựa vào giao thức `HTTP`. Các hoạt động cơ bản nêu trên sẽ sử dụng những phương thức HTTP riêng. (CRUD)
  - **GET (SELECT):** Trả về một Resource hoặc một danh sách Resource.
  - **POST (CREATE):** Tạo mới một Resource.
  - **PUT (UPDATE):** Cập nhật thông tin cho Resource.
  - **DELETE (DELETE):** Xoá một Resource.

#### Nguyên tắc của RESTful API
- **Client-Server (Máy khách - Máy chủ):**
  -  Phân tách rõ ràng giữa client (giao diện người dùng) và server (lưu trữ và xử lý dữ liệu).
  - Client chỉ cần biết cách sử dụng API mà không cần biết chi tiết bên trong server.
- **Stateless (Không trạng thái):**
  - Mỗi yêu cầu từ client phải chứa đủ thông tin (như xác thực, tham số, v.v.) để server xử lý.
  - Server không lưu trạng thái của client giữa các yêu cầu. Điều này giúp API dễ mở rộng hơn.
- **Cacheable (Có thể cache):**
  - Dữ liệu từ RESTful API có thể được cache (lưu tạm thời) để cải thiện hiệu suất.
  - API nên chỉ định rõ những dữ liệu nào có thể cache (qua các header HTTP như Cache-Control).
- **Uniform Interface (Giao diện thống nhất):**
  - API phải tuân thủ một giao diện nhất quán, giúp client dễ dàng sử dụng.
  - Mỗi tài nguyên được đại diện bởi một URI duy nhất.
- **Layered System (Hệ thống phân lớp):**
  - API có thể có nhiều lớp trung gian (proxy, gateway, load balancer).
  - Client không cần biết các lớp này; chỉ cần giao tiếp với API endpoint.
- **Code on Demand (Tùy chọn):**
  - Server có thể gửi mã (thường là JavaScript) để client thực thi.
  - Đây là một tính năng tùy chọn, không bắt buộc trong RESTful API.
#### Các phương thức HTTP tong RESTful API
- **HTTP Request:** `HTTP request` có tất cả 9 loại method , 2 loại được sử dụng phổ biến nhất là G`ET` và `POST`
  - **GET:** được sử dụng để lấy thông tin từ server theo URI đã cung cấp.
  - **HEAD:** giống với `GET` nhưng response trả về `không có body`, chỉ có `header`.
  - **POST:** gửi thông tin tới sever thông qua các biểu mẫu http.
  - **PUT:** ghi đè tất cả thông tin của đối tượng với những gì được gửi lên.
  - **PATCH:** ghi đè các thông tin được thay đổi của đối tượng.
  - **DELETE:** xóa tài nguyên trên server.
  - **CONNECT:** thiết lập một kết nối tới server theo URI.
  - **OPTIONS:** mô tả các tùy chọn giao tiếp cho resource.
  - **TRACE:** thực hiện một bài test loop – back theo đường dẫn đến resource.

#### API endpoint
- **Base URL:** Đây là địa chỉ chính của API, đại diện cho server hoặc hệ thống mà API đang hoạt động.
- Thường bao gồm:
  - **Giao thức:** http:// hoặc https://.
  - **Tên miền hoặc địa chỉ IP:** Tên miền của server cung cấp API.
  - **Port (nếu cần):** Mặc định là 80 cho HTTP và 443 cho HTTPS, nhưng có thể chỉ định khác.
  - **Ví dụ:** Base URL của GoongMap: `https://rsapi.goong.io/`, `https://api.example.com`

- **Endpoint:** Là phần mở rộng của `Base URL`, chỉ định cụ thể tài nguyên hoặc hành động mà bạn muốn truy cập.
- Được tổ chức theo danh từ để đại diện tài nguyên, không phải động từ.
- Thường có dạng:
  - /users (Danh sách người dùng).
  - /users/{id} (Thông tin chi tiết của một người dùng).
  - **Ví dụ:**` /Place/AutoComplete` => https://rsapi.goong.io/Place/AutoComplete

- **Path Parameters:** Các tham số được nhúng trong đường dẫn (path) của Endpoint.
  - Thường được dùng để xác định cụ thể một tài nguyên.
  - Được đánh dấu bằng {} khi định nghĩa.
  - Endpoint: `/users/{id}`
    - URL đầy đủ với Path Parameter: `https://api.example.com/users/123`
    - 123 là giá trị của tham số id.
- **Query Parameters**
  - Các tham số được thêm vào cuối URL, sau dấu `?`, để lọc hoặc tùy chỉnh dữ liệu trả về.
  - Dạng key=value, có thể nối nhiều tham số bằng dấu `&`.
  `https://rsapi.goong.io/Place/AutoComplete?api_key=your_api_key&location=21.0285,105.8542&input=coffee`

### Request: Header, Body, Method
- Phương thức `HTTP` chỉ định hành động mà `client` muốn thực hiện trên server. Các phương thức phổ biến gồm:
  - **GET:**
    - Lấy thông tin từ server.
    - Không có body, thường chỉ gửi qua URL.
    - Ví dụ: Lấy danh sách người dùng từ API.
  - **POST:**
    - Gửi dữ liệu lên server để tạo mới tài nguyên.
    - Dữ liệu được gửi qua body.
    - Ví dụ: Tạo tài khoản mới.
  - **PUT:**
    - Cập nhật toàn bộ tài nguyên hiện có trên server.
    - Dữ liệu cập nhật được gửi qua body.
  - **PATCH:**
    - Cập nhật một phần tài nguyên.
    - Dữ liệu được gửi qua body.
  - **DELETE:**
    - Xóa tài nguyên trên server.
    - Thường không có body.
```Kotlin
POST /users HTTP/1.1
```
#### Request Header (Tiêu đề yêu cầu)
- `Headers` chứa thông tin bổ sung về request, chẳng hạn như loại dữ liệu, định dạng mã hóa, thông tin xác thực, hoặc cache.
- **Một số Header phổ biến:**
  - **Authorization:** Chứa thông tin xác thực (API Key, token, v.v.).
    - Ví dụ: `Authorization: Bearer your_token`
  - **Content-Type:** Xác định định dạng của dữ liệu trong body.
    - Ví dụ: `application/json, application/x-www-form-urlencoded.`
  - **Accept:** Chỉ định loại dữ liệu mà client mong muốn nhận từ server.
    - Ví dụ: `application/json, text/html.`
  - **User-Agent:** Thông tin về ứng dụng client thực hiện request.
    - Ví dụ: `User-Agent: Mozilla/5.0.`
  - **Cache-Control:** Hướng dẫn client/server về cách cache tài nguyên.
    - Ví dụ: `Cache-Control: max-age=3600`, `Cache-Control: no-cache`
```http
Content-Type: application/json
Authorization: Bearer your_token
Accept: application/json
```
#### Request Body
- Body chứa dữ liệu mà client muốn gửi tới server, thường dùng trong các phương thức như `POST, PUT, và PATCH.`
- Dạng dữ liệu phổ biến trong Body:
  - **Json**
  ```json
  {
    "username": "johndoe",
    "password": "123456"
  }
  ```
  - **Form Data (URL-encoded):** `username=johndoe&password=123456`
  - **Binary Data:** Dùng khi tải tệp (file upload), ví dụ như hình ảnh hoặc tài liệu.
- Ví dụ về HTTP Request
```http
POST /users HTTP/1.1
Host: api.example.com
Authorization: Bearer your_token
Content-Type: application/json
Accept: application/json

{
  "username": "johndoe",
  "email": "johndoe@example.com",
  "password": "123456"
}
```
- Method: `POST`
- Header:
  - `Authorization: Bearer your_token`
  - `Content-Type: application/json`
  - `Accept: application/json`
- Body:
```json
{
  "username": "johndoe",
  "email": "johndoe@example.com",
  "password": "123456"
}
```
### Response: Status Code, Body
- Cấu trúc **HTTP response** gần giống với **HTTP request**, chỉ khác nhau là thay vì **Request-Line**, thì **HTTP response** có **Status-Line**. Và giống như **Request-Line**, **Status-Line** cũng có ba phần như sau:
  - ***HTTP-version:*** phiên bản HTTP cao nhất mà server hỗ trợ.
  - ***Status-Code:*** mã kết quả trả về.
  - **Reason-Phrase:** mô tả về Status-Code.
  ![](https://cloud-patch-e32.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F01ebe1db-9e71-42de-919f-82ad19395582%2F572d2ad2-bbe6-4912-b540-56d7f3b5ea2d%2Fimage.png?table=block&id=14f6be34-044c-80da-8d36-d96592961f34&spaceId=01ebe1db-9e71-42de-919f-82ad19395582&width=1160&userId=&cache=v2)
- **Status Code:**
  - **1xx (Information)**: Đang xử lý request.
  - **2xx (Success)**: Thành công
    - **200 OK**: Thành công.
    - **202 Accepted**: request đã được nhận, nhưng không có kết quả nào trả về, thông báo cho client tiếp tục chờ đợi.
    - **204 No content**: request đã được xử lý nhưng không có thành phần nào được trả về.
    - **205 Reset**: giống như 204 nhưng mã này còn yêu câu client reset lại document view.
    - **206 Partial Content**: server chỉ gửi về một phần dữ liệu, phụ thuộc vào giá trị range header của client đã gửi.
  - **3xx Redirection**: server thông báo cho client phải thực hiện thêm thao tác để hoàn tất request:
    - **301 Moved Permanently**: tài nguyên đã được chuyển hoàn toàn tới địa chỉ Location trong HTTP response.
    - **303 See other**: tài nguyên đã được chuyển tạm thời tới địa chỉ Location trong HTTP response.
    - **304 Not Modified**: tài nguyên không thay đổi từ lần cuối client request, nên client có thể sử dụng đã lưu trong cache.
  - **4xx Client error**: lỗi của client:
    - **400 Bad Request**: request không đúng dạng, cú pháp.
    - **401 Unauthorized**: client chưa xác thực.
    - **403 Forbidden**: client không có quyền truy cập.
    - **404 Not Found**: không tìm thấy tài nguyên.
    - **405 Method Not Allowed**: phương thức không được server hỗ trợ.
  - **5xx Server Error**: lỗi của server:
    - **500 Internal Server Error**: có lỗi trong quá trình xử lý của server.
    - **501 Not Implemented**: server không hỗ trợ chức năng client yêu cầu.
    - **503 Service Unavailable**: Server bị quá tải, hoặc bị lỗi xử lý.
#### Response Body
- Body chứa dữ liệu mà server trả về, thường ở dạng JSON, XML, HTML, hoặc plain text.
- **Json**
```json
{
  "status": "success",
  "data": {
    "id": 1,
    "name": "John Doe",
    "email": "johndoe@example.com"
  }
}

```
- **XML (ít phổ biến hơn):**
```xml
<response>
    <status>success</status>
    <data>
        <id>1</id>
        <name>John Doe</name>
        <email>johndoe@example.com</email>
    </data>
</response>
```
- **HTML (thường dùng cho web)**
```html
<html>
    <body>
        <h1>Welcome</h1>
    </body>
</html>
```
- **Plain Text (ít dữ liệu, đơn giản):** `Operation completed successfully.`

- Nội dung body phụ thuộc vào Status Code
  - **200 OK**: Body chứa kết quả của request.
  ```json
  {
    "message": "Request successful",
    "data": [...]
  }
  ```
  - **201 Created:** Body chứa thông tin về tài nguyên vừa được tạo.
  ```json
  {
    "id": 123,
    "message": "Resource created successfully"
  }
  ```
  - **400 Bad Request:** Body chứa thông tin chi tiết về lỗi.
  ```json
  {
    "error": "Invalid parameters",
    "details": {
      "field": "email",
      "message": "Invalid email format"
    }
  }
  ```
  - **500 Internal Server Error:** Thông báo lỗi từ server (nếu có).
  ```json
  {
    "error": "Internal server error",
    "message": "Unexpected condition encountered"
  }
  ```
- Ví dụ
  - **Response khi lấy danh sách người dùng thành công**
  ```http
  HTTP/1.1 200 OK
  Content-Type: application/json
  Content-Length: 85

  {
    "status": "success",
    "data": [
      {
        "id": 1,
        "name": "John Doe",
        "email": "johndoe@example.com"
      }
    ]
  }
  ```
  - **Response khi xảy ra lỗi xác thực**
  ```http
  HTTP/1.1 401 Unauthorized
  Content-Type: application/json
  Content-Length: 51

  {
    "error": "Unauthorized",
    "message": "Invalid API key"
  }
  ```
|Thành phần	|Vai trò|Ví dụ|
|---|---|---|
|Status Code|	Mã trạng thái cho biết kết quả xử lý của server	|200 OK, 404 Not Found, 500 Internal Server Error
|Response Body	|Dữ liệu phản hồi từ server, thường ở dạng JSON, XML, hoặc HTML	|{ "status": "success", "data": [...] }|

### JSON và cách nó được sử dụng trong RESTful API
- **JSON (JavaScript Object Notation)** Là một định dạng dữ liệu có cấu trúc, được thiết kế để con người dễ đọc và máy móc dễ phân tích. Nó được sử dụng rộng rãi trong RESTful API để truyền dữ liệu giữa client và server.
#### Cấu trúc cơ bản của Json
- JSON bao gồm hai thành phần chính:
  - `Key-Value Pairs:` Dữ liệu được lưu dưới dạng cặp khóa-giá trị.
  - `Array:` Danh sách các giá trị.
```json
{
  "name": "John Doe",
  "age": 30,
  "isEmployee": true,
  "skills": ["JavaScript", "Kotlin", "Python"],
  "address": {
    "city": "New York",
    "zip": "10001"
  }
}
```
- Key: `"name", "age", "isEmployee", ...`
- Value: "`John Doe", 30, true,` array `["JavaScript", "Kotlin", "Python"]`, object `{"city": "New York", "zip": "10001"}`.

#### Json trong RESTful API
- **JSON trong Request**: Khi client gửi dữ liệu tới server `(POST, PUT, PATCH)`, body của request thường chứa JSON.
  - **Tạo một người dùng mới**
  ```http
  POST /users HTTP/1.1
  Content-Type: application/json

  {
    "name": "Jane Doe",
    "email": "jane.doe@example.com",
    "password": "securepassword123"
  }
  ```
- **JSON trong Response**: Server thường trả về dữ liệu dưới dạng JSON để phản hồi client.
  - **Lấy danh sách người dùng**
  ```http
  GET /users HTTP/1.1

  HTTP/1.1 200 OK
  Content-Type: application/json

  {
    "status": "success",
    "data": [
      {
        "id": 1,
        "name": "Jane Doe",
        "email": "jane.doe@example.com"
      },
      {
        "id": 2,
        "name": "John Smith",
        "email": "john.smith@example.com"
      }
    ]
  }
  ```
#### Ưu điểm của JSON trong RESTful API
- `Nhẹ và nhanh:` Dễ dàng truyền tải dữ liệu qua mạng.
- `Dễ đọc và viết:` Thân thiện với con người.
- `Hỗ trợ tốt trong các ngôn ngữ lập trình:` Hầu hết các ngôn ngữ như Python, JavaScript, Java, Kotlin, và Go đều có thư viện để xử lý JSON.
- `Tính tương thích tốt:` JSON hoạt động tốt với các cấu trúc REST API hiện đại.

#### Cách xử lý JSON trong RESTful API
- **Trong Server**
  - Parsing Request Body: Server nhận JSON từ client và phân tích để xử lý.
    - Ví dụ: Trong Node.js, dùng express:
  ```javascript
  app.post('/users', (req, res) => {
      const user = req.body; // { name: "Jane Doe", email: "jane.doe@example.com" }
      console.log(user);
      res.status(201).json({ message: "User created", user });
  });
  ```
  - Trả về Response: Server tạo JSON để phản hồi client.
    - Ví dụ:
  ```javascript
  res.status(200).json({
      status: "success",
      data: [{ id: 1, name: "John Doe" }]
  });
  ```
- **Trong Client**
  - **Parsing JSON từ Response:**
    - Trong JavaScript:
  ```javascript
  fetch('https://api.example.com/users')
    .then(response => response.json())
    .then(data => console.log(data));
  ```
  - **Gửi JSON qua Request:**
    - Trong Kotlin:
  ```kotlin
  val jsonBody = JSONObject()
  jsonBody.put("name", "Jane Doe")
  jsonBody.put("email", "jane.doe@example.com")

  val requestBody = jsonBody.toString().toRequestBody("application/json".toMediaType())
  val request = Request.Builder()
      .url("https://api.example.com/users")
      .post(requestBody)
      .build()
  ```
## cURL và Postman
### cURL
Ví dụ với api: https://reqres.in/api/users
- **Câu lệnh gửi yêu cầu GET:**
```bash
curl "https://reqres.in/api/users"
```
- **Câu lệnh gửi yêu cầu POST với JSON body:** 
```bash
curl -X POST "https://ca5208a24fc7e3774bb4.free.beeceptor.com/api/users" -H "Content-Type: application/json" -d "{\"data\":\"aaaaa\"}"
```
- **Câu lệnh gửi yêu cầu POST với dữ liệu form:**
```bash
curl -X POST "https://example.com/api" -d "key=value"
```
- **Câu lệnh gửi yêu cầu GET với query parameters:**
```bash
curl "https://reqres.in/api/users?page=2"
```
- **Câu lệnh gửi yêu cầu với header tùy chỉnh:**
```bash
curl -H "Authorization: Bearer token" "https://example.com/api"
```
- **Câu lệnh gửi yêu cầu với nhiều headers và dữ liệu:**
```bash
curl -X POST "https://ca5208a24fc7e3774bb4.free.beeceptor.com/api/users" -H "Content-Type: application/json" -d "{\"data\":\"aaaaa\"}"
```
- **Câu lệnh gửi yêu cầu với thông tin chi tiết (verbose mode):**
```bash
curl -v https://reqres.in/api/users
```
- **Câu lệnh gửi yêu cầu và lưu kết quả vào file:**
```bash
curl -o test.json https://reqres.in/api/users
```

|Method|syntax|code|
|---|---|---|
|Get Only Response| curl [url]| `curl https://reqres.in/api/users?page=2`|
|Get Response and Headers| curl -i [url]| `curl -i https://reqres.in/api/users`|
|Get Only Header| curl --head [url] or curl -I [url]| `curl -I https://reqres.in/api/users`|
|Get Details Of Client Server Interaction| curl -v [url]|`curl -v https://reqres.in/api/users`|
|Get Even More Details And Log All Interaction To File|curl --trace [FILE] [URL]| `curl --trace log.txt https://reqres.in/api/users`|
|Send Headers In Request| curl -H "Accept:application/json" [url]| `curl -H "Accept:application/json" https://reqres.in/api/users`|
|POST| curl [url] -d "key=value&key2=value2"| `curl https://reqres.in/api/users -d "name:Chinh&job:IT`"|
|- X POST| curl -X POST [url] -H "Accept:application/json" -d '{"key":"value","key2":"value2"}'| `curl -X POST -H "Accept:application/json" https://reqres.in/api/users -d '{"name":"morpheus","job":"leader"}'`|
|PUT| curl -X PUT [url] -d '{"key":"value","key2":"value2"}'| `curl -X PUT https://reqres.in/api/user2/2 -d '{"name":"morpheus","job":"zion resident"}'`|
|DELETE| curl -X DELETE [url]| `curl -X DELETE https://reqres.in/api/users/2`|

### Postman

## Lib HTTP

- Thư viện http trong Flutter là một trong những thư viện phổ biến nhất để thực hiện các yêu cầu HTTP, giúp ứng dụng giao tiếp với API REST hoặc các dịch vụ web khác.

### Cài đặt thư viện http

```yaml
Thêm thư viện vào pubspec.yaml:
dependencies:
  http: ^1.2.0  # Kiểm tra phiên bản mới nhất trên pub.dev
```

### Các phương thức HTTP cơ bản
- Thư viện http hỗ trợ nhiều phương thức như GET, POST, PUT, DELETE, v.v.

#### Gửi yêu cầu GET
- Dùng để lấy dữ liệu từ API.

```dart
import 'package:http/http.dart' as http;
import 'dart:convert';

Future<void> fetchData() async {
  final url = Uri.parse('https://jsonplaceholder.typicode.com/posts/1');
  final response = await http.get(url);

  if (response.statusCode == 200) {
    final data = json.decode(response.body);
    print('Dữ liệu: $data');
  } else {
    print('Lỗi: ${response.statusCode}');
  }
}

```
#### Gửi yêu cầu POST
- Dùng để gửi dữ liệu lên server.

```dart
Future<void> sendData() async {
  final url = Uri.parse('https://jsonplaceholder.typicode.com/posts');
  final response = await http.post(
    url,
    headers: {'Content-Type': 'application/json'},
    body: jsonEncode({'title': 'Flutter', 'body': 'Hello world', 'userId': 1}),
  );

  if (response.statusCode == 201) {
    print('Thành công: ${response.body}');
  } else {
    print('Lỗi: ${response.statusCode}');
  }
}

```
#### Gửi yêu cầu PUT
- Dùng để cập nhật dữ liệu.

```dart
Future<void> updateData() async {
  final url = Uri.parse('https://jsonplaceholder.typicode.com/posts/1');
  final response = await http.put(
    url,
    headers: {'Content-Type': 'application/json'},
    body: jsonEncode({'id': 1, 'title': 'Updated', 'body': 'Updated content', 'userId': 1}),
  );

  if (response.statusCode == 200) {
    print('Cập nhật thành công: ${response.body}');
  } else {
    print('Lỗi: ${response.statusCode}');
  }
}
```
#### Gửi yêu cầu DELETE
- Dùng để xóa dữ liệu.

```dart
Future<void> deleteData() async {
  final url = Uri.parse('https://jsonplaceholder.typicode.com/posts/1');
  final response = await http.delete(url);

  if (response.statusCode == 200) {
    print('Xóa thành công');
  } else {
    print('Lỗi: ${response.statusCode}');
  }
}

```
### Xử lý lỗi
- Có thể xảy ra lỗi mạng, server hoặc dữ liệu không hợp lệ.

```dart
Future<void> fetchDataWithErrorHandling() async {
  try {
    final url = Uri.parse('https://jsonplaceholder.typicode.com/posts/1');
    final response = await http.get(url);

    if (response.statusCode == 200) {
      print('Dữ liệu: ${json.decode(response.body)}');
    } else {
      print('Lỗi server: ${response.statusCode}');
    }
  } catch (e) {
    print('Lỗi mạng: $e');
  }
}
```
### Sử dụng với FutureBuilder trong UI
- Khi lấy dữ liệu từ API, ta có thể hiển thị trong UI bằng FutureBuilder.

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

class MyHomePage extends StatelessWidget {
  Future<Map<String, dynamic>> fetchData() async {
    final url = Uri.parse('https://jsonplaceholder.typicode.com/posts/1');
    final response = await http.get(url);
    return json.decode(response.body);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('FutureBuilder Example')),
      body: FutureBuilder<Map<String, dynamic>>(
        future: fetchData(),
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting) {
            return Center(child: CircularProgressIndicator());
          } else if (snapshot.hasError) {
            return Center(child: Text('Lỗi: ${snapshot.error}'));
          } else {
            final data = snapshot.data!;
            return Center(child: Text('Tiêu đề: ${data['title']}'));
          }
        },
      ),
    );
  }
}
```
### Sử dụng http.Client để tối ưu hóa
- Thay vì gọi `http.get()`, ta có thể dùng `http.Client()` để tái sử dụng kết nối, giảm tải cho server.

```dart
Future<void> fetchDataWithClient() async {
  final client = http.Client();
  try {
    final url = Uri.parse('https://jsonplaceholder.typicode.com/posts/1');
    final response = await client.get(url);

    if (response.statusCode == 200) {
      print('Dữ liệu: ${json.decode(response.body)}');
    }
  } finally {
    client.close();
  }
}
```
### Sử dụng http với Bloc
- có thể kết hợp http với Bloc để quản lý dữ liệu từ API.

Ví dụ với Cubit:

```dart
import 'package:flutter_bloc/flutter_bloc.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

class PostCubit extends Cubit<Map<String, dynamic>?> {
  PostCubit() : super(null);

  Future<void> fetchPost() async {
    final url = Uri.parse('https://jsonplaceholder.typicode.com/posts/1');
    final response = await http.get(url);

    if (response.statusCode == 200) {
      emit(json.decode(response.body));
    } else {
      emit(null);
    }
  }
}
```
- Dùng trong UI:

```dart
class PostScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Bloc Example')),
      body: BlocBuilder<PostCubit, Map<String, dynamic>?>(
        builder: (context, post) {
          if (post == null) {
            return Center(
              child: ElevatedButton(
                onPressed: () => context.read<PostCubit>().fetchPost(),
                child: Text('Tải dữ liệu'),
              ),
            );
          } else {
            return Center(child: Text('Tiêu đề: ${post['title']}'));
          }
        },
      ),
    );
  }
}
```

### Kết hợp http với StreamBuilder trong Flutter
- Thư viện http chỉ hỗ trợ Future, không có Stream, nên ta phải tự tạo Stream bằng StreamController

```dart
import 'dart:async';
import 'dart:convert';
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

class StreamHttpExample extends StatefulWidget {
  @override
  _StreamHttpExampleState createState() => _StreamHttpExampleState();
}

class _StreamHttpExampleState extends State<StreamHttpExample> {
  final StreamController<Map<String, dynamic>> _streamController = StreamController();
  
  @override
  void initState() {
    super.initState();
    fetchDataPeriodically();
  }

  /// Gọi API mỗi 5 giây và cập nhật dữ liệu vào Stream
  // api polling
  void fetchDataPeriodically() async {
    Timer.periodic(Duration(seconds: 5), (timer) async {
      try {
        final url = Uri.parse('https://jsonplaceholder.typicode.com/posts/1');
        final response = await http.get(url);

        if (response.statusCode == 200) {
          final data = jsonDecode(response.body);
          _streamController.add(data); // Đẩy dữ liệu mới vào Stream
        } else {
          _streamController.addError('Lỗi API: ${response.statusCode}');
        }
      } catch (e) {
        _streamController.addError('Lỗi kết nối: $e');
      }
    });
  }

  @override
  void dispose() {
    _streamController.close(); // Đóng StreamController khi không dùng nữa
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('HTTP + StreamBuilder')),
      body: StreamBuilder<Map<String, dynamic>>(
        stream: _streamController.stream,
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting) {
            return Center(child: CircularProgressIndicator());
          } else if (snapshot.hasError) {
            return Center(child: Text('Error: ${snapshot.error}'));
          } else if (!snapshot.hasData) {
            return Center(child: Text('Không có dữ liệu'));
          } else {
            final post = snapshot.data!;
            return Padding(
              padding: EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text('Tiêu đề: ${post['title']}', style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold)),
                  SizedBox(height: 10),
                  Text('Nội dung: ${post['body']}'),
                ],
              ),
            );
          }
        },
      ),
    );
  }
}
```