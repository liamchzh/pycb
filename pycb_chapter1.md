第一章 文本
===========
前言
----
文本是一串字符，正是它与二进制数据之间的不同，但是，别忘了计算机的世界都只是0和1.  
基本的文本操作：

* 解析数据并将数据放入程序内部的结构中；
* 将数据以某种方式转化为另一种相似的形式，数据本身发生了改变；
* 生成全新数据。

文本的来源：

* 内存
* 磁盘
* 网络

一些基础：

* 字符串是无法改变的，意味着无论进行什么操作，总是创建了一个新的字符串对象。
* 在字符串前加'r'or'R'，表示是一个真正的“原”字符串，所以反斜线转以被完全忽略。
* 可以使用切片的方法访问字符串的一部分，还可以增加第三个参数作为步长。例如string[0:0:-1]
* 如果一个庞大的包含多行文本的的字符串，可以用splitlines来分隔为多个单行字符串并置入一个列表中：`list_of_lines = one_large_string.splitlines()`
* 然后用join来重新生成一个庞大的单个字符串：`one_large_string = '\n'.join(list_of_Lines)`

1.1 每次处理一个字符
--------------------
假设thestring = 'abcde'，如何处理每一个字符？  
可以创建一个列表：`thelist = list(thestring)`  
也可用for语句来循环遍历：

    for c in thestring:  
        do_something_with(c)  

或者用列表推导中的for来遍历：`results = [do_something_with(c) for c in thestring]`  
或者用内建的map函数：`results = map(do_something, thestring)`  

获得字符串的集合：

    import sets
    sets.Set('abracadabra')
    Set(['a', 'r', 'b', 'c', 'd'])

1.2 字符和字符值之间的转换
------------
ASCII的转化：

* print ord('a')
* print chr(97)

Unicode的转化：

* print ord(u'\u2020')
* print repr(unichr(8224))

把字符串转化为一个包含各个字符的值的列表，同时使用map和ord：  
`list = map(ord, 'abcde')`

反过来，把包含字符值的列表转换成字符串，可以使用join,map和chr：  
`''.join(map(chr, range(97,102)))`

1.3 测试一个对象是否是类字符串
-------------------------
测试一个对象是否是字符串（或者具有类似字符串的行为模式）。  
利用内建的isinstance和basestring来快速检查某个对象是否是字符串或Unicode对象：

    def isAString(anobj):
        return isinstance(anobj, basestring)

但是这个方案却不能检查UserString模块提供的UserString类的实例。  
如果想支持这种类型，可以直接检查一个对象的行为是否像真的字符串一样，比如：

    def isStringLike(anobj):
        try: anobj + ''
        except: return False
        else: return True
其中try可以添加更多细节来检查是否具有字符串的行为特征。  

1.4 字符串对齐
--------------
正是string对象的ljust,rjust和center方法解决的问题。  
例如`'abcde.ljust(20)'`，会在'abcde'右段添加20个空格。  
还可以指定填充字符：

    print 'abc'.center(20, '+')
    ++++++++++abc++++++++++  

1.5 去除字符串两端的空格
------------------------
使用string对象的lstrip,rstrip和strip方法。它们会返回一个删除了开头、末尾或者两端空格的原字符串的拷贝。  
也可以选择去除其他字符：

    str = 'xyxxyy hejyx yyx'
    print '|'+str.strip('xy')+'|'
    output:| hejyx |
这样开头的x和y都将去除。  
关于strip的用法：

    str.strip('x')
    'yxxy hejyx y'
    str.strip('y')
    'xyxxy hejyx yxx'

1.6 合并字符串
--------------
假如pieces是一个字符串列表，可以使用join：

    largeString = ''.join(pieces)

如果想把存在某些变量的字符串片拼接起来，那么可以使用字符串格式化操作符%：

    largeString = '%s%s something %s yet more' % (str1, str2, str3)

### 讨论：
+操作符也能将字符串拼接起来。但是python中字符串对象是无法改变的，所以对字符串的任何操作，都会产生一个新的字符串对象。  
如果有少量字符串（尤其是绑定到变量上的）需要拼接，使用%通常是更好的选择。另外，对于非字符串（如数字）进行另外处理（如调用str），因为%已经暗中完成了这些工作。格式化操作符%还能将浮点数转化为字符串的同时控制它的有效位数。  
join也不会产生大量的中间结果，是一种快速、整洁、优雅的合并大量字符串的方法。  

1.7 将字符串逐字符或逐词反转
-----------------------------
反转字符串，最简单的方法是使用“步长”为-1的切片方法。

    revchars = astring[::-1]

如果要按照单词来反转字符串，即反转单词顺序，需先创建一个单词列表，然后将列表反转，再合并：

    revwords = astring.split()
    revwords.reverse() # 反转列表
    revwords = ' '.join(revwords)

或者一行解决：`revwords = ' '.join(astring.split()[::-1])`  

1.8 检查字符串中是否包含某字符集合中的字符
-----------------------------------------
检查字符串中是否出现某字符集合中的字符，最简单的方法如下：

    def containAny(seq, aset):
        for c in seq:
            if c in aset：
                return True
        return False
循环检查seq每一个子项，只要找到一个属于aset即可返回True.  

set有一个difference的方法：任何一个set对象a，a.difference(b)返回a中所有不属于b的元素，即a-b。比如：

    L1 = [1, 2, 3, 3]
    L2 = [1, 2, 3, 4]
    set(L1).difference(L2)
    output:set([])
    set(L2).difference(L1)
    output:set([4])

1.9 简化字符串的translate方法的使用
----------------------------------
用字符串的translate方法进行快速编码，但却发现很难记住这个方法和string.maketrans函数的应用细节，所以需要进行简单的封装。  

    import string
    def translator(frm='', to='', delete='', keep=None):
        if len(to) == 1:
            to = to * len(frm)
        trans = string.maketrans(frm, to)
        if keep is not None:
            allchars = string.maktrans('', '')
            delete = allchars.translate(allchars, keep.translate(allchars, delete))
        def translate(s):
            return s.translate(trans, delete)
        return translate

### 闭包

    def make_adder(addend):
        def adder(augend):return augend+addend
        return adder

    p = make_adder(23)
    q = make_adder(42)
    print p(100), q(100)
    output:123 142

闭包的奥秘，其实就是一个“根据不同配置信息得到不同结果”的功能。  
关于这一小节，可以参考[这里](http://blackgu.blogbus.com/logs/171867049.html)

1.10 过滤字符串中不属于指定集合的字符：
--------------------------------------

