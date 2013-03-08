第二章：文件
------------
###读取和写入
python支持对文件对象的迭代处理

    for line in input:
        process(line)

如果不确定文件会用什么样的换行符，或者不确定所在的操作系统，可以将open函数的第二个参数设置为'rU'，表示通用换行符。  
[文件对象的方法](http://docs.python.org/2/tutorial/inputoutput.html#methods-of-file-objects)：

* `f.read(size)` 当不给出size时，会读取整个文件内容。当文件指针指向文件最后时，会返回空字符串''。
* `f.readline()` 将读取一整行，直到遇到换行符'\n'。
* `f.readlines()` 将返回一个包含所有行的**列表**。
* `f.write(string)` 将string写入文件。
* `f.tell()` 返回一个整数，表示文件对象在文件中的当前位置。
* `f.seek(offset[, from_what])` 重新定位文件对象位置，from_what的值表示从哪里开始算起。0（默认）：从文件头；1：从当前位置；2：从文件尾。

###改文件名
