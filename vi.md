## 4.2.6 Sử dụng những file tùy chọn

Hầu hết các chương trình MySQL có thể đọc các tùy chọn khởi động từ các file tùy chọn (đôi khi được gọi là file cấu hình). Các file tùy chọn cung cấp một cách thuận tiện để chỉ định các tùy chọn được sử dụng để chúng không được nhập vào dòng lệnh mỗi khi bạn chạy một chương trình. 


Để xác định một đọc các file tùy chọn hay không, hãy gọi nó bằng tùy chọn `--help`. (Đối với **mysqld** , sử dụng `--verbose` và `--help`.) Nếu chương trình đọc các file tùy chọn, tin nhắn trợ giúp cho biết file nào nó tìm và nhóm tùy chọn nào nó nhận ra.

Ghi chú

Một chương trình MySQL đã bắt đầu với tùy chọn `--no-defaults` không đọc những file tùy chọn khác trừ `.mylogin.cnf`.


Nhiều file tùy chọn là những file văn bản thuần, được tạo bằng bất cứ trình chỉnh sửa nào. Ngoại lệ là file `.mylogin.cnf` nó chứa tùy chọn đường dẫn đăng nhập. Đây là một file mã hóa được tạo bởi tiện ích [**mysql\_config\_editor**](mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility"). Nhìn [Mục 4.6.6, “**mysql\_config\_editor** — Tiện ích cấu hình MySQL”](mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility"). Một 'đường dẫn đăng nhập' là một nhóm tùy chọn chỉ cho phép trong những tùy chọn chắc chắn sau đây: `host`, `user`, `password`, `port` và `socket`. Chương trình khách hàng chỉ định đường dẫn đăng nhập để đọc từ `.mylogin.cnf` sử dụng tùy chọn [`--login-path`](option-file-options.html#option_general_login-path).

Để chỉ đinh một sự thay thế tên file đường dẫn đăng nhập, hãy đặt biến môi trường `MYSQL_TEST_LOGIN_FILE`. Biến này được sử dụng bởi tiện ích kiểm tra **mysql-test-run.pl**, nhưng cũng được công nhận bởi [**mysql\_config\_editor**](mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility") và bởi những MySQL khách ví dụ như [**mysql**](mysql.html "4.5.1 mysql — The MySQL Command-Line Tool"), [**mysqladmin**](mysqladmin.html "4.5.2 mysqladmin — Client for Administering a MySQL Server"), và vân vân.

MySQL tìm kiếm những file tùy chọn trong mô tả theo thứ tự tại phần thảo luận sau và đọc bất cứ cái nào tồn tại. Nếu một file tùy chọn bạn muốn sử dụng không tồn tại, tạo nó bằng phương thức thích hợp, như vừa thảo luận.

Ghi chú

Những file tùy chọn sử dụng với chương trình NDB Cluster được trình bày trong [Phần 21.3, Cấu hình của NDB Cluster”](mysql-cluster-configuration.html "21.3 Configuration of NDB Cluster").

Trên Windows, chương trình MySQL đọc những tùy chọn khởi động từ những file được hiển thị tại bảng dưới đây, theo thứ tự cụ thể (những file liệt kê trước đọc trước, rồi đến những file sau được đọc sau).

**Bảng những file tùy chọn 4.1 trong hệ thống Windows**

| Tên file | Mục đích |
|---|---|
| ```%PROGRAMDATA%\MySQL\MySQL Server 5.7\my.ini```,  ```%PROGRAMDATA%\MySQL\MySQL Server 5.7\my.cnf``` | Tùy chọn chung |
| `` %WINDIR%\my.ini``, `` %WINDIR%\my.cnf`` | Tùy chọn chung |
| `C:\my.ini`, `C:\my.cnf` | Tùy chọn chung |
| ``_BASEDIR_\my.ini``, ``_BASEDIR_\my.cnf`` | Tùy chọn chung |
| `defaults-extra-file` | File quy định với [`--defaults-extra-file`](option-file-options.html#option_general_defaults-extra-file), nếu có |
| ` %APPDATA%\MySQL\.mylogin.cnf` |  Tùy chọn đường dẫn đăng nhập (chỉ khách) |


Trong bảng trên, `%PROGRAMDATA%` đại diện cho thư mục file hệ thống chứa dữ liệu ứng dụng cho tất cả người dùng trên máy chủ. Đường dẫn này mặc định tại `C:\ProgramData` trên Windows Vista trở lên `C:\Documents` và `Settings\All Users\Application Data` trên các phiên bản cũ hơn của Windows.



`%WINDIR%` đại diện vị trí thư mục Windows của bạn. Nó thường là `C:\WINDOWS`. Sử dụng lệnh sau để xác định vị trí chính xác từ giá trị của biến môi trường `WINDIR`.

`C:\> echo %WINDIR%`



`%APPDATA%` đại diện giá trị của thư mục dữ liệu ứng dụng Windows. Dử dụng lệnh sau để xác định vị trí chính xác từ giá trị của biến môi trường `APPDATA`.

`C:\> echo %APPDATA%`


_`BASEDIR`_ đại diện cho thư mục cài đặt nền tảng. Khi MySQL 5.7 được cài đặt bằng MySQL Installer, nó thường là``C:\_`PROGRAMDIR`_\MySQL\MySQL 5.7 Server`` tại _`PROGRAMDIR`_ đại diện cho thư mục chương trình (thường `Program Files` trong bản Windows tiếng Anh), Xem tại [Phần 2.3.3, “MySQL Installer cho Windows”](mysql-installer.html "2.3.3 MySQL Installer for Windows").

Trên Unix và hệ thống tương tự, chương trình MySQL đọc file tùy chọn khởi động từ những file được ghi trong bảng dưới đây, theo thứ tự cụ thể (những file liệt kê trước đọc trước, rồi đến những file sau được đọc sau);

Ghi chú

Trên nền tảng Unix, MySQL loại trừ những file cấu hình mà nó có thể ghi bởi bất cứ ai. Đây là ý định giống như một biện pháp an ninh.

**Bảng 4.2 những file tùy chọn được đọc trên các hệ thống giống Unix và Unix**

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

Nếu nhiều trường hợp của một tùy chọn được tìm thấy, trường hợp cuối cùng sẽ lấy cái trước đó, với một ngoại lệ: Với [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server"), trường hợp đầu tiên của tùy chọn [`--user`](server-options.html#option_mysqld_user) được sử dụng giống như một biện pháp an ninh, để ngặn chặn một người dùng cụ thể trong file tùy chọn từ sự ghi đè trong dòng lệnh.

Phần mô tả dươi đây của cú pháp file tùy chọn áp dụng cho những file mà bạn chỉnh sửa thủ công. Điều này không bao gồm `.mylogin.cnf`, nó được tại ra bằng [**mysql\_config\_editor**](mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility") và được mã hóa.

Bất cứ tùy chọn dài nào có thể đưa ra trong dòng lệnh khi chạy một chương trình MySQL có thể đưa ra trong một file tùy chọn. Để lấy một danh sách tùy chọn khả dụng cho một chương trình, chạy nó với tùy chọn `--help`. (Với [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server"), sử dụng [`--verbose`](server-options.html#option_mysqld_verbose) và [`--help`](server-options.html#option_mysqld_help).)

Cứ pháp cho những tùy chọn chỉ định trong một file tùy chọn là giống nhau trên dòng lệnh (xem [Phần 4.2.4, “Sử dụng tùy chọn trên Command Line”](command-line-options.html "4.2.4 Using Options on the Command Line")). Tuy nhiên trong một file tùy chọn, bạn bỏ qua hai gạch đầu dòng từ tên tùy chọn và bạn chỉ định duy nhất một tùy chọn một dòng. Ví dụ, `--quick` và `--host=localhost` trên dòng lệnh bạn nên chỉ định như `quick` và `host=localhost` trên những dòng riêng biệt trên một file tùy chọn. Để chỉ định một tùy chọn của biểu mẫu ``--loose-_`opt_name`_`` trong một file tùy chọn, hãy viết nó như sau ``loose-_`opt_name`_``.

Những dòng trống trong những file tùy chọn bị bỏ bỏ. Những dòng còn lại có thể như bất cứ dạng sau:

*   ``#_`comment`_``, ``;_`comment`_``
    
    Dòng comment bắt đầu với `#` hoặc `;`. Một `#` comment có thể bắt đầu ở giữa dòng vẫn được.
    
*   ``[_`group`_]``
    
    _`group`_ là tên của chương trình hoặc nhóm cho cái bạn muốn đặt tùy chọn. Sau một nhóm dòng, bất cứ dòng cài đặt tùy chọn nào đều áp dụng cho đặt tên nhóm cho đến cưới cùng của file tùy chọn hoặc một nhóm dòng khác được đưa ra. Tên nhóm tùy chọn không phải trường hợp nhạy cảm.
    
*   ``_`opt_name`_``
    
    Nó tương đương với ``--_`opt_name`_`` trên dòng lệnh.
    
*   ``_`opt_name`_=_`value`_``
        
    Nó tương đương với ``--_`opt_name`_=_`value`_`` trên dòng lệnh. Trong một file tùy chọn, bạn có thể dùng dấu cách trước và sau ký tự `=`, một số không đúng trên dòng lệnh. Giá trị tùy chọn có thể được đi kèm với một hoặc hai dấu ngoặc kép, nó hữu dụng nếu giá trị chứa một ký tự comment `#`. 
    

Leading and trailing spaces are automatically deleted from option names and values.

You can use the escape sequences `\b`, `\t`, `\n`, `\r`, `\\`, and `\s` in option values to represent the backspace, tab, newline, carriage return, backslash, and space characters. In option files, these escaping rules apply:

*   A backslash followed by a valid escape sequence character is converted to the character represented by the sequence. For example, `\s` is converted to a space.
    
*   A backslash not followed by a valid escape sequence character remains unchanged. For example, `\S` is retained as is.
    

The preceding rules mean that a literal backslash can be given as `\\`, or as `\` if it is not followed by a valid escape sequence character.

The rules for escape sequences in option files differ slightly from the rules for escape sequences in string literals in SQL statements. In the latter context, if “_`x`_” is not a valid escape sequence character, ``\_`x`_`` becomes “_`x`_” rather than ``\_`x`_``. See [Section 9.1.1, “String Literals”](string-literals.html "9.1.1 String Literals").

The escaping rules for option file values are especially pertinent for Windows path names, which use `\` as a path name separator. A separator in a Windows path name must be written as `\\` if it is followed by an escape sequence character. It can be written as `\\` or `\` if it is not. Alternatively, `/` may be used in Windows path names and will be treated as `\`. Suppose that you want to specify a base directory of `C:\Program Files\MySQL\MySQL Server 5.7` in an option file. This can be done several ways. Some examples:

Press CTRL+C to copy

 

`basedir="C:\Program Files\MySQL\MySQL Server 5.7"
basedir="C:\\Program Files\\MySQL\\MySQL Server 5.7"
basedir="C:/Program Files/MySQL/MySQL Server 5.7"
basedir=C:\\Program\sFiles\\MySQL\\MySQL\sServer\s5.7`

If an option group name is the same as a program name, options in the group apply specifically to that program. For example, the `[mysqld]` and `[mysql]` groups apply to the [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server") server and the [**mysql**](mysql.html "4.5.1 mysql — The MySQL Command-Line Tool") client program, respectively.

The `[client]` option group is read by all client programs provided in MySQL distributions (but _not_ by [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server")). To understand how third-party client programs that use the C API can use option files, see the C API documentation at [Section 27.8.7.50, “mysql_options()”](mysql-options.html "27.8.7.50 mysql_options()").

The `[client]` group enables you to specify options that apply to all clients. For example, `[client]` is the appropriate group to use to specify the password for connecting to the server. (But make sure that the option file is accessible only by yourself, so that other people cannot discover your password.) Be sure not to put an option in the `[client]` group unless it is recognized by _all_ client programs that you use. Programs that do not understand the option quit after displaying an error message if you try to run them.

List more general option groups first and more specific groups later. For example, a `[client]` group is more general because it is read by all client programs, whereas a `[mysqldump]` group is read only by [**mysqldump**](mysqldump.html "4.5.4 mysqldump — A Database Backup Program"). Options specified later override options specified earlier, so putting the option groups in the order `[client]`, `[mysqldump]` enables [**mysqldump**](mysqldump.html "4.5.4 mysqldump — A Database Backup Program")-specific options to override `[client]` options.

Here is a typical global option file:

Press CTRL+C to copy

 

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

Here is a typical user option file:

Press CTRL+C to copy

 

```
[client]
# The following password will be sent to all standard MySQL clients
password="my password"

[mysql]
no-auto-rehash
connect_timeout=2
```

To create option groups to be read only by [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server") servers from specific MySQL release series, use groups with names of `[mysqld-5.6]`, `[mysqld-5.7]`, and so forth. The following group indicates that the [`sql_mode`](server-system-variables.html#sysvar_sql_mode) setting should be used only by MySQL servers with 5.7.x version numbers:

Press CTRL+C to copy

 

`[mysqld-5.7]
sql_mode=TRADITIONAL`

It is possible to use `!include` directives in option files to include other option files and `!includedir` to search specific directories for option files. For example, to include the `/home/mydir/myopt.cnf` file, use the following directive:

Press CTRL+C to copy

 

`!include /home/mydir/myopt.cnf`

To search the `/home/mydir` directory and read option files found there, use this directive:

Press CTRL+C to copy

 

`!includedir /home/mydir`

MySQL makes no guarantee about the order in which option files in the directory will be read.

Note

Any files to be found and included using the `!includedir` directive on Unix operating systems _must_ have file names ending in `.cnf`. On Windows, this directive checks for files with the `.ini` or `.cnf` extension.

Write the contents of an included option file like any other option file. That is, it should contain groups of options, each preceded by a ``[_`group`_]`` line that indicates the program to which the options apply.

While an included file is being processed, only those options in groups that the current program is looking for are used. Other groups are ignored. Suppose that a `my.cnf` file contains this line:

Press CTRL+C to copy

 

`!include /home/mydir/myopt.cnf`

And suppose that `/home/mydir/myopt.cnf` looks like this:

Press CTRL+C to copy

 

```
[mysqladmin]
force

[mysqld]
key_buffer_size=16M
```

If `my.cnf` is processed by [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server"), only the `[mysqld]` group in `/home/mydir/myopt.cnf` is used. If the file is processed by [**mysqladmin**](mysqladmin.html "4.5.2 mysqladmin — Client for Administering a MySQL Server"), only the `[mysqladmin]` group is used. If the file is processed by any other program, no options in `/home/mydir/myopt.cnf` are used.

The `!includedir` directive is processed similarly except that all option files in the named directory are read.

If an option file contains `!include` or `!includedir` directives, files named by those directives are processed whenever the option file is processed, no matter where they appear in the file.