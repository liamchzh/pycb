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
给定一个需要保留的字符的集合，构建一个过滤函数。  
使用string对象的translate方法。因为translate的第二个参数是需要删除的字符，所以需要为该字符集合准备一个补集。  

    import string
    allchars = string.maktrans('', '') #无需映射
    def makefilter(keep):
        delchars = allchars.translate(allchars, keep)
        # 删除掉要keep的，就是要删除的字符集，即keep的补集。
        def thefilter(s):
            return s.translate(allchars, delchars)
        return thefilter
    
    if __name__ == '__main__':
        just_vowels = makefilter('aeiou')
        print just_vowels('abcdefghi')

关于maketrans和translate的使用请看[这里](http://hi.baidu.com/whlcshit/item/009fc9fc9f70b1733d198b8f)  

1.11 检查一个字符串是文本还是二进制
-----------------------------------
采用Perl的判定方法，如果字符串包含了空值或者其中30%的字符编码大于126或者是奇怪的控制码，就认为是二进制数据。  

    from __future__ import division
    import string
    text_characters = "".join(map(chr, range(32, 127))) + "\n\r\t\b"
    _null_trans = string.maketrans("", "")
    def istext(s, text_characters = text_characters, threshold = 0.30)
        s包含空值，就不是文本
        if "\0" in s:
            return Flase
        # 空字符串是文本
        if not s:
            return True
        t = s.translate(_null_trans, text_characters) # 删掉字符，剩余就是非字符
        return len(t)/len(s) <= threshold # 非字符的比例小于30%，即文本

这小节继续是maketrans和translate的使用。  

1.12 控制大小写
---------------
字符串对象有upper和lower的方法，不需要参数，直接返回一个字符串的拷贝：`little.upper()`和`big.lower`  
而`capitalize()`和`title()`分别是第一个字符改成大写，其余转成小写和每一个单词首字母大写，其余字母小写。  

另外，python中还可以检查一个字符串是否已经满足需求的形式，比如isupper,islower,istitle。但是没有iscapitalized，下面给出一个严格的判断iscapitalized的代码：

    import string
    notrans = string.maketrans("", "")
    def containsAny(str, strset):
        return len(strset) != len(strset.translate(notrans, str))
        # 检查是否包含字母
    def iscapilized(s):
        return s == s.capitalize() and containsAny(s, string.letters)

1.13 访问子字符串
-----------------
切片是个好方法，但是它只能取得一个字段。如果考虑字段的长度，struct.unpack会更合适。  
将字符串拆分成固定长度，可以利用带列表推导（LC）的切片方法：
    
    fiver = [theline[k:k+5] for k in xrange(0, len(theline), 5)]

如果想把数据切成指定长度的列，用带LC的方法是最简单的：

    cuts = [8, 14, 20, 26]
    pieces = [theline[i:j] for i, j in zip([0]+cuts, cuts+[None])]
    # zip将会把两个序列按照索引的顺序组成新的tuple
    # zip([0]+cuts, cuts+[None])输出如下：
    # [(0, 8), (8, 14), (14, 20), (20, 26), (26, None)]

以上两个方法封装成函数：

    def split_by(theline, n, lastfield=False):
        pieces = [theline[k:k+n] for k in xrange(0, len(theline), n)]
        if not lastfield and len(pieces[-1]) < n:
            pieces.pop()
        return pieces

    def split_at(theline, cuts, lastfield=False):
        pieces = [theline[i:j] for i, j in zip([0]+cuts, cuts+[None])]
        if not lastfield:
            pieces.pop()
        return pieces

用生成器可以实现一个完全不同的方式：

    def split_at(the_line, cuts, lastfield=False):
        last = 0
        for cut in cuts:
            yield the_line[last:cut]
            last = cut
        if lastfield:
            yield the_line[last:]

    def split_by(the_line, n, lastfield=False):
        return split_at(the_line, xrange(0, len(the_line), n), lastfield)

    list_of_fields = list(split_by(the_line, 5))

1.14 改变多行文本字符串的缩进
-----------------------------
在每行行首添加或删除一些空格，以保证每行的缩进都是指定数目的空格数。  

    def reindent(s, numSpaces):
        leading_space = numSpaces * ' '
        lines = [leading_space + line.strip() for line in s.plitlines()]
        return '\n'.join(lines)

如果不想改变现有的缩进：

    def addSpaces(s, numAdd):
        white = numAdd * ''
        return white + white.join(s.splitlines(True))
    def numSpaces(s):
        return [len(line)-len(line.lstrip()) for line in s.splitlines()]
    def delSapces(s, numDel):
        if numDel > min(numSpaces(s))
            raise ValueError, "Error!"
        return '\n'.join([line[numDel:] for line in s.splitlines()])

1.15 扩展和压缩制表符
--------------------
将字符串中的制表符转化成一定数目的空格，或反其道而行。  

1. 将制表符转为一定数目的空格：  
`mystring = mystring.expandtabs()` #默认制表符的宽度为8

2. 将空格转为制表符：  

    def unexpand(astring, tablen=8):
        import re
        pieces = re.split(r'( +)', astring.expandtabs(tablen))
        lensofar = 0
        for i, piece in enumerate(pieces):
            thislen = len(piece)
            lensofar += thislen
            if piece.isspace():
                numblanks = lensofar % tablen
                numtabs = (thislen-numblanks+tablen-1)/tablen
                pieces[i] = '\t' * numtabs + ' ' * numblanks
            return ''.join(pieces)

### 讨论
如何处理制表符和空格混合的字符串？

1.16 & 1.17 替换字符串中的子串  
------------------------------
有一个string.Template类，可以完成任务。然后为substitute提供字典参数。

    import string
    new_style = string.Template("this is $thing")
    print new_style.substitute({'thing':5}) # output:this is 5
    print new_style.substitite({'thing':'test'}) # output:this is test
    print new_style.substitute(thing=5) # output:this is 5
    print new_style.substitute(thing='test') # output:this is test

再看一个例子：

    msg = string.Template("the square of $number is $square")
    for i in range(10):
        print msg.substitute(number=i, square=i*i)

1.8 一次完成多个替换
--------------------
re（正则）提供了强大的sub方法，高效地进行匹配替换。

    import re
    def multiple_replace(text, adict):
        rx = re.compile('|'.join(map(re.escape, adict)))
        def one_xlat(match):
            return adict[match.group(0)]
        return rx.sub(one_xlat, text)

首先，rx把需要替换的字符从adict提取出来，变成a1|a2|...|aN的形式。然后，不直接给re.sub传递用于替换的字符串，而是传入回调函数参数。  
如果只使用同一个固定不变的翻译表来完成很多文本的替换，这种情况也许希望只做一次准备工作，出于这种需求，也许会使用以下基于闭包的方式：

    import re
    def make_xlat(args, **kwds): # args应该为*args，下同
        adict = dict(args, **kwds)
        rx = re.compile('|'.join(map(re.escape, adict)))
        def one_xlat(match):
            return adict[match.group(0)]
        def xlat(text):
            return rx.sub(one_xlat, text)
        return xlat

替换通常是单词的替换。通过特殊的r'\b'序列，正则表达式可以很好地找到单词开始和结束的位置。只需修改rx的部分，从而可以完成一些自定义的任务：

    rx = re.compile(r'\b%s\b' % r'\b|\b'.join(map(re.escape, adict)))

其余代码和前面给出的一样。但是，如果有大量的这样自定义的工作，代码就会大量地重复。所以，最好将其变成一个类：

    class make_xlat:
        def __init__(self, args, **kwds): # args应该为*args，下同
            self.adict = dict(args, **kwds)
            self.rx = self.make_rx()
        def make_rx(self):
            return re.compile('|'.join(map(re.escape, adict)))
        def one_xlat(self, match):
            return self.adict[match.group(0)]
        def __call__(self, text):
            return self.rx.sub(self.one_xlat, text)

这样就能通过修改make_rx()来实现重载。

1.19 检查字符串中的结束标记
---------------------------
给出一个字符串s，检查s中是否存在多个结束标记中的一个。

    import itertools
    def anyTrue(predicate, sequence):
        return True in itertools.imap(predicate, sequence)
    def endsWith(s, *endings):
        return anyTrue(s.endswith, endings)

imap和map不同的是，imap返回的是一个iteration对象，而map返回的是一个list对象。  
imap等价于：

    def imap(function, *iterables):
         iterables = map(iter, iterables)
         while True:
             args = [i.next() for i in iterables]
             if function is None:
                 yield tuple(args)
             else:
                 yield function(args) #args应为*args

典型应用：打印当前目录所有的图片文件  

    import os
    for filename in os.listdir('.'):
        if endsWith(filename, '.jpg', '.jpeg', '.gif'):
            print filename

1.20 使用Unicode来处理国际化文本
-------------------------------
任务：处理包含了非ASCII字符的文本字符串。  
使用python提供的内置unicode类型，可以轻松转换：`german_ae = unicode('\xc3\xa4', 'utf-8')`  
unicode字符串和ascii字符串的操作会产生unicode字符串：`sentence = "this is a" + german_ae`  

unicode字符串和未声明编码的字节串的操作，python会拒绝猜测其编码，直接给出UnicodeDecodeError.  
所以当需要处理“外部”的文本数据时，应当先创建一个unicode对象，找出最合适的编码再转换。  


1.21 在Unicode和普通字符串之间转换
----------------------------------
使用encode和decode：

    unicodestring = u"hello world"
    # unicode转化成普通字符串
    utf8string = unicodestring.encode("utf-8")
    asciistring = unicodestring.encode("ascii")
    # 转换成unicode
    plainstring1 = unicode(utf8string, "utf-8")
    plainstring2 = unicode(asciistring, "ascii")

除了"utf-8"和"ascii"，还有很多编码，可以用作encode和decode的参数。关于unicode的内容还有很多。

1.22 在标准输出中打印unicode字符
-------------------------------
在IDLE中，python可能知道输出的信息是什么编码，但是在终端中，python默认使用utf-8编码，所以输出的内容可能不是想要的。  
例如上上小节中的u'\xc3\xa4'，是德文字母，如果直接输出是：Ã¤。但是通过python标准库的codecs模块，使终端使用"latin-1"来显示，则变成ä  

    import codecs, sys
    sys.stdout = codecs.lookup("latin-1")[-1](sys.stdout)


