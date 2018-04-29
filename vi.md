### 4.2.6 Using Option Files

Most MySQL programs can read startup options from option files (sometimes called configuration files). Option files provide a convenient way to specify commonly used options so that they need not be entered on the command line each time you run a program.

To determine whether a program reads option files, invoke it with the `--help` option. (For [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server"), use [`--verbose`](server-options.html#option_mysqld_verbose) and [`--help`](server-options.html#option_mysqld_help).) If the program reads option files, the help message indicates which files it looks for and which option groups it recognizes.

Note

A MySQL program started with the `--no-defaults` option reads no option files other than `.mylogin.cnf`.

Many option files are plain text files, created using any text editor. The exception is the `.mylogin.cnf` file that contains login path options. This is an encrypted file created by the [**mysql\_config\_editor**](mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility") utility. See [Section 4.6.6, “**mysql\_config\_editor** — MySQL Configuration Utility”](mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility"). A “login path” is an option group that permits only certain options: `host`, `user`, `password`, `port` and `socket`. Client programs specify which login path to read from `.mylogin.cnf` using the [`--login-path`](option-file-options.html#option_general_login-path) option.

To specify an alternative login path file name, set the `MYSQL_TEST_LOGIN_FILE` environment variable. This variable is used by the **mysql-test-run.pl** testing utility, but also is recognized by [**mysql\_config\_editor**](mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility") and by MySQL clients such as [**mysql**](mysql.html "4.5.1 mysql — The MySQL Command-Line Tool"), [**mysqladmin**](mysqladmin.html "4.5.2 mysqladmin — Client for Administering a MySQL Server"), and so forth.

MySQL looks for option files in the order described in the following discussion and reads any that exist. If an option file you want to use does not exist, create it using the appropriate method, as just discussed.

Note

Option files used with NDB Cluster programs are covered in [Section 21.3, “Configuration of NDB Cluster”](mysql-cluster-configuration.html "21.3 Configuration of NDB Cluster").

On Windows, MySQL programs read startup options from the files shown in the following table, in the specified order (files listed first are read first, files read later take precedence).

**Table 4.1 Option Files Read on Windows Systems**

|File Name | Purpose |
|-----------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|

|```%PROGRAMDATA%\MySQL\MySQL Server 5.7\my.ini```,  ```%PROGRAMDATA%\MySQL\MySQL Server 5.7\my.cnf```| Global options |
|`` `%WINDIR%`\my.ini``, `` `%WINDIR%`\my.cnf``| Global options |

| `C:\my.ini`, `C:\my.cnf` | Global options |

| ``_`BASEDIR`_\my.ini``, ``_`BASEDIR`_\my.cnf`` | Global options |
| `defaults-extra-file` | The file specified with [`--defaults-extra-file`](option-file-options.html#option_general_defaults-extra-file), if any |
| `` `%APPDATA%`\MySQL\.mylogin.cnf`` |  Login path options (clients only) |

  

In the preceding table, `%PROGRAMDATA%` represents the file system directory that contains application data for all users on the host. This path defaults to `C:\ProgramData` on Microsoft Windows Vista and greater, and `C:\Documents and Settings\All Users\Application Data` on older versions of Microsoft Windows.

`%WINDIR%` represents the location of your Windows directory. This is commonly `C:\WINDOWS`. Use the following command to determine its exact location from the value of the `WINDIR` environment variable:

Press CTRL+C to copy

 

`C:\> echo %WINDIR%`

`%APPDATA%` represents the value of the Windows application data directory. Use the following command to determine its exact location from the value of the `APPDATA` environment variable:

Press CTRL+C to copy

 

`C:\> echo %APPDATA%`

_`BASEDIR`_ represents the MySQL base installation directory. When MySQL 5.7 has been installed using MySQL Installer, this is typically ``C:\_`PROGRAMDIR`_\MySQL\MySQL 5.7 Server`` where _`PROGRAMDIR`_ represents the programs directory (usually `Program Files` on English-language versions of Windows), See [Section 2.3.3, “MySQL Installer for Windows”](mysql-installer.html "2.3.3 MySQL Installer for Windows").

On Unix and Unix-like systems, MySQL programs read startup options from the files shown in the following table, in the specified order (files listed first are read first, files read later take precedence).

Note

On Unix platforms, MySQL ignores configuration files that are world-writable. This is intentional as a security measure.

**Table 4.2 Option Files Read on Unix and Unix-Like Systems**

File Name

Purpose

`/etc/my.cnf`

Global options

`/etc/mysql/my.cnf`

Global options

``_`SYSCONFDIR`_/my.cnf``

Global options

`$MYSQL_HOME/my.cnf`

Server-specific options (server only)

`defaults-extra-file`

The file specified with [`--defaults-extra-file`](option-file-options.html#option_general_defaults-extra-file), if any

`~/.my.cnf`

User-specific options

`~/.mylogin.cnf`

User-specific login path options (clients only)

  

In the preceding table, `~` represents the current user's home directory (the value of `$HOME`).

_`SYSCONFDIR`_ represents the directory specified with the [`SYSCONFDIR`](source-configuration-options.html#option_cmake_sysconfdir) option to **CMake** when MySQL was built. By default, this is the `etc` directory located under the compiled-in installation directory.

`MYSQL_HOME` is an environment variable containing the path to the directory in which the server-specific `my.cnf` file resides. If `MYSQL_HOME` is not set and you start the server using the [**mysqld_safe**](mysqld-safe.html "4.3.2 mysqld_safe — MySQL Server Startup Script") program, [**mysqld_safe**](mysqld-safe.html "4.3.2 mysqld_safe — MySQL Server Startup Script") sets it to _`BASEDIR`_, the MySQL base installation directory.

_`DATADIR`_ is commonly `/usr/local/mysql/data`, although this can vary per platform or installation method. The value is the data directory location built in when MySQL was compiled, not the location specified with the [`--datadir`](server-options.html#option_mysqld_datadir) option when [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server") starts. Use of [`--datadir`](server-options.html#option_mysqld_datadir) at runtime has no effect on where the server looks for option files that it reads before processing any options.

If multiple instances of a given option are found, the last instance takes precedence, with one exception: For [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server"), the _first_ instance of the [`--user`](server-options.html#option_mysqld_user) option is used as a security precaution, to prevent a user specified in an option file from being overridden on the command line.

The following description of option file syntax applies to files that you edit manually. This excludes `.mylogin.cnf`, which is created using [**mysql\_config\_editor**](mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility") and is encrypted.

Any long option that may be given on the command line when running a MySQL program can be given in an option file as well. To get the list of available options for a program, run it with the `--help` option. (For [**mysqld**](mysqld.html "4.3.1 mysqld — The MySQL Server"), use [`--verbose`](server-options.html#option_mysqld_verbose) and [`--help`](server-options.html#option_mysqld_help).)

The syntax for specifying options in an option file is similar to command-line syntax (see [Section 4.2.4, “Using Options on the Command Line”](command-line-options.html "4.2.4 Using Options on the Command Line")). However, in an option file, you omit the leading two dashes from the option name and you specify only one option per line. For example, `--quick` and `--host=localhost` on the command line should be specified as `quick` and `host=localhost` on separate lines in an option file. To specify an option of the form ``--loose-_`opt_name`_`` in an option file, write it as ``loose-_`opt_name`_``.

Empty lines in option files are ignored. Nonempty lines can take any of the following forms:

*   ``#_`comment`_``, ``;_`comment`_``
    
    Comment lines start with `#` or `;`. A `#` comment can start in the middle of a line as well.
    
*   ``[_`group`_]``
    
    _`group`_ is the name of the program or group for which you want to set options. After a group line, any option-setting lines apply to the named group until the end of the option file or another group line is given. Option group names are not case-sensitive.
    
*   ``_`opt_name`_``
    
    This is equivalent to ``--_`opt_name`_`` on the command line.
    
*   ``_`opt_name`_=_`value`_``
    
    This is equivalent to ``--_`opt_name`_=_`value`_`` on the command line. In an option file, you can have spaces around the `=` character, something that is not true on the command line. The value optionally can be enclosed within single quotation marks or double quotation marks, which is useful if the value contains a `#` comment character.
    

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