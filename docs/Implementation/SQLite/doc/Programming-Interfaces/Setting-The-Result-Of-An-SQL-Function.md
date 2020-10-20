# [Setting The Result Of An SQL Function](https://sqlite.org/c3ref/result_blob.html)



The `sqlite3_result_text()`, `sqlite3_result_text16()`, `sqlite3_result_text16le()`, and `sqlite3_result_text16be()` interfaces set the return value of the application-defined function to be a text string which is represented as UTF-8, UTF-16 native byte order, UTF-16 little endian, or UTF-16 big endian, respectively.

The `sqlite3_result_text64()` interface sets the return value of an application-defined function to be a text string in an encoding specified by the fifth (and last) parameter, which must be one of [SQLITE_UTF8](https://sqlite.org/c3ref/c_any.html), [SQLITE_UTF16](https://sqlite.org/c3ref/c_any.html), [SQLITE_UTF16BE](https://sqlite.org/c3ref/c_any.html), or [SQLITE_UTF16LE](https://sqlite.org/c3ref/c_any.html). 

SQLite takes the text result from the application from the 2nd parameter of the `sqlite3_result_text*` interfaces. 

If the 3rd parameter to the `sqlite3_result_text*` interfaces is negative, then SQLite takes result text from the 2nd parameter through the first zero character. 

If the 3rd parameter to the `sqlite3_result_text*` interfaces is non-negative, then as many bytes (not characters) of the text pointed to by the 2nd parameter are taken as the application-defined function result. 

If the 3rd parameter is non-negative, then it must be the byte offset into the string where the NUL terminator would appear if the string where NUL terminated. If any NUL characters occur in the string at a byte offset that is less than the value of the 3rd parameter, then the resulting string will contain embedded NULs and the result of expressions operating on strings with embedded NULs is undefined. 

If the 4th parameter to the `sqlite3_result_text*` interfaces or `sqlite3_result_blob` is a non-NULL pointer, then SQLite calls that function as the destructor on the text or BLOB result when it has finished using that result. 

If the 4th parameter to the `sqlite3_result_text*` interfaces or to `sqlite3_result_blob` is the special constant `SQLITE_STATIC`, then SQLite assumes that the text or BLOB result is in constant space and does not copy the content of the parameter nor call a destructor on the content when it has finished using that result. 

If the 4th parameter to the `sqlite3_result_text*` interfaces or `sqlite3_result_blob` is the special constant `SQLITE_TRANSIENT` then SQLite makes a copy of the result into space obtained from [sqlite3_malloc()](https://sqlite.org/c3ref/free.html) before it returns.