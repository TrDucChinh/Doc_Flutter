- [Start Flutter](#start-flutter)
  - [Flutter là gì, tại sao lại có flutter](#flutter-là-gì-tại-sao-lại-có-flutter)
    - [Thành phần của Flutter](#thành-phần-của-flutter)
    - [Tại sao lại có Flutter](#tại-sao-lại-có-flutter)
  - [Framework và Library](#framework-và-library)
    - [Library](#library)
      - [Ưu điểm](#ưu-điểm)
      - [Nhược điểm](#nhược-điểm)
    - [Framework](#framework)
      - [Ưu điểm](#ưu-điểm-1)
      - [Nhược điểm](#nhược-điểm-1)
    - [Khác nhau giữa Framework và Library](#khác-nhau-giữa-framework-và-library)
  - [So sánh Flutter với React Native](#so-sánh-flutter-với-react-native)
    - [Giống](#giống)
    - [Khác](#khác)
    - [Ưu nhược điểm của Flutter](#ưu-nhược-điểm-của-flutter)
    - [Ưu và nhược điểm của React Native](#ưu-và-nhược-điểm-của-react-native)
  - [Git nâng cao](#git-nâng-cao)
    - [Git merge](#git-merge)
    - [Git rebase](#git-rebase)
    - [Git revert](#git-revert)
    - [Git reset](#git-reset)
      - [Giống và khác nhau giữa revert và reset](#giống-và-khác-nhau-giữa-revert-và-reset)
    - [Git cherry-pick](#git-cherry-pick)
    - [.gitignore](#gitignore)
# Start Flutter
## [Flutter là gì, tại sao lại có flutter](https://www.topcv.vn/flutter-la-gi)
- **Flutter** là một `framework` phát triển ứng dụng di động (mobile) và được phát hành bởi Google.
- Nó cho phép các nhà phát triển xây dựng ứng dụng di động chất lượng, đẹp mắt, nhanh chóng và hiệu quả bằng cách sử dụng một mã nguồn duy nhất. 
- **Flutter** hỗ trợ viết code một lần và triển khai trên nhiều nền tảng, bao gồm cả `Android` và `iOS`.
- **Flutter** sử dụng ngôn ngữ lập trình **Dart**, một ngôn ngữ có hiệu suất cao, đáng tin cậy và dễ học.
- Một trong những đặc điểm nổi bật của **Flutter** là giao diện người dùng được xây dựng bằng cách kết hợp các thành phần UI tùy chỉnh.
- **Flutter** cung cấp một loạt các công cụ, thư viện và hỗ trợ phong phú, giúp nhà phát triển xây dựng các ứng dụng phức tạp và tùy chỉnh một cách dễ dàng.
- Đối với ứng dụng di động, **Flutter** cung cấp **Hot Reload**, cho phép nhà phát triển ngay lập tức thấy được sự thay đổi trong ứng dụng mà không cần phải khởi động lại toàn bộ ứng dụng. Điều này giúp tăng tốc quá trình phát triển và thử nghiệm.

### Thành phần của Flutter
- Gồm 2 thành phần quan trọng:
  - **SDK (Software Development Kit)**: Flutter là một **SDK** - bộ công cụ phát triển phần mềm - chứa các công cụ cần thiết để phát triển ứng dụng di động.
    -  **Flutter SDK** bao gồm môi trường phát triển, trình biên dịch, thư viện, thư viện hỗ trợ và các công cụ khác giúp bạn viết, kiểm tra và triển khai ứng dụng của mình. 
  - **Framework (UI Library based on widgets):** Flutter là một `framework UI (User Interface)` xây dựng dựa trên widget. Widget trong `Flutter` là các thành phần cơ bản để xây dựng giao diện người dùng. Mọi thứ trong `Flutter` đều là widget, từ các thành phần đơn giản như văn bản, nút, hình ảnh đến các thành phần phức tạp hơn như danh sách cuộn và màn hình. 

### Tại sao lại có Flutter
- **Flutter** ra đời để giải quyết một số thách thức phổ biến trong phát triển ứng dụng di động:
  - **Tiết kiệm thời gian và chi phí phát triển:** Với Flutter, các nhà phát triển có thể xây dựng một codebase duy nhất để chạy trên cả Android và iOS, giúp tiết kiệm thời gian và chi phí phát triển.
  - **Hiệu suất cao:** `Flutter` không sử dụng các thành phần giao diện gốc (native components) mà tự dựng giao diện dựa trên công cụ đồ họa Skia của riêng nó. Điều này giúp các ứng dụng `Flutter` chạy nhanh hơn và mượt mà hơn so với các framework khác, chẳng hạn như `React Native`.
  - **Cộng đồng và hỗ trợ lớn:** `Flutter` được phát triển và hỗ trợ bởi Google cùng với một cộng đồng lớn, cung cấp rất nhiều tài liệu, thư viện, và hỗ trợ.
  - **Hot Reload:** `Flutter` cung cấp tính năng` Hot Reload` cho phép các nhà phát triển thấy ngay lập tức các thay đổi trong mã nguồn mà không cần phải khởi động lại ứng dụng, điều này làm tăng hiệu quả phát triển và kiểm thử. 
  - **Giao diện đẹp và tùy biến cao:** `Flutter` cho phép dễ dàng tạo các giao diện tuỳ chỉnh, đẹp mắt nhờ hệ thống widget phong phú và linh hoạt.

## Framework và Library
### Library
- **Library (thư viện)** trong lập trình là một tập hợp các hàm, phương thức hoặc đoạn mã được xây dựng sẵn.
- Nhằm thực hiện các nhiệm vụ cụ thể và có thể được tái sử dụng trong nhiều ứng dụng khác nhau.
- `Thư viện` giúp lập trình viên tiết kiệm thời gian và công sức bằng cách `cung cấp các chức năng chung`, chẳng hạn như xử lý dữ liệu, quản lý giao diện người dùng, hoặc thao tác với tập tin, mà không cần viết lại mã từ đầu.
- Ví dụ: 
  - Glide: 1 thư viện giúp ta tải ảnh về android
  - Room: Giúp chúng ta kết nối với CSDL SQLite 1 cách dễ dàng hơn

#### Ưu điểm
- **Tái sử dụng mã:** Thư viện cung cấp các hàm và phương thức sẵn có, cho phép lập trình viên tái sử dụng mã trong nhiều dự án khác nhau mà không cần viết lại từ đầu, giúp tiết kiệm thời gian và công sức.
- **Dễ dàng sử dụng:** Các thư viện thường được thiết kế với giao diện lập trình dễ hiểu, giúp lập trình viên nhanh chóng tích hợp và sử dụng mà không cần phải tìm hiểu sâu về cách thức hoạt động bên trong.
- **Linh hoạt:** Library cung cấp một tập hợp các chức năng mà lập trình viên có thể sử dụng tùy theo nhu cầu. Bạn có quyền kiểm soát khi nào và ở đâu sử dụng các hàm trong thư viện.
- **Tập trung vào nhiệm vụ cụ thể:** Thư viện thường tập trung vào giải quyết một số vấn đề nhất định, giúp giải quyết nhanh chóng những tác vụ đó.
- **Giảm thiểu lỗi lập trình:** Việc sử dụng các thư viện đã được kiểm thử và sử dụng rộng rãi sẽ giảm nguy cơ mắc phải những lỗi phổ biến trong lập trình, vì thư viện thường được kiểm tra và tối ưu hóa kỹ lưỡng.

#### Nhược điểm
- **Phụ thuộc vào thư viện bên ngoài:** Khi sử dụng nhiều thư viện, dự án của bạn có thể bị phụ thuộc vào các thư viện đó. Nếu thư viện không được duy trì tốt hoặc không còn được cập nhật, nó có thể gây ra các vấn đề bảo mật hoặc không tương thích với công nghệ mới.
- **Thiếu đồng nhất về cấu trúc:** Thư viện không quy định cấu trúc tổng thể của ứng dụng, dẫn đến việc sử dụng quá nhiều thư viện khác nhau có thể làm cho mã nguồn không đồng nhất và khó bảo trì.
- **Hiệu suất:** Một số thư viện có thể chứa nhiều tính năng không cần thiết cho ứng dụng của bạn, làm tăng kích thước tệp và có thể ảnh hưởng đến hiệu suất, đặc biệt là trong các ứng dụng web.
- **Cần học cách sử dụng từng thư viện:** Mặc dù thư viện giúp giảm khối lượng công việc, nhưng mỗi thư viện đều có cú pháp và cách sử dụng riêng. Lập trình viên phải học cách sử dụng hiệu quả từng thư viện.
- **Khó kiểm soát nội dung bên trong:** Vì library thường là mã đóng gói sẵn, bạn không thể dễ dàng điều chỉnh hay thay đổi cách thức hoạt động bên trong nếu thư viện không cung cấp đủ tính năng mà bạn cần.

### Framework
- **Framework** là các đoạn code đã được viết sẵn, cấu thành nên một bộ khung và các thư viện lập trình được đóng gói.
- là một khung làm việc hoặc bộ công cụ giúp lập trình viên phát triển ứng dụng một cách có tổ chức và hiệu quả.
- Khác với thư viện, **framework** không chỉ cung cấp các hàm và phương thức mà còn định nghĩa luồng điều khiển và cấu trúc tổng thể của ứng dụng.
- Khi sử dụng **framework**, lập trình viên phải tuân theo các quy tắc và cấu trúc mà **framework** đề ra.
- Ví dụ: Angular, ReactJs, React Native, Flutter ...

![](https://topdev.vn/blog/wp-content/uploads/2020/11/thanh-phan-framework.png)

#### Ưu điểm
- **Framework** xây dựng sẵn các tính năng chung
- Sử dụng **framework** giảm thiểu tối đa thời gian và công sức phát triển ứng dụng.
- Cho phép ứng dụng kế thừa một cấu trúc được chuẩn hóa, đảm bảo dễ dàng trong vận hành và bảo trì sau này.
- Tăng năng suất
- Bảo mật cao
- Tính nhất quán
- Hỗ trợ cộng đồng và tài liệu phong phú
- Hiệu suất tối ưu và khả năng mở rộng
#### Nhược điểm
- Mất nhiều thời gian để học và làm chủ một **framework**.
- Kích cỡ ứng dụng khi sử dụng **framework** là rất lớn
- Cần viết code tuân thủ theo các tiêu chuẩn chung mà **framework** đặt ra.
- Không thích hợp với các ứng dụng quá nhỏ

### Khác nhau giữa Framework và Library
- **Framework** và **Library** đều cung cấp các tính năng (functions) được viết sẵn để chúng ta có thể tái sử dụng.
- **Framework** lớn hơn và phức tạp hơn **Library**.
- Sử dụng **Framework** phải thay đổi cấu trúc code của dự án (project’s structure) theo các quy tắc của framework đó để có thể sử dụng được các functions mà framework đó cung cấp.
- Có thể sử dụng các functions của **Library** một cách trực tiếp mà không cần thay đổi cấu trúc code của dự án.
- **Framework** có thể hiểu là một khung chương trình, người dùng bổ sung code và tuân theo quy tắc để tạo ra ứng dụng. Còn **Library** chỉ cung cấp các chức năng tiện ích hay các class để sử dụng trong quá trình xây dựng ứng dụng.
- **Framework** hoạt động chủ động. Nghĩa là nó có thể đưa ra các quyết định gọi hoặc bị gọi bởi các **Library** hay ứng dụng nào đó. Library hoạt động bị động. Nghĩa là nó chỉ được gọi khi nào chúng ta cần dùng nó.

## So sánh Flutter với React Native
### Giống
- **Đa nền tảng:** Cả hai đều cho phép phát triển ứng dụng di động cho iOS và Android từ một codebase duy nhất.
- **Hot Reload:** Cả Flutter và React Native đều hỗ trợ tính năng Hot Reload, giúp lập trình viên thấy ngay lập tức các thay đổi trong mã nguồn mà không cần khởi động lại ứng dụng.
- **Mã nguồn mở:** Flutter và React Native đều là mã nguồn mở, nhận được hỗ trợ từ cộng đồng lớn, và có tài liệu phong phú.
- **Hiệu suất cao:** Cả hai framework đều cho phép tạo ra các ứng dụng với hiệu suất cao, đáp ứng được yêu cầu về trải nghiệm mượt mà của người dùng.

### Khác

|Tiêu chí	|Flutter	|React Native|
|---|---|---|
|Ngôn ngữ	|Sử dụng Dart	|Sử dụng JavaScript|
|Công ty phát triển	|Google	|Facebook (hiện là Meta)|
|Giao diện UI	|Sử dụng widget riêng để xây dựng giao diện	|Sử dụng các thành phần native của hệ điều hành
|Khả năng tuỳ biến giao diện|	Rất cao, có thể tùy chỉnh từng chi tiết UI	|Tốt, nhưng phụ thuộc vào component của hệ điều hành|
|Hiệu suất đồ họa|	Cao hơn nhờ dùng công cụ đồ họa Skia	|Phụ thuộc vào Native Components của iOS/Android|
|Tài liệu và hỗ trợ	|Rất phong phú, cộng đồng đang phát triển mạnh	|Cộng đồng lớn và đã phát triển từ lâu|
|Quy trình phát triển	|Chuyên biệt hơn, cần học thêm Dart	|Thân thiện với developer JavaScript|

### Ưu nhược điểm của Flutter
**Ưu điểm:**
- **Hiệu suất cao:** `Flutter` hoạt động mượt mà nhờ sử dụng công cụ đồ họa Skia, không cần đến các thành phần giao diện gốc của hệ điều hành.
- **UI tùy chỉnh mạnh mẽ:** Với hệ thống widget phong phú, `Flutter` hỗ trợ tạo giao diện đẹp và tinh tế, dễ dàng tùy biến.
- **Tương thích nền tảng:** `Flutter` hoạt động giống nhau trên cả Android và iOS, giúp giữ được tính đồng nhất của giao diện trên các nền tảng.

**Nhược điểm:**
- **Cộng đồng và hệ sinh thái chưa phát triển hoàn thiện:** `Flutter` vẫn còn mới so với `React Native`, do đó, một số thư viện và tài liệu hỗ trợ còn hạn chế.
- **Dart:** Sử dụng ngôn ngữ `Dart` khiến nhiều lập trình viên phải học ngôn ngữ mới, điều này có thể là rào cản với một số người.

### Ưu và nhược điểm của React Native
**Ưu điểm:**
- **Cộng đồng lớn:** `React Native` có cộng đồng người dùng lớn với nhiều thư viện và tài liệu hỗ trợ phát triển sẵn có.
- **Hỗ trợ JavaScript**: `JavaScript` là ngôn ngữ phổ biến, dễ học và sử dụng, nên nhiều lập trình viên cảm thấy dễ dàng tiếp cận.
- **Native Components:** Sử dụng thành phần giao diện gốc của hệ điều hành giúp các ứng dụng `React Native` có trải nghiệm gần giống ứng dụng native.

**Nhược điểm:**
- **Vấn đề về hiệu suất:** Do phải chuyển đổi giữa `JavaScript` và các thành phần native, đôi khi `React Native` không mượt mà như `Flutter`.
- **Giới hạn UI tùy chỉnh:** Dù có thể tùy chỉnh giao diện, `React Native` vẫn phụ thuộc vào các thành phần của hệ điều hành, khiến việc tạo các giao diện phức tạp có thể gặp hạn chế.

## Git nâng cao
### Git merge
**Git merge**: dùng để kết hợp mã nguồn từ nhánh này sang nhánh khác, thường là từ một nhánh tính năng `(feature branch)` về nhánh chính `(main hoặc master)`. Có hai loại merge phổ biến:
- `Fast-forward merge:` Khi nhánh đích không có commit mới kể từ khi tách nhánh, Git sẽ "tiến thẳng" đến commit cuối cùng của nhánh được merge.
- `Recursive merge:` Khi cả hai nhánh đều có commit riêng, Git sẽ tạo ra một commit merge mới để kết hợp mã nguồn từ cả hai nhánh. Nếu có thay đổi ở cùng một file, xung đột (conflict) sẽ xảy ra và cần phải giải quyết thủ công.
```git
git checkout [branch]
git merge [feature-branch]
```
### Git rebase
**git rebase**: dùng để gộp commit
- Có thể gộp commit từ các nhánh khác nhau:
```git
git checkout [feature-branch]
git rebase [main]
```
- Gộp các commit trong cùng 1 nhánh: `git rebase -i [commit]`
- Lưu ý trên CLI: phím I để bật / tắt chế độ insert (cho phép thay đổi file)
  - Để lưu và đóng file: thoát chế độ insert, nhập `:wq` để lưu và đóng file hiện tại.
  - Đầu tiên dùng lệnh `git log --oneline` để xem lịch sử commit.
  - Sau khi xác định được cần gộp những commit nào, dùng lệnh `git rebase -i [commit]`.
  - Hiển thị ra màn hình các commit được lựa chọn, giữ lại 1 cái (để nguyên chữ `pick` / hoặc để chữ `r`).
  - Các commit khác để chữ `s`.
  - Sau đó thoát chế độ insert và lưu, đóng file.
  - Sửa đổi, để lại 1 message, các dòng khác bỏ đi bằng cách thêm dấu # đằng trước.
  - Sau đó thoát chế độ insert và lưu, đóng file.
  - Resolve conflict nếu có, sau khi resolve các conflict thì sẽ tiếp tục `git add .` rồi git `rebase --continue.`
  - Sau khi đã resolve xong, `git push origin --force` để thay đổi remote.

### Git revert
- Tạo ra một commit mới để đảo ngược các thay đổi của một commit cụ thể mà không thay đổi lịch sử commit. Điều này giúp giữ lại toàn bộ lịch sử của dự án, bao gồm cả những thay đổi đã bị hủy.
`git revert <commit-id>`

### Git reset
- `git reset` di chuyển con trỏ `HEAD` của bạn đến một commit cụ thể và có thể thay đổi trạng thái của staging area (khu vực tạm) và thư mục làm việc tùy vào cách sử dụng `--soft, --mixed, hoặc --hard`.
  - `--soft:` Di chuyển HEAD về commit cụ thể, nhưng giữ lại thay đổi trong staging area.
  - `--mixed:` Di chuyển HEAD về commit cụ thể và bỏ thay đổi khỏi staging area nhưng vẫn giữ lại trong thư mục làm việc.
  - `--hard:` Di chuyển HEAD về commit cụ thể và loại bỏ toàn bộ thay đổi khỏi cả staging area và thư mục làm việc.

#### Giống và khác nhau giữa revert và reset
**Cách hoạt động**
- **git revert:** Tạo một commit mới để "đảo ngược" (revert) các thay đổi của một commit cụ thể. revert không thay đổi lịch sử commit mà chỉ thêm một commit mới nhằm hoàn tác thay đổi của commit cũ. Do đó, tất cả các commit vẫn sẽ được giữ lại trong lịch sử.
- **git reset:** Di chuyển nhánh hiện tại đến một commit khác và có thể xóa các commit hoặc thay đổi. Lệnh reset thay đổi lịch sử commit và có thể xóa các commit khỏi lịch sử (đặc biệt là khi dùng `--hard`).
**Ảnh hưởng đến lịch sử commit**
- `git revert:` Lịch sử commit sẽ giữ lại tất cả các commit ban đầu và thêm vào một commit mới để đảo ngược thay đổi. Điều này giúp cho lịch sử Git rõ ràng và không gây xung đột khi làm việc nhóm.
- `git reset:` Có thể thay đổi hoàn toàn lịch sử của nhánh hiện tại, đặc biệt là với `--hard`. Điều này sẽ xóa các commit khỏi lịch sử, làm cho chúng không thể truy cập lại (trừ khi bạn đã có backup hoặc dùng reflog).
**Sử dụng khi**
- **git revert**:Thường được dùng khi bạn muốn hủy một thay đổi đã commit nhưng vẫn muốn giữ lịch sử nguyên vẹn.
  - Phù hợp cho các nhánh đã được đẩy lên remote, vì nó không làm thay đổi lịch sử của nhánh.
- **git reset:** Phù hợp khi bạn đang làm việc cục bộ và muốn thay đổi hoặc xóa commit mà không cần giữ lại lịch sử.
  - Thường sử dụng để quay lại trạng thái trước đó và làm sạch các thay đổi chưa muốn giữ.

### Git cherry-pick
- cho phép chọn các commit cụ thể từ một nhánh và áp dụng vào nhánh hiện tại mà không cần merge toàn bộ.
```git
git checkout main
git cherry-pick <commit-id>
```
- `cherry-pick` hữu ích khi bạn chỉ muốn áp dụng một hoặc vài thay đổi từ một nhánh khác mà không cần merge cả nhánh đó.

### .gitignore
- `.gitignore` là file cấu hình giúp bỏ qua các file hoặc thư mục không cần thiết trong kho lưu trữ (như các file tạm, file cấu hình cá nhân, hoặc thư mục của IDE).
```git
# Bỏ qua thư mục build
/build/

# Bỏ qua file tạm của hệ điều hành
*.log
*.tmp

# Bỏ qua các file nhị phân của IDE
*.swp
*.DS_Store
```
- `.gitignore` chỉ có tác dụng với các file chưa được thêm vào Git. Nếu file đã được commit, bạn cần phải xóa file khỏi index của Git trước khi .gitignore có hiệu lực.
- Cách dùng:
  - Dấu `#` để comment.
  - Ignore 1 file: `tên_file.txt`
  - Ignore cả thư mục: `example_folder/`
    - Khi ignore thư mục nên có dấu / ở sau tên thư mục để nhận biết đó là thư mục, nếu không `.gitignore` có thể là coi đó là file hay symbolic link.
  - Sử dụng `*` để biểu diễn dạng của file muốn ignore:
    - Ví Dụ: ignore những file nào có đuôi là `.abc` trong 1 folder: `folder/*.abc`
  - Sử dụng `**` để mang ý nghĩa toàn bộ. 
    - Ví dụ: `**/abc` nó sẽ có hiệu lực cho tất cả file hoặc thư mục có tên là `abc` ở mọi nơi trong project.
    - Ví dụ:  `folder/**` để có hiệu lực cho tất cả các file bên trong thư mục `folder`.
- **Xử lý khi lỡ push file đáng ra cần .gitignore lên trên remote:**
  - Cần xóa file đó khỏi `git cache` để `.gitignore `có thể nhận biết được file đó.
  ```bash
  # remove 1 file cụ thể
  git rm -r --cached path/to/file 
  ```
  - Hoặc
  ```bash
  # remove toàn bộ cache sau đó add lại toàn bộ để .gitignore nhận biết được file cần ignore
  git rm -r --cached
  git add .
  ```
  - Sau đó tạo một commit mới và push lên remote, thực hiện gộp commit để không bị thừa commit, và cũng để người khác ko xem lại được commit sai đó.
  ```bash
  git commit -m 'Message ví dụ: File cần ignore đã được xóa'
  git push origin tên_branch
  ``` 