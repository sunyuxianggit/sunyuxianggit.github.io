---
title: Effective Python
date: 2020-03-10 19:14:04
tags: python
---


编写高质量代码的59+91个建议读书笔记

### 第一条
* 确认自己所用的python版本。
* 确保该版本与你想使用的python版本相符。
* 优先使用python3

##### Windows平台下：
`$python --version`
![a](Effective-Python/2020-03-07-11-15-23.png)

<!-- more -->

##### 其他程序内：
* Autodesk Maya:
![b](Effective-Python/2020-03-07-11-18-24.png)

* Substance Designer:
![c](Effective-Python/2020-03-07-11-19-36.png)
注：SD里sys.version_info报错，原因未知

* Houdini：
![d](Effective-Python/2020-03-07-11-22-33.png)

Tips： Python中sys模块还有一个常用功能：
`sys.path`可以用来找到应用程序内的python编译器位置.

### 第二条
遵循PEP8 风格指南
《Python Enhancement Proposal #8》（8号Python增强法案）又叫PEP8,它是针对Python代码格式而编订的风格指南。
* 使用空格来表示缩进，而不要用制表符（tab）。
* 和语法相关的每一层缩进都用四个空格表示。
* 每行的字符数不应超过79。
* 对于占据多行的长表达式，除了首行之外的其余各行都应该在通常的缩进级别之上再加四个空格。
* 文件中的代码与函数和类之间应该用两个空行隔开。
* 在同一个类中，各方法之间应该用一个空行隔开。
* 在使用下标来获取列表元素、调用函数或给关键字参数赋值的时候，不要再两边添加空格。
* 为变量赋值的时候，赋值符号的左侧和右侧应该各自写上一个空格，而且只写一个。

命名：PEP8 提倡采用不同的命名风格来编写Python代码中的各个部分,以便再阅读代码时可以根据这些名称看出它们的角色。
* 函数、变量名及属性应该用小写字母，各单词之间用下划线相连，例如，lowercase_underscore。
* 受保护的实例属性，应该以单个下划线开头，例如，_leading_underscore.
* 私有的实例属性，应该以两个下划线开头，例如__double_leading_underscore.
* 类与异常， 应该以每个单词首字母均大写的形式来命名，例如，CapitalizedWord。
* 模块级别的常量，应该全部采用大写字母来拼写，各单词之间以下划线连接，例如，ALL_CAPS。
* 类中的实例方法（instance method），应该把首个参数命名self，以表示该对象自身.
* 类方法（class method）的首个参数，应该命名cls，以表示该类自身。

表达式和语句《The Zen of Python》（python之禅）中说，每件事都应该有直白的做法，而且最好只有一种。
* 采用内联形式的否定词 ，而不要把否定词放在整个表达式的前面，例如，应该写 if a is not b 而不是 if not a is b。
* 不要通过检测长度的方法来判断列表是否为空，而是应该采用if not somelist来判定。
* 同上条，如果判断列表不为空也是一样的， 
* 不要编写单行的if语句、for循环、while循环及except复合语句，而是应该把这些语句分成多行来书写，以示清晰。
* import语句应该重视放在文件开头.
* 引入模块的时候，总是应该使用绝对名称，而不应该根据当前模块的路径来使用相对名称，例如，引入bar包中的foo模块时，应该完成的写出 from bar import foo，而不应该简写为import foo。
* 如果一定要以相对的名称来编写import语句，那就采用明确的写法:from.import foo。
* 文件中的那些import语句应该按照顺序划分为三个部分，分别为标准库模块、第三方模块以及自用模块。各import语句应该按照模块的字母顺序来排列。

Tips： vscode可以采用pylint来自动检测受测代码是否符合pep8。
https://www.pylint.org/

### 第三条
##### 了解bytes、str、和unicode的区别
* python3有两种表示字符序列的类型，bytes和str，前者的实例中包含原始的8位值，后者的实例中包含Unicode字符。
* python2中也有两种表示字符序列的类型，str和Unicode. 前者包含原始的八位值，后者的实例着包含unicode字符。
![e](Effective-Python/2020-03-07-14-29-10.png)
* bytes 类型，是指一堆字节的集合，十六进制表现形式，两个十六进制数构成一个 byte ，以 b 开头的字符串是 bytes 类型。计算机只能存储二进制，字符、图片、视频、音乐等想存到硬盘上，必须以正确的方式编码成二进制后再存，但是转成二进制后不是直接以 0101010 的形式表示的，而是用bytes() 的类型来表示的。
* 把Unicode字符表示为二进制数据（原始八位值）有很多办法，常见且推荐的编码方式就是UTF-8。
* 但是python3 的str实例和python2的Unicode实例都没有和特定的二进制编码形式相关联，想要把Unicode字符转换为二进制数据，就必须使用encode方法，想要把二进制数据转化成为Unicode字符，则必须使用decode 方法。
![f](Effective-Python/2020-03-07-14-34-59.png)
* 在 Python3 中内存里的字符串是以 Unicode 编码的，Unicode 的其中一个特性就是跟所有语言编码都有映射关系，所以 UTF-8 格式的文件，在 Windows 电脑上若是不能看，就可以把 UTF-8 先解码成 Unicode ，再由 Unicode 编码成 GBK 就可以了。
![g](Effective-Python/2020-03-07-14-32-20.png)
##### 字符串的转换
* 在Python3中，接受str或byts，并总是返回str的方法:
    ```py
    def to_str(bytes_or_str):
        if isinstance(bytes_or_str, bytes):#注意这个函数
            value = bytes_or_str.decode('utf-8')
        else:
            value = bytes_or_str
        return value
    ```
* 接受str或bytes，并总是返回bytes的方法：
    ```py
    def to_bytes(bytes_or_str):
        if isinstance(bytes_or_str, str):
            value = bytes_or_str.encode('utf-8')
        else:
            value = bytes_or_str
        return value
    ```
* 在Python2中，接受str或unicode，并总是返回unicode的方法：
    ```
    def to_unicode(unicode_or_str):
        if isinstance(unicode_or_str, str):
            value = unicode_or_str.decode('utf-8')
        else:
            value = unicode_or_str
        return value
    ```
* 接受str或unicode，并总是返回str的方法：
    ```
    def to_str(unicode_or_str):
        if isinstance(unicode_or_str, unicode):
            value = unicode_or_str.encode('utf-8')
        else:
            value = unicode_or_str
        return value
    ```
##### 推荐的文件操作符
如果通过open函数获取文件句柄，该句柄会采用UTF-8编码格式来操作文件。
而在Python2中，文件操作的默认编码格式则是二进制形式。这可能会导致程序出现奇怪的错误。
例如，向文件中随机写入一些二进制数据。下面这种方法Python2中可以正常运行，但是在Python3中则不行：
```py
with open('/tmp/random.bin', 'w') as f:
    f.write(os.urandom(10))
>>> TypeError: must be str, not bytes
```
上述情况是因为Python3给open函数添加了名为encoding的新参数，而这个新参数默认值是’utf-8′。这样在文件句柄上进行read和write操作时，系统就要求开发者必须传入包含unicode字符的str实例，而不接受包含二进制数据的bytes实例。

解决这个问题，可以用二进制写入模式(‘wb’)来开启待操作的文件，按照这种方式可同时适配Python2和Python3：

```py
with open('/tmp/random.bin', 'wb') as f:
    f.write(os.urandom(10))
```
(读取文件也同理，可使用’rb’模式)

### 第四条
用辅助函数来取代复杂的表达式
* 开发者很容易过度运用Python的语法特性，从而写出那种特别复杂并且难以理解的单行表达式。
* 请把复杂的表达式移入辅助函数中。如果要反复使用相同的逻辑，那就更应该那么做。
* 使用if/esle表达式，要比用or或者and 这样的Boolean操作符写出的表达式更清晰。

### 第五条
了解切割序列的方法
python提供了一种把序列切成小块的写法，这种切片操作很容易四开发者轻易的访问序列中的某些元素所构成的子集。  
最简单的用法，就是对内置list和bytes进行切割。  
切割操作还可以延伸到实现了__getitem__和__setitem__这两个特殊方法的python类上，参见28条。
* 不要写多余的代码。但start索引为零或者end索引为序列长度时，应该将其忽略
* 切片操作不会计较start或者end索引是否越界，这样我们很容易从前端或者后端开始。
* 对list赋值时，如果使用切片操作，就会把原列表中处在相关范围内的值替换成新值，即便它们的长度不同也依然可以替换。

### 第六条
在单词切片操作内，不要同时指定start、 end 和 stride 
* 问题在于采用stride方式进行切片时，经常会出现不符合预期的结果
* 切割列表时，如果制定了stride，代码就会变得费解。尤其是stride为负值的时候更是如此.
* 在同一个切片操作内，不要同时使用start、end和stride. 如果确实需要执行这种操作，那就考虑将其拆解为两条赋值语句，其中一条做范围切割，另一条做步进，或者考虑使用内置itertools模块中的islice.

tips：
`mystring[::-1]#反转字符串`

### 第七条
用列表推导来取代map和filter
python提供了一种精炼的写法，可以根据一个列表来制作另外一个列表.这种表达式称为list comprehension （列表推导）
* 列表推导要比内置的map和filter函数清晰
* 列表推导可以很跳过输入列表中的某些元素
* 字典与集也支持推导表达式

### 第八条
不要使用含有两个以上表达式的列表推导
* 列表推导支持多级循环，每一级循环也支持多项条件
* 超过两个表达式的列表推导难以理解，应该尽量避免
Tips：
```py
matrix = [[1,2,3],[4,5,6],[7,8,9]]
flat = [x for row in matrix for x in row]
print(flat)
>>>[1,2,3,4,5,6,7,8,9]
```

```py
a = [1,2,3,4,5,6,7,8,9,10]
#下面两种写法是等效的
#要从列表中取出大于4的偶数
b = [x for x in a if x>4 if x %2 ==0]
c = [x for x in a if x>4 and x%2==0]
```

### 第九条
用生成器表达式来改写数据量较大的列表推导
列表推导的缺点是，对于输入序列中的每个值来说，都要创建一项仅含一项元素的全新列表，但输入数据较大时，可能会消耗大量内存，并导致程序崩溃。
为了解决此问题，python 提供了生成式表达式
```py
it = (len(x) for x in open(temp.txt))
print(it)
>>>
<generator object <genexpr> at 0x101b81480>
print(next(it))
print(next(it))
>>>
100
57
```

Tips：
获取文件每行的字符数

```py
value = [len(x) for x in open(temp.txt)]
print(value)
```

* 当输入的数据量较大时，列表推导可能会因为占用太对内存而出问题。
* 由生成表达式所返回的迭代器，可以逐次产生输出值，从而避免了内存用量问题。
* 把某个生成器表达式说返回的迭代器，放在另一个生成器表达式的for子表达式中，即可将二者组合起来。
* 串在一起的生成器表达式执行速度很快。 

### 第十条
尽量用enumerate取代range
在一些列的整数上面迭代，内置的range函数很有用，
当迭代列表的时候，通常还想知道当前元素在列表中的索引。
```py
for i in range(len(flavor_list)):
    flavor = flavor_lsit[i]
    print("%d:%s"%(i+1,flavor))
```
这种代码不利于理解，python提供了enumerate来解决此问题。enumerate可以把各种迭代器包装为生成器，以便稍后产生输出值，生成器每次产生一对输出值，前者为循环下标，后者表示从迭代器中获取到的下一个序列元素，这样写出来的代码会非常整洁。
```py
for i ,flavor in enumerate(flavor_list):
    print("%d:%s"%(i+1,flavor))
```

* enumerate函数提供了一种精简的写法，可以在遍历迭代器时获知每个元素的索引
* 尽量用enumerate来改写那种将range与下标访问相结合的序列遍历代码
* 可以给enumerate 提供第二个参数，已指定开始计数时所用的值（默认为0）


Tips:
还可以直接指定enumerate开始计数所用的值。
```py
for i ,flavor in enumerate(flavor_list，1):
    print("%d:%s"%(i,flavor))
```

### 第十一条
用zip函数同时遍历两个迭代器

* 使用for循环
```py
names = ['Cecilia', 'Kufu', 'JayChou']
letters = [len(n) for n in names]
longest_name = None
max_letters = 0

for name, count in zip(names, letters):
    if count > max_letters:
        longest_name = name
        max_letters = count
>>>
Cecilia
```
上面这段代码问题在一，整个循环语句看上去很乱，用下标来访问names和letters会使代码不易阅读。
改用enumerate可以稍稍缓解这个问题，但仍然不够理想。

* 使用for循环加enumerate
```py
names = ['Cecilia', 'Kufu', 'JayChou']
letters = [len(n) for n in names]
longest_name = None
max_letters = 0

for i, name in enumerate(names)：
    count =letters[i]
    if count > max_letters:
        longest_name = name
        max_letters = count
```
* 使用zip
```py
names = ['Cecilia', 'Kufu', 'JayChou']
letters = [len(n) for n in names]
longest_name = None
max_letters = 0

for name, count in zip(names, letter):
    if count > max_letters:
        longest_name = name
        max_letters = count
```

* 内置的zip函数可以平行的遍历多个迭代器
* Python3中的zip相当于生成器，会在遍历过程中逐次产生元组，而Python2中的zip则是直接把这些元组完全生成号，并一次性的返回给整份列表。
* 如果提供的迭代器长度不等，那么zip就会自动提前终止。
* itertools 内置模块中的zip_longest函数可以平行的遍历多个迭代器，而不用在乎它们的长度是否相等。


### 第十二条
不要在for和while循环后面写else语块

* python 有种特殊语法，可在 for及 while 循环的内部语句块之后紧跟一个else块。
* 只有当整个循环主体都没遇到break语句时，循环后面的else块才会执行。
* 不要再循环后面使用else块，因为在这种写法即不直观，又容易引人误解。
### 第十三条
合理利用try/except/else/finally 结构中的每个代码块
异常处理可能要考虑四种不同的时机，
1.finally块
如果既要向上传播，又要在异常发生时执行清理工作，那就可以使用try/finally结构 
```py
handle = open("/tmp/data.txt")   #May raise IOError
try:
    data = handle.read()         #May raise unicodeDecodeError
finally:
    handle.close()               #Always runs after try:
```
read方法抛出异常会向上传播给调用方，而finally块中的handle.close方法则一定能够执行。open方法放到try块外面是因为如果打开文件发生异常，那么程序应该跳过finally块。
2. else块
try/except/else结构可以清晰的描述出哪写异常会由自己的代码来处理，哪写异常会传播到上一级。如果try没有发生异常，那么就执行else块。
```py
def load_json(data,key):
    try:
        result_dict = json.loads(data) #May raise ValueError
    except ValueError as e:
        raise KeyError from e
    else:
        return result_dict[key]        #May raise KeyError
```
如果数据不是有效的json格式，那么会产生ValueError，这个异常会由except块来捕获处理并处理，如果能够执行，就会执行else里面的查找语句，如何查找执行有异常，那么该异常就会向上传播，因为查询语句并不在try范围内，这种else子句会把try/except后面的内容和except块本身区分开，使异常的传播行为变得更加清晰。

3. 混合使用
要从文件中读取某项事务的描述信息，处理该事务，然后就地更新该文件。
```py
UNDEFINED = object()
def divide_json(path):
    handle = open(path,"r+")
    try:
        data = handle.read()
        op = json.loads(data)
        value = (op["numrator"]/op["denominator])
    except ZeroDivisionError as e:
        return UNDEFINED
    else:
        op["result"] = value
        result = json.dumps(op)
        handle.seek(0)
        handle.write(result)
        return value
    finally:
        handle.close()
```
使用try快读取文件并处理内容，用except块来对应try块中可能发生的异常，用else块实时更新文件，并把更新中可能出现的异常回报给上级代码，然后用finally块来清理文件句柄。
即使else块写入发生异常，finally也能关闭句柄。
Tips:
1. 无论try块是否发生异常，都可以利用try/finally复合语句块来执行清理工作。
2. else块可以用来缩减try块中的代码量，并把没发生异常时所要执行的语句与try/except代码块
3. 顺利运行try之后，若想时某些操作能在finally块清理代码之前执行，则可以将这些操作写到else块中（这种写法必须由except块）。

### 第十四条
函数：尽量用异常来表示特殊情况，而不要返回None。
编写功能函数（utility function）的时候，令函数返回None，可能会使调用它的人在判断返回值的时候写出错误的代码，有两种解决办法。
1. 把返回值拆成两个部分，第一部分返回操作是否成功，第二部分返回操作结果。
2. 把异常抛给上一级，使得调用者必须应对它

### 第十五条
函数：了解如何在闭包里使用外围作用域中的变量
假如有一份列表，其中的元素都是数值，现在要对其排序，但排序时，要把出现在某个群组内的数字，放在群组外的那些数字之前。
```py
def sort_priority(values,group):
    def helper(x):
        if x in group:#注意这里helper函数直接访问了sort_priority的参数group
            return (0,x)
        return (1,x)
    values.sort(key = helper)#注意这里把函数helper当作参数传递给了 sort函数
numbers = [8,3,1,2,5,4,7,6]
group = {2,3,5,7}
sort_priority(numbers,group)
print(numbers)
```
这个函数之所以能够正常运作，是基于下列三个原因：
1. python 支持（closure）：闭包是一种定义在某个作用域中的函数，这种函数引用了那个作用域里面的变量。helper函数之所以能够访问sort_priority的group参数，就因为它是闭包。
2. python的函数是一级对象(first-class object)，也就是说我们可以直接 引用函数、把函数赋给变量、把函数当作参数传给其他函数，并通过表达式及if语句对其进行比较和判断，等等。于是我们可以把helper这个闭包函数，传给sort方法的key参数。
3. python使用特殊的规则来比较两个元组，它首先比较各元组下标为0的对应元素，如过相等，再比较下标为1的对应元素，如果还是相等，那就继续比较下标为2的对应元素，以此类推。

当函数中引用变量时，python解释器将按如下顺序遍历各作用域，以解析该引用。
1. 当前函数的作用域
2. 任何外围作用域（例如，包含当前函数的其他函数）
3. 包含当前代码的那个模块的作用域（也叫global scope 全局作用域）
4. 内置作用域（也就是包含len及str等内置函数的那个作用域）

python3 中有一种特殊的写法，能够获取闭包内的数据。我们可以用nonlocal语句来表明这样的意图，也就是，给相关变量赋值的时候，应该在上层作用域中查找该变量。nonlocal 的唯一限制在于，它不能延申到模块级别，这是为了防止它污染全局作用域。它与global语句互为补充。global用来表示对该变量的赋值操作，将会直接修改模块作用域里的那个变量。
python2 里不支持nonlocal关键字。
python2 可以利用可变数据赋值来利用作用域规则来解决。found[0] = True。
Tips：
1. 对于定义在某作用域内的闭包来说，它可以引用这些作用域的变量。
2. 使用默认方式对闭包内的变量赋值，不会影响外围作用域中的同名变量。
3. 在python3，程序可以在闭包内用nonlocal语句来修饰某个名称，使该闭包能够修改外围作用域中的同名变量。
4. 在python2中， 程序可以使用可变值（例如，包含单个元素的列表）来实现与nonlocal语句相仿的机制。
5. 除了那种比较简单的函数，建立不要用nonlocal语句

### 第十六条
函数：考虑用生成器来改写直接返回列表的函数
如果函数要产生一系列结果，那么最简单的做法就是把这些结果都放在一份列表里，并将其返回给调用者。

```py
def index_words(text):
    result = []
    if text:
        result.append(0)
    for index, letter in enumerate(text):
        if letter  ==" ":
            result.append(index + 1)
    return result

address = "Four score and  seven years ago..."
result = index_words(address)
print(result[:3])
```

但是这样做的弊端在于，它在返回前，要把所有结果都放到列表里，如果输入量很大，那么程序可能会占用大量内存而崩溃，如果使用生成器，则可以应对任意长度的输入数据。
所以我们可以考虑使用生成器（generator）。**生成器指的是使用yield表达式的函数** 
调用生成器函数时，它并不会真的运行，而是会返回**迭代器**。
每次在这个**迭代器**上面调用内置的next函数时，**迭代器会把生成器推进到下一个yield表达式那里**。
生成器函数传给yield的每一个值，都会由迭代器返回给调用者。

```py
def index_words_iter(text):
    """
    这个函数可以跟之前的那个达到一样的目的，并且更简洁
    """
    if text:
        yield 0
    for index, letter in enumerate(text):
        if letter == " ":
            yield index +1

def index_file(handle):
    """
    依次读入各行内容，然后逐个处理每行中的单词，并产生对应的结果。该函数执行时所耗的内存，由单行输入值的最大字符数来界定。
    """
    offset = 0 
    for line in handle:
        if line:
            yield offset
        for letter in line:
            offst += 1
            if letter ==" ":
                yield offset
```

Tips：
1. 使用生成器比把收集到的结果放入列表里返回给调用者更加清晰
2. 由生成器函数所返回的那个迭代器，可以把生成器函数体中，传给yield表达式的那些值，逐次产生出来
3. 无论输出量有多大，生成器都能产生一系列输出，因为这些输入量和输出量都不会影响它在执行时所耗的内存

### 第十七条
函数：在参数上面迭代时，要多加小心
如果函数接受的参数是个对象列表，那么很有可能要在这个列表上多次迭代。
```py
def normalize(numbers):
    total = sum(numbers)
    result = []
    for value in numbers:
        # 注意这里，迭代器只能产生一轮结果。在抛出过StopIteration异常的迭代器或生成器上面继续迭代第二轮，是不会有结果的。
        print(value)
        percent = 100 * value/total
        result.append(percent)
    return result

def read_visits(data_path):
    with open(data_path) as f:
        for line in f:
            yield int(line)
# it = read_visits("Learn_python\my_numbers.txt")
# percentages = normalize(it)
# print(percentages)

def normalize_copy(numbers):
    numbers = list(numbers)# 注意这里，为了解决这个问题，我们可以明确的使用该迭代器制作一份列表。
    #但是迭代器可能含有大量输入数据，从而导致程序在复制迭代器的时候耗尽内存并崩溃
    total = sum(numbers)
    result = []
    for value in numbers:
        print(value)
        percent = 100 * value/total
        result.append(percent)
    return result
# it = read_visits("Learn_python\my_numbers.txt")
# percentages = normalize_copy(it)
# print(percentages)


class ReadVisits(object):
    def __init__(self, data_path):
        self.data_path = data_path
    def __iter__(self):
        with open(self.data_path) as f:
            for line in f:
                yield int(line)

# visits = ReadVisits("Learn_python\my_numbers.txt")
# percentages = normalize(visits)
# # normalize函数中的sum方法会调用ReadVisits.__iter__,从而得到新的迭代器对象，而调整数值所用的那个for循环，也会调用__iter__,
# print(percentages)

def normalize_defensive(numbers):
    # 判断传入的实参是否是容器
    if iter(numbers) is iter(numbers):
        raise TypeError("Must supply a container")
    total = sum(numbers)
    result = []
    for value in numbers:
        print(value)
        percent = 100 * value/total
        result.append(percent)
    return result
```
Tips
1. 函数在输入的参数上面多次迭代的时候要小心，如果参数是迭代器，那么可能会导致奇怪的行为并错失某些值。
2. Python的迭代器协议，描述了容器和迭代器应该如果与iter和next内置函数、for循环及相关表达式相互配合。
3. 把__iter__方法实现为生成器，即可定义自己的容器类型。
4. 想判断某个值是迭代器还是容器，可以拿该值为参数，两次调用iter，若结果相同，则是迭代器，调用内置的next函数，即可令该迭代器前进一步。

### 第十八条
函数： 用数量可变的位置参数减少视觉杂讯
令函数接受可选的位置参数（由于这种参数习惯上写为*args，所以又称为star args，星号参数），能够使代码更加清晰，并能减少视觉杂讯（visual noise）。
```py
def log(message, value):
    if not values:
        print(message)
    else:
        values_str = ", ".join(str(x) for x in value)
        print("%s:%s"%(message, values_str))
log("My numbers are",[1,2])
log("Hi there",[])#只想打印一条消息，调用者也必须像上面那样，即麻烦又杂乱。

def log(message, *value):#注意这里加了个*号
    if not values:
        print(message)
    else:
        values_str = ", ".join(str(x) for x in value)
        print("%s:%s"%(message, values_str))
log("My numbers are",[1,2])
log("Hi there")#这样就好多了
#如果要把已有列表传给像log这种带有变长参数的函数，那么调用的时候可以给列表前面加上*符号。
favorites = [7, 33, 99]
log("favorites colors",*favorites)

```
长度可变的位置参数有两个问题：
1. 变长参数在传给函数时，总是要先转换成元组，如果是用带有*操作符的生成器为参数，来调用函数，python就必须要先把该生成器完整的迭代一轮，这可能会消耗大量内存，引起程序崩溃。
    ```py
    def my_generator():
        for i in range(10):
            yield i 
    def my_func(*args):
        print(args)
    it = my_generator()
    my_func(*it)
    # (0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
    ```
2. 如果要添加新的位置参数，那就必须修改原先调用该函数的那些旧代码。

Tip:
1. 在def语句中使用*args，即可令函数接收数量可变的位置参数。
2. 调用函数时，可以采用*操作符，把序列中的元素当作位置参数，传给该函数
3. 对生成器使用*操作符，可能导致程序耗尽内存而崩溃，
4. 在已经接收 *args参数的函数上面添加位置参数，可能会产生难以排查的BUG。


### 第十九条
函数：用关键字参数来表达可选的行为
Tips
1. 函数参数可以按位置或者关键字来指定
2. 只使用位置参数来调用函数，可能会 导致这些参数值的含义不够明确，而关键字参数则能够阐明每个参数的意图。
3. 给函数添加新的行为时，可以使用带默认值的关键字参数，以便与原有的函数调用代码保持兼容。
4. 可选的关键字参数，总是应该以关键字形式来指定，而不应该以位置参数的形式来指定。

### 第二十条
函数：用None和文档字符串来描述具有动态默认值的参数
```python
def log(message,when = datetime.now()):
    print("%s: %s"%(when, message))

# 合适调用这个函数when参数都是一个固定的值
# 而不是想要的动态值
```


函数参数的默认值会在每个模块加载进来的时候求出，而很多模块都是在程序启动时加载的，包含这段代码的模块一旦加载尽量，参数的默认值就固定不变了。
在python若想实现动态默认值，习惯上是把 参数设为None，并在文档字符串（docstring）里面把None所对应的实际行为描述出来。

```python
def log(message,when = None):
    when = datatime.now() if when is None else when
    print("%s: %s"%(when, message))

# 合适调用这个函数when参数都是一个固定的值
# 而不是想要的动态值
```
Tips:
1. 参数的默认值，只会在程序加载模块并督导本函数的定义时评估一次，对于{}或者[]等动态的值，这可能导致奇怪的行为
2. 对于以动态值作为实际默认值的关键字参数来说，应该把形式上的默认值写为None，并在函数的文档字符串里面描述该默认值所对应的实际行为。

### 第二十一条
函数：用只能以关键字形式指定的参数来确保代码明晰
Tips：
1. 关键字参数能够使函数函数调用的意图更加明确
2. 对于各参数之间很容易混淆的函数，可以声明只能以关键字形式指定的参数，以确保调用者必须通过关键字来指定它们。对于接受多个Boolean标志的函数，更应该这样做。
3. python3 有明确的语法来定义这种只能以关键字形式指定的参数（positional_argument ,*,关键字参数）
4. python2 函数可以接受 **kwargs, 并手动抛出TypeError异常，以便模拟以关键字形式来指定的参数

### 第二十二条
类与继承：尽量用辅助类来维护程序的状态，而不要用字典和元组

如果要把许多学生的成绩记录下来：
```python
class SimpleGradebook(object):
    def __init__(self):
        self._grades = {}
    def add_student(self, name):
        self._grades[name] = []
    def report_grade(self, name, score):
        self._grades[name].append(score)
    def average_grade(self, name):
        grades = self._grades[name]
        return sum(grades)/len(grades)

book = SimpleGradebook()
book.add_student("Isaac Newton")
book.report_grade("Isaac Newton",90)
print(book.average_grade("Isaac Newton"))
```
由于字典用起来很方便，所以有可能因为功夫过分膨胀而导致代码出问题。
例如要扩充SimpleGradebook类，使它能够按照科目来保存成绩。
```python
class BySubjectGradebook(object):
    def __init__(self):
        self._grades = {} # 字典
    def add_student(self, name):
        self._grades[name] = {} # 字典存储键name的字典
        print(self._grades)
    def report_grade(self, name, subject, grade):
        by_subject = self._grades[name]
        grade_list = by_subject.setdefault(subject, [])
        grade_list.append(grade)
        print(self._grades)


book = BySubjectGradebook()
book.add_student("Isaac Newton")
book.report_grade("Isaac Newton","Math",90)
book.report_grade("Isaac Newton","Math",30)
book.report_grade("Isaac Newton","gym",30)
book.report_grade("Isaac Newton","gym",50)


```
假设现在需求又变了，还要记录各科成绩。


```python
class WeightedGradebook(object):
    def __init__(self):
        self._grades = {} # 字典
    def add_student(self, name):
        self._grades[name] = {} # 字典存储键name的字典
        print(self._grades)
    def report_grade(self, name, subject, socre, weight):
        by_subject = self._grades[name]
        grade_list = by_subject.setdefault(subject, [])
        grade_list.append(grade)
        print(self._grades)


book = BySubjectGradebook()
book.add_student("Isaac Newton")
book.report_grade("Isaac Newton","Math",90)
book.report_grade("Isaac Newton","Math",30)
book.report_grade("Isaac Newton","gym",30)
book.report_grade("Isaac Newton","gym",50)


```
假设现在需求又变了，还要记录各科每次成绩所占权重。程序就会变得非常复杂难以理解。  
这个时候我们可以考虑从字典和元组迁移到类体系了。  
>>>用来保存程序状态的数据结构一旦变得过于复杂，就应该将其拆解为类，以便提供更为明确的接口，并更好的封装数据。这样做也能够在接口与具体实现之间创建抽象层。  

Tips：
1. 不要使用包含其他字典的字典，也不要使用过长的元组。
2. 如果容器中包含简单而又不可变的数据，那么可以先使用namedtuple来表示，待稍后有需要时，再修改为完整的类。
3. 保存内部状态的字典如果变得比较复杂，那就应该把这些代码拆解为多个辅助类。



### 第二十三条
类与继承：简单的接口应该接受函数，而不是类的实例。
python 有许多内置的API，都允许调用者传入函数，以定制其行为,例如：
```py
names = ["Socrates","Archimedes","Plato","Aristotle"]
names.sort(key = lambda x: (len(x)))
print(names)


import collections
def log_missing():
    print("Key added")
    return 0
current = {"green":12,"blue":3}
increments = [("red",5),("blue",17),("orange",9)]
result = collections.defaultdict(log_missing,current)
print("before:",dict(result))
for key,amount in increments:
    result[key] += amount
print("After: ",dict(result))

```
Tips:
1. 对于连接各种Python组件的简单接口来说，通常应该给其直接传入函数，而不是先定义某个类，然后再传入该类的实例。
2. Python 中的函数和方法都可以像一级类那样引用，因此它们与其他类型的对象一样，也能够放在表达式里面。
3. 通过名为__call__的特殊方法，可以使类的实例能够像普通的Python函数那样得到调用。
4. 如果要用函数来保存状态，那就应该定义新的类，并令其实现__call__方法，而不要定义带状态的闭包。


### 第二十四条
类与继承： 以@Classmethod形式的多态取通用的构建对象


### 第二十五条
类与继承： 用super初始化父类
初始化父类的传统方法，是在子类里用子类实例直接调用父类的__init__方法。
```py
class MyBaseClass(object):
    def __init__(self,value):
        self.value = value
class MyChildClass(MyBaseClass):
    def __init__(self):
        MyBaseClass.__init__(self,5)
```
这种方法对于简单的继承体系是可行的，但是在许多情况下会出问题。
1. 多重继承影响。
如果子类受到了多重继承的影响。 那么直接调用超类的__init__方法，可能会产生无法预知的行为。
在子类调用__init__的问题之一，使它的调用顺序并不固定。
```py
class MyBaseClass(object):
    def __init__(self, value):
        self.value = value


class TimesTwo(object):
    def __init__(self):
        self.value *= 2


class PlusFive(object):
    def __init__(self):
        self.value += 5


class OneWay(MyBaseClass, TimesTwo, PlusFive):
    def __init__(self, value):
        MyBaseClass.__init__(self, value)
        TimesTwo.__init__(self)
        PlusFive.__init__(self)


class AnotherWay(MyBaseClass, PlusFive, TimesTwo):
    def __init__(self, value):
        MyBaseClass.__init__(self, value)
        TimesTwo.__init__(self)
        PlusFive.__init__(self)

foo = OneWay(5)
print("(5*2)+5 = ", foo.value)

foo = AnotherWay(5)
print("(5+5) *2 = ", foo.value)
# 原因是没有修改超类构造器的调用顺序，还是以前一样。

```


钻石型：如果子类继承自两个单独的超类，而那两个超类又继承自同一个公共基类。就构成了菱形结构也叫钻石形继承体系。
还有一个问题发生在钻石形继承之中.如果多次执行了公共基类构造器方法，从而产生意向不到的行为。
```py
class MyBaseClass(object):
    def __init__(self, value):
        self.value = value


class TimesFive(MyBaseClass):
    def __init__(self, value):
        MyBaseClass.__init__(self, value)
        self.value *= 5


class PlusTwo(MyBaseClass):
    def __init__(self, value):
        MyBaseClass.__init__(self, value)
        self.value += 2


class ThisWay(TimesFive, PlusTwo):
    def __init__(self, value):
        TimesFive.__init__(self, value)
        PlusTwo.__init__(self, value)


foo = ThisWay(5)
# 因为在调用第二个超类（PlusTwo）的构造器的时候，它会再度调用MyBaseClass.__init__,从而导致self.value重新变成5
print("(5*5)+2 = ",foo.value )
```

python2.2 增加了内置的super函数，并且定义类方法的解析顺序，（method resolution order,MRO）,以解决这一问题，MRO以标准的流程来安排超类之间的初始化顺序（深度优先，从左往右），它也保证钻石顶部的哪个公共基类的构造方法指挥运行一次。



```py

class MyBaseClass(object):
    def __init__(self, value):
        self.value = value


class TimesFiveCorrect(MyBaseClass):
    def __init__(self, value):
        super(TimesFiveCorrect, self).__init__(value)
        self.value *= 5


class PlusTwoCorrect(MyBaseClass):
    def __init__(self, value):
        super(PlusTwoCorrect, self).__init__(value)
        self.value += 2


class GoodWay(TimesFiveCorrect, PlusTwoCorrect):
    def __init__(self, value):
        super(GoodWay, self).__init__(value)
 


foo = GoodWay(5)
print("5*(5+2)=",foo.value)
#为什么是35而不是27呢？
#可以看一下这个类的MRO（method resolution order） 
from pprint import pprint
pprint(GoodWay.mro())
```

python3 可以直接忽略super参数：
`super().__init__(value)`


### 第二十六条
类与继承：只能使用Mix-in 组件制作工具类时进行多重继承

Min-in是一种小型的类，它只定义了其他类可能需要提供的一套附加方法，而不定义自己的实例属性，此外，它也不要求使用者调用自己的__init__构造器。



```py
class JsonMixin(object):
    @classmethod
    def form_json(cls,data):
        kwargs = json.loads(data)
        return cls(**kwargs)
    def to_json(self):
        return json.dumps(self.to_dict())

```
没看懂。略。

### 第二十七条
类与基础：多用Public属性，少用Private属性。
对于Python类来说，其属性的可见度只有两种，也就是Public（公开、公共的）和Private（私密、私有的）。
```py
class MyObject(object):
    def __init__(self):
        self.public_field = 5
        self.__private_field = 10#这里定义了私有属性
    def get_private_field(self):#类级别的方法仍然声明在本类的class代码块之内，所以这些方法是可以访问到类的私有属性
        return self.__private_field
foo = MyObject()
assert foo.public_field == 5#断言 
assert foo.get_private_field() == 10
foo.__private_field#这里会直接报错

```

例子2
```py
class MyOtherObject(object):
    def __init__(self):
        self.__private_field =71
    @classmethod
    def get_private_field_of_instance(cls,instance):
        return instance.__private_field
bar = MyOtherObject()
assert MyOtherObject.get_private_field_of_instance(bar) == 71

```
例子2 子类无法访问父类的private属性
```py
class MyParentObject(object):
    def __init__(self):
        self.__private_field =71
class MyChildObject(MyParentObject):
    def get_private_field(self):
        return self.__private_field
baz = MyChildObject()
baz.get_private_field()
baz._MyParentObject_private_field()
```

Python会对私有属性的名称最一些简单的变换，以保证private属性的私密性。baz.get_private_field()实际上访问的是_MyChildObject__private_field，父类的私有属性的真实名称实际上是，_MyParentObject__private_field，子类之所以无法访问父类的私有属性，只不过是因为变换后的属性名与待访问的属性名不相符而已。
了解到这套机制之后，我们就可以从任意类中访问相关类的私有属性了。无论是从该类的子类访问，还是从外部访问，我们都不受制于Private属性的访问权限。

Tip：
1. Python 编译器无法严格保证private字段的私密性
2. 不要盲目地将属性设为private，而是应该从一开始就做好规划，并允许子类更多的访问超类内部的API。
3. 应该多用protected属性，并在文档中把这些字段的合理用法告诉子类的开发者，而不要视图用private属性来限制子类访问这些字段
4. 只有当子类不受自己控制时，才可以考虑用private属性来避免名称冲突。

### 引用
https://www.cnblogs.com/lipandeng/p/11162039.html
https://lingyunfx.com/?page_id=152
https://blog.csdn.net/liuskyter/article/details/80371371