### 1. Gitignore là gì? (WHAT)

- Gitignore là file có tên là .gitignore do Git quy định. Nhiệm vụ của nó là liệt kê những file mà mình không mong muốn cho vào git

### 6. Khi nào lên dùng (WHERE, WHEN)

- Bất cứ project nào cũng nên dùng nó, bạn nên tạo ngay file .gitignore trong thư mục gốc ngay khi **khởi tạo project** của bạn và liệt kê luôn những file mà bạn muốn git bỏ qua. Tại sao phải liệt kê trước làm gì thế? Đọc phần phần [`Cache`](#git-cache) ?

### 2. Cách thức hoạt động: (WHY??? (or/and How?))

- Có thể hiểu đơn giản là git sẽ bỏ qua file hoặc một tập các file trong project của chúng ta khi commit và push lên repository. Ví dụ:

  - Các file mà IDE tự sinh ra trong quá trình build project (VD: files trong folder `build`) -> Tránh tốn kém tài nguyên server lưu trữ project.
  - Các file cấu hình các biến môi trường (VD: file `.env`) -> Gây ra việc không build được project khi checkout về ở các máy thành viên khác.
  - …

- Git quản lý các file mà chúng ta muốn "ignore" bằng file `.gitignore` được đặt ở trong thư mục root project.

- Khi add 1 file mới vào git, git sẽ kiểm tra danh sách những file sẽ bỏ qua trong file `.gitignore` và không add chúng vào git. Đó mới chỉ là điều kiện cần, điều kiện đủ là files không có trong git cache nữa thì git nó mới bỏ qua, chứ files mà nằm trong git cache thì .gitignore sẽ vô tác dụng.

### 3. Các pattern format: (HOW)

- Sử dụng dấu `#` để comment và có thể để cách dòng cho dễ đọc.
- Đơn giản nhất là tên file cần ignore (VD: `example.exe`), hay cả thư mục (VD: `example_folder/`)
- Khi ignore thư mục nên có dấu `/` ở sau tên thư mục để nhận biết đó là thư mục, nếu không nó có thể là coi là thư mục hoặc file hay symbolic link.
- Dấu ! phía trước có ý nghĩa phủ định (VD: `!abc/example.exe`)
- Sử dụng 1 dấu `*` để tìm các file có cùng định dạng. Ví dụ như bạn muốn ignore tất cả các file .xml trong project (VD: `*.xml`)
- Trường hợp khác của 1 dấu `*` nếu bạn chỉ rõ đường dẫn (VD: `config/*.xml`) thì nó chỉ có hiệu lực cho các file `config/abc.xml` mà không có hiệu lực cho các file `config/sub/abc.xml`.
- Sử dụng `**` để có hiệu lực cho các thư mục không cần định rõ tên. Ví dụ: `**/foo` nó sẽ có hiệu lực cho tất cả file hoặc thư mục có tên là foo ở mọi nơi trong project.
- Hay sử dụng kiểu folder`/**` để có hiệu lực cho tất cả các file bên trong thư mục.

### 4. Tools

- Hầu hết các IDE đều hỗ trợ, nếu chưa có thì có thể cài đặt thêm plugin hay config ở đâu đó.

- Hoặc đơn giản có thể vào [gitignore.io](https://www.toptal.com/developers/gitignore/) sau đó chọn loại project mình đang làm.

### 5. Phạm vi phủ sóng

- File `.gitignore` sẽ ảnh hưởng đến các file và thư mục anh em với nó hoặc là con cháu, chắt của nó. Thường thì project chỉ cần 1 file `.gitignore` ở ngoài cùng là đủ nhưng nếu project quá lớn ta có thể tách file `.gitignore` vào từng folder nhỏ để dễ quản lý.

### <div id="git-cache"> 7. Git cache </div>

Giả dụ thế này! Bạn vừa join vào project và thấy project liên tục bị conflict vì mấy file rác. Nhưng may quá bạn đọc được bài viết này và bạn rất thông minh nên đã tạo luôn file `.gitignore` cho project và thêm luôn file rác đó vào `.gitignore` rồi bạn xóa file rác đi và commit lên.

Rồi sao! 1 ông khác lại pull code mới về lại tạo ra file rác đó và nó vẫn dính vào git bình thường. Đờ heo? “Em cho nó vào .gitignore rồi cơ mà?.

Vì sao à? Vì file đó đã được thằng git cache thu nạp thành của nó rồi nên thằng git nó vẫn có quyền quản lý file đó. Vậy cách giải quyết đơn giản nó phải giải thoát file đó ra khỏi git cache là xong, bằng 1 dòng lệnh:

```
git rm -r --cached /path/to/file_or_folder
```

Từ bây giờ file đó không còn là của git cache nữa nên nó không thuộc quyền quản lý của git nữa và bây giờ `.gitignore` mới có tác dụng. Theo lý thuyết là vậy nhưng nếu bạn cần reset lại hết project để `.gitignore` hoạt động đúng thì mình thường xóa bỏ hết file của git cache luôn?

```
git rm -r --cached .
```

Sau đó mình sẽ add tất cả các file lại vào project như lúc mới tạo project.

```
git add
```

Và bây giờ bạn lại commit và push như bình thường.
