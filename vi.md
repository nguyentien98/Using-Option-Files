## 4.2.6 Sử dụng những file tùy chọn

Hầu hết các chương trình MySQL có thể đọc các tùy chọn khởi động từ các file tùy chọn (đôi khi được gọi là file cấu hình). Các file tùy chọn cung cấp một cách thuận tiện để chỉ định các tùy chọn được sử dụng để chúng không được nhập vào dòng lệnh mỗi khi bạn chạy một chương trình. 


Để xác định một đọc các file tùy chọn hay không, hãy gọi nó bằng tùy chọn `--help`. (Đối với **mysqld** , sử dụng `--verbose` và `--help`.) Nếu chương trình đọc các file tùy chọn, tin nhắn trợ giúp cho biết file nào nó tìm và nhóm tùy chọn nào nó nhận ra.

Ghi chú

Một chương trình MySQL đã bắt đầu với tùy chọn `--no-defaults`  sẽ chỉ đọc cấu hình từ file `.mylogin.cnf`.


Nhiều file tùy chọn là những file văn bản thuần, được tạo bằng bất cứ trình soạn thảo nào. Ngoại lệ là file `.mylogin.cnf` nó chứa tùy chọn đường dẫn đăng nhập. Đây là một file mã hóa được tạo bởi tiện ích [**mysql\_config\_editor**](mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility"). Nhìn [Mục 4.6.6, “**mysql\_config\_editor** — Tiện ích cấu hình MySQL”](mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility"). Một 'đường dẫn đăng nhập' là một nhóm tùy chọn chỉ cho phép trong những tùy chọn chắc chắn sau đây: `host`, `user`, `password`, `port` và `socket`. Chương trình client chỉ định đường dẫn đăng nhập để đọc từ `.mylogin.cnf` sử dụng tùy chọn [`--login-path`](option-file-options.html#option_general_login-path).

Để chỉ đinh một sự thay thế tên file đường dẫn đăng nhập, hãy đặt biến môi trường `MYSQL_TEST_LOGIN_FILE`. Biến này được sử dụng bởi tiện ích kiểm tra **mysql-test-run.pl**, nhưng cũng được công nhận bởi [**mysql\_config\_editor**](mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility") và bởi những MySQL client ví dụ như [**mysql**](mysql.html "4.5.1 mysql — The MySQL Command-Line Tool"), [**mysqladmin**](mysqladmin.html "4.5.2 mysqladmin — Client for Administering a MySQL Server"), và vân vân.

MySQL tìm kiếm những file tùy chọn trong mô tả theo thứ tự tại phần thảo luận sau và đọc bất cứ cái nào tồn tại. Nếu một file tùy chọn bạn muốn sử dụng không tồn tại, tạo nó bằng phương thức thích hợp, như vừa thảo luận.

Ghi chú

Những file tùy chọn sử dụng với chương trình NDB Cluster được trình bày trong [Phần 21.3, Cấu hình của NDB Cluster”](mysql-cluster-configuration.html "21.3 Configuration of NDB Cluster").

Trên Windows, chương trình MySQL đọc những tùy chọn khởi động từ những file được hiển thị tại bảng dưới đây, theo thứ tự cụ thể (những file liệt kê trước đọc trước, các file theo sau được đọc lần lượt).

**Bảng những file tùy chọn 4.1 trong hệ thống Windows**

| Tên file | Mục đích |
|---|---|
| ```%PROGRAMDATA%\MySQL\MySQL Server 5.7\my.ini```,  ```%PROGRAMDATA%\MySQL\MySQL Server 5.7\my.cnf``` | Tùy chọn chung |
| `` %WINDIR%\my.ini``, `` %WINDIR%\my.cnf`` | Tùy chọn chung |
| `C:\my.ini`, `C:\my.cnf` | Tùy chọn chung |
| ``_BASEDIR_\my.ini``, ``_BASEDIR_\my.cnf`` | Tùy chọn chung |
| `defaults-extra-file` | File quy định với [`--defaults-extra-file`](option-file-options.html#option_general_defaults-extra-file), nếu có |
| ` %APPDATA%\MySQL\.mylogin.cnf` |  Tùy chọn đường dẫn đăng nhập (chỉ khách) |


Trong bảng trên, `%PROGRAMDATA%` đại diện cho thư mục file hệ thống chứa dữ liệu ứng dụng cho tất cả người dùng trên máy chủ. Đường dẫn này mặc định tại `C:\ProgramData` trên Microsoft  Windows Vista trở lên `C:\Documents` và `Settings\All Users\Application Data` trên các phiên bản cũ hơn của Microsoft  Windows.



`%WINDIR%` đại diện vị trí thư mục Windows của bạn. Nó thường là `C:\WINDOWS`. Sử dụng lệnh sau để xác định vị trí chính xác từ giá trị của biến môi trường `WINDIR`.

`C:\> echo %WINDIR%`



`%APPDATA%` đại diện giá trị của thư mục dữ liệu ứng dụng Windows. Dử dụng lệnh sau để xác định vị trí chính xác từ giá trị của biến môi trường `APPDATA`.

`C:\> echo %APPDATA%`


_`BASEDIR`_ đại diện cho thư mục cài đặt nền tảng. Khi MySQL 5.7 được cài đặt bằng MySQL Installer, nó thường là``C:\_`PROGRAMDIR`_\MySQL\MySQL 5.7 Server`` tại _`PROGRAMDIR`_ đại diện cho thư mục chương trình (thường `Program Files` trong bản Windows tiếng Anh), Xem tại [Phần 2.3.3, “MySQL Installer cho Windows”](mysql-installer.html "2.3.3 MySQL Installer for Windows").

Trên Unix và hệ thống giống Unix, chương trình MySQL đọc file tùy chọn khởi động từ những file được ghi trong bảng dưới đây, theo thứ tự cụ thể (những file liệt kê trước đọc trước, các file theo sau được đọc lần lượt);

Ghi chú

Trên nền tảng Unix, MySQL loại trừ những file cấu hình mà nó có thể ghi bởi bất cứ ai. Đây là chủ đích như một biện pháp an ninh.

**Bảng 4.2 những file tùy chọn được đọc trên các hệ thống giống Unix và giống Unix**

| Tên file | Mục đích |
|---|---|
| `/etc/my.cnf` | Tùy chọn chung |
| `/etc/mysql/my.cnf` | Tùy chọn chung |
| ``_`SYSCONFDIR`_/my.cnf`` | Tùy chọn chung |
| `$MYSQL_HOME/my.cnf` | Tùy chọn máy chủ cụ thể (chỉ dành cho máy chủ) |
| `defaults-extra-file` | Tệp được chỉ định [`--defaults-extra-file`](option-file-options.html#option_general_defaults-extra-file), nếu có |
| `~/.my.cnf` | Tùy chọn người dùng cụ thể |
| `~/.mylogin.cnf` | Tùy chọn đường dẫn đăng nhập người dùng cụ thể (chỉ khách) |

  

Trong bảng trên, `~` đại diện cho thư mục chính của người dùng hiện tại (giá trị của )`$HOME`

_`SYSCONFDIR`_ đại diện cho thư mục đã chỉ định với tùy chọn [`SYSCONFDIR`](source-configuration-options.html#option_cmake_sysconfdir) cho **CMake** khi MySQL được xây dựng. Theo mặc đinh, đây là thư mục `etc` nằm dưới thư mục cài đặt đã compile.

`MYSQL_HOME` là một biến môi trường chứa đường dẫn đến thư mục trong file `my.cnf` của máy chủ cụ thể. Nếu `MYSQL_HOME` chưa được cài và bạn khởi động máy chủ bằng chương trình [**mysqld_safe**](mysqld-safe.html "4.3.2 mysqld_safe — MySQL Server Startup Script"), [**mysqld_safe**](mysqld-safe.html "4.3.2 mysqld_safe — MySQL Server Startup Script") sẽ đặt nó vào _`BASEDIR`_, thư mục cài đặt nền tảng MySQL.

_`DATADIR`_ thường là `/usr/local/mysql/data`, mặc dù nó có thể thay đổi trên nền tảng hoặc phương thức cài đặt. Giá trị là nơi thư mục dữ liệu đã được xây dựng khi MySQL được compile, không phải nơi được chỉ định với tùy chọn [`--datadir`](server-options.html#option_mysqld_datadir) khi [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server") khởi động. Sử dụng [`--datadir`](server-options.html#option_mysqld_datadir) trong lúc chạy không ảnh hướng đến nơi mà máy chủ tìm kiếm những file tùy chọn mà nó đọc trước khi thực thi bất cứ tùy chọn nào.

Nếu nhiều trường hợp của một tùy chọn được tìm thấy, trường hợp cuối cùng sẽ được ưu tiên với một ngoại lệ: Với [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server"), trường hợp đầu tiên của tùy chọn [`--user`](server-options.html#option_mysqld_user) được sử dụng giống như một biện pháp an ninh, để ngặn chặn một người dùng cụ thể trong file tùy chọn từ sự ghi đè trong dòng lệnh.

Phần mô tả dươi đây của cú pháp file tùy chọn áp dụng cho những file mà bạn chỉnh sửa thủ công. Điều này không bao gồm `.mylogin.cnf`, nó được tại ra bằng [**mysql\_config\_editor**](mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility") và được mã hóa.

Bất cứ tùy chọn dài nào có thể đưa ra trong dòng lệnh khi chạy một chương trình MySQL có thể đưa ra trong một file tùy chọn. Để lấy một danh sách tùy chọn khả dụng cho một chương trình, chạy nó với tùy chọn `--help`. (Với [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server"), sử dụng [`--verbose`](server-options.html#option_mysqld_verbose) và [`--help`](server-options.html#option_mysqld_help).)

Cứ pháp cho những tùy chọn chỉ định trong một file tùy chọn là giống nhau trên dòng lệnh (xem [Phần 4.2.4, “Sử dụng tùy chọn trên Command Line”](command-line-options.html "4.2.4 Using Options on the Command Line")). Tuy nhiên trong một file tùy chọn, bạn bỏ qua hai gạch đầu dòng từ tên tùy chọn và bạn chỉ định duy nhất một tùy chọn một dòng. Ví dụ, `--quick` và `--host=localhost` trên dòng lệnh bạn nên chỉ định như `quick` và `host=localhost` trên những dòng riêng biệt trên một file tùy chọn. Để chỉ định một tùy chọn của biểu mẫu ``--loose-_`opt_name`_`` trong một file tùy chọn, hãy viết nó như sau ``loose-_`opt_name`_``.

Những dòng trống trong những file tùy chọn bị bỏ bỏ. Những dòng còn lại có thể như bất cứ dạng sau:

*   ``#_`comment`_``, ``;_`comment`_``
    
    Dòng comment bắt đầu với `#` hoặc `;`. Một `#` comment có thể bắt đầu ở giữa dòng vẫn được.
    
*   ``[_`group`_]``
    
    _`group`_ là tên của chương trình hoặc nhóm cho cái bạn muốn đặt tùy chọn. Sau một nhóm dòng, bất cứ dòng cài đặt tùy chọn nào đều áp dụng cho đặt tên nhóm cho đến cuối cùng của file tùy chọn hoặc một nhóm dòng khác được đưa ra. Tên nhóm tùy chọn không ảnh hưởng bởi chữ viết hoa hay viết thường..
    
*   ``_`opt_name`_``
    
    Nó tương đương với ``--_`opt_name`_`` trên dòng lệnh.
    
*   ``_`opt_name`_=_`value`_``
        
    Nó tương đương với ``--_`opt_name`_=_`value`_`` trên dòng lệnh. Trong một file tùy chọn, bạn có thể dùng dấu cách trước và sau ký tự `=`, một số không đúng trên dòng lệnh. Giá trị tùy chọn có thể được đi kèm với một hoặc hai dấu ngoặc kép, nó hữu dụng nếu giá trị chứa một ký tự comment `#`. 
    

Những dấu cách ở đầu và cuối được tự động xóa khởi tên và giá trị tùy chọn.

Bạn có thể sử dụng ký tự `\b`, `\t`, `\n`, `\r`, `\\`, và `\s` trong những giá trị tùy chọn để đại diện cho backspace, tab, dòng mới, carriage return, backslash, và dấu cách. Trong những file tùy chọn, đây là những quy tắc được áp dụng:

*   Một dấu backslash theo sau bởi một ký tự escape sequence hợp lệ được chuyển thành ký tự đại diện bởi trình tự. Ví dụ, `\s` được chuyển thành một dấu cách.
    
*   Một dấu backslash theo sau bởi một ký tự escape sequence không hợp lệ vẫn không thay đổi. Ví dụ, `\S` được giữ nguyên. 
    

Những quy tắc trên có nghĩa là một dấu backslash có thể đưa ra giống như `\\`, hoặc `\` nếu nó không được đứng sau bởi một ký tự escape sequence hợp lệ.

Quy tắc cho escape sequences trong những file tùy chọn khác nhau một chút so với những quy tắc cho escape sequences trong chuỗi chữ viết tại câu lệnh SQL. Trong bối cảnh sau này, nếu “_`x`_” không phải một ký tự escape sequence hợp lệ, ``\_x_`` trở thành “_`x`_” thay vì ``\_x_``. Xem tại [Phần 9.1.1, “Chuỗi chữ viết”](string-literals.html "9.1.1 String Literals").

Quy tắc escaping dành cho giá trị file tùy chọn đặc biệt thích hợp cho tên đường dẫn Windows, nó dùng `\` giống như một phân cách đường dẫn. Một phân cách trong tên đường dẫn Windows phải được viết là `\\` nếu nó không theo sau bởi một ký tự escape sequence. Nó có thể được viết là `\\` hoặc `\` nếu nó không phải. Bổ sung, `/` có thể được sử dụng trong tên đường dẫn Windows và sẽ được thay giống như `\`. Giả sử đó là điều bạn muốn để chỉ định một thư muc nền tảng của `C:\Program Files\MySQL\MySQL Server 5.7` trong một file tùy chọn. Nó có thể được hoàn thành theo một số cách. Ví dụ:

 

`basedir="C:\Program Files\MySQL\MySQL Server 5.7"
basedir="C:\\Program Files\\MySQL\\MySQL Server 5.7"
basedir="C:/Program Files/MySQL/MySQL Server 5.7"
basedir=C:\\Program\sFiles\\MySQL\\MySQL\sServer\s5.7`


Nếu một tên nhóm tùy chọn giống với tên một chương trình, tùy chọn trong nhóm áp dụng đặc biệt đến chương trình đó. Ví dụ, nhóm `[mysqld]` và `[mysql]` áp dụng cho server [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server") và [**mysql**](mysql.html "4.5.1 mysql — The MySQL Command-Line Tool") chương trình khách, tương ứng.

Nhóm tùy chọn `[client]` được đọc bởi tất cả chương trình máy client được cung cấp trong các phiên bản MySQL (nhưng _không phải_ bởi [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server")). Để hiểu cách các chương trình khách của bên thứ ba sử dụng API C có thể dùng file tùy chọn, xem tài liệu về API C tại [Mục 27.8.7.50, “mysql_options()”](mysql-options.html "27.8.7.50 mysql_options()").

Nhóm `[client]` cho phép bạn chỉ định tùy chọn để áp dụng cho tất cả máy khách. Ví dụ, `[client]` là nhóm thích hợp để sử dụng để chỉ định mật khẩu để kết nối với máy chủ. (Nhưng hãy chắc chắn rằng file tùy chọn chỉ có thể được truy bởi bạn, vì vậy người khác không thể tìm ra mật khẩu của bạn). Chắc chắn không đặt một tùy chọn trong nhóm `[client]` trừ khi nó được sử dụng bởi _tất cả_ chương trình khách mà bạn sử dụng. Những chương trình đó không hiểu tùy chọn thoát sau khi hiển thị một thông báo lỗi nếu bạn thử chạy chúng.

Liệt kê nhiều hơn các nhóm tùy chọn chung hơn trước và các nhóm cụ thể hơn sau. Ví dụ, một nhóm `[client]` chung hơn bởi ví nó được đọc bởi tất cả chương trình khách, trong khi nhóm `[mysqldump]` chỉ được đọc bởi [**mysqldump**](mysqldump.html "4.5.4 mysqldump — A Database Backup Program"). Tùy chọn được chỉ định sau sẽ ghi đè những tùy chọn được chỉ định sớm hơn, vì vậy đặt nó trong những nhóm tùy chọn theo thứ tự `[client]`, `[mysqldump]` cho phép tùy chọn [**mysqldump**](mysqldump.html "4.5.4 mysqldump — A Database Backup Program") ghi đè tùy chọn `[client]`.

Đây là file tùy chọn chung điển hình:
 

```
[client]
port=3306
socket=/tmp/mysql.sock

[mysqld]
port=3306
socket=/tmp/mysql.sock
key_buffer_size=16M
max_allowed_packet=8M

[mysqldump]
quick
```

Đây là file tùy chọn người dùng điển hình:


```
[client]
# The following password will be sent to all standard MySQL clients
password="my password"

[mysql]
no-auto-rehash
connect_timeout=2
```

Để tạo các nhóm tùy chọn chỉ đọc bởi các máy chủ [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server") từ những phiên bản MySQL phát hành cụ thể, hãy sử dụng các nhóm với tên `[mysqld-5.6]`, `[mysqld-5.7]`, vân vân. Nhóm sau chỉ ra rằng cài đặt [`sql_mode`](server-system-variables.html#sysvar_sql_mode) nên được sử dụng bởi máy chủ MySQL phiên bản 5.7.x.
 

`[mysqld-5.7]
sql_mode=TRADITIONAL`



Có thể sử dụng các chỉ thị `!include` trong những file tùy chọn để include file tùy chọn khác và `!includedir` để tìm kiếm thư mục và file tùy chọn cụ thể. Ví dụ, để include file `/home/mydir/myopt.cnf` sử dụng chỉ thị sau:

 

`!include /home/mydir/myopt.cnf`

Để tìm thư mục `/home/mydir` và đọc những file tùy chọn được tìm thấy, sử dụng chỉ thị sau:

 

`!includedir /home/mydir`

MySQL không đảm bảo về thứ tự các tập tin tùy chọn trong thư mục sẽ được đọc.


Ghi chú

Bất cứ file được tìm thấy và include bằng `!includedir` trong hệ điều hành _phải_ có tên file kết thúc trong `.cnf`. Trên Windows, chỉ thị này kiểm tra những file với đuôi `.ini` hoặc `.cnf`.

Viết nội dung của tệp tùy chọn được include giống như bất kỳ tệp tùy chọn nào khác. Tức là, nó phải chứa các nhóm tùy chọn, mỗi nhóm đứng trước một dòng ``[_`group`_] `` chỉ ra chương trình mà các tùy chọn áp dụng.

Trong khi tệp được include đang được xử lý, chỉ những tùy chọn trong nhóm mà chương trình hiện tại đang tìm kiếm được sử dụng. Các nhóm khác bị bỏ qua. Giả sử rằng tệp `my.cnf` chứa dòng này:

 

`!include /home/mydir/myopt.cnf`

Và giả sử rằng `/home/mydir/myopt.cnf` giống như sau:

 

```
[mysqladmin]
force

[mysqld]
key_buffer_size=16M
```

Nếu `my.cnf` được xử lý bởi [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server"), chỉ nhóm `[mysqld]` trong `/home/mydir/myopt.cnf` được sử dụng. Nếu file được xử lý bởi [**mysqladmin**](mysqladmin.html "4.5.2 mysqladmin — Client for Administering a MySQL Server"), chỉ nhóm `[mysqladmin]` được sử dụng. Nếu file được xử lý bởi một chương trình khác, không tùy chọn nào trong `/home/mydir/myopt.cnf` được sử dụng.

Chỉ thị `!Includeir` được xử lý tương tự ngoại trừ tất cả các tệp tùy chọn trong thư mục có tên được đọc.

Nếu một tệp tùy chọn chứa các chỉ thị `! Include` hoặc `!Includeir`, các tệp được đặt tên bởi các chỉ thị đó được xử lý bất cứ khi nào tệp tùy chọn được xử lý, bất kể chúng xuất hiện ở đâu trong tệp.
