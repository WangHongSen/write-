[TOC]

# 语法基础

  - #：注释<br>
  -  \n:标准换行符<br>
  -  \：继续上一行(闭合操作符例外，例如括号）<br>
  - '''语句块''' docstring，obi.__doc__访问<br>
  - 代码组：缩进相同的一组语句
  - 字句（clause）：关键字（if、while等）以及冒号及冒号以后的的代码组
  - 建议四个空格缩进
  - 用‘；’可以同一行写多个语句
  - 增量赋值：+=，-=，等
  - 不支持自增自减
  - 链式赋值：x=y=1
  - 多元赋值：x，y，z=1，2，‘asfa’
  - 标识符：第一个字符为字母或者下划线（不建议用下划线，python开头下划线一般为自带函数或者私有变量），其余可以是字母、数字、下划线，大小写敏感
  - 关键字： 条件：if,else,for,in,while,break,return,continue;
           运算符: True,False,None;类级:class,lamdba,def,is,import,from;逻辑:and,or,not;异常:try,exception,raise,finally;其他：is，nonlocal，del，global,with,yield,assert
  - 下划线：_xxx,不用from module import 导入
           __xx__两个下划线系统定义名字
           __xxx类中的私有变量
  - 代码布局：起始行（unix标识python位置），模块文档，模块导入，变量定义，类定义，函数定义，主程序
  - del直接释放内存
  - 引用计数保持追踪内存中对象,计数增加：对象被创建,别名被创建，作为参数传给函数，成为容器对象一部份；计数减少：变量被赋值给另外一个对象（实质上不变）,del语句即别名被显式销毁，对象被移出窗口，容器，窗口本身被销毁，本地引用离开了其作用范围

# 对象
 - 特性：身份（id即内存地址，is判断是否引向同一个对象），类型，值
 - 标准类型：整形、布尔、长整型、浮点、复数、字符串、列表、元组，字典，None
 - type是所有Python标准类的默认元类（metaclass），所有Python类型的根，
 - 内部类型：代码，帧，跟踪记录，切片，省略，xrange
 - is 等价与 is(a) = is(b)
 - 标准类型内建函数：repr()机器友好，str(),type(),''运算符，isinstance()
 - 存储模型：标量/原子类型（数值，字符串），容器类型
 - 更新模型：可变类型（列表，字典），不可变类型（数字，字符串，元组）
 - 访问模型：直接访问（数字），顺序访问（序列：字符串，列表，元组），映射访问
 - Python不支持char，byte，指针

# 数字

- 支持整形、长整型、布尔型、双精度浮点、十进制浮点、复数
- 长整型取值范围与虚拟内存有关
- python3只有int无long，int用long表示
- 复数，实部和虚部用浮点数表示，实部+虚部j（J），.conjugate()返回共轭
- /一般意义的除法，保留小数位，//c类似除法，结果没有小数
- **幂
- 转换工厂函数，int(),long(),float(),complex()
- abs()，绝对值，复数则将实部和虚部平方之和开方
- coerce(a,b),两种数据类熊转换，python3已经废除
- divmod(num1,num2),返回包含商和余数的列表
- hex(num),转化成十六进制以字符串形式返回，oct(num)是八进制
- ord(chr),接受一个ASCII或者unicode字符返回编码
- chr(num),将ASCII值的数字形式转换成ASCII字符，0<=num<=255
- unichr(num),将unicode编码转换成对应的unicode字符（python3废除）
- 布尔是整形的子类，但不能被继承，无__nonezero__()方法的对象默认值是true，任意空集合类，0，空字符串布尔值为false
- 十进制浮点数，导入Decimal模块：from decimal import Decimal
- random;randRange()返回range([start],stop,[end])中一项，uniform(start,end)，小数，randint(start,end)类似uniform,choice(seq),random()返回0到1之间的一个数

# 序列

- in判断是否在序列内，返回布尔值，not in 不在
- seq[index],seq[start:end],切片包含start到n-1这些元素无seq[end]，seq[start::],start后面所有元素
- seq*num 将序列元素重复num遍形成新的序列
- seq1+seq2连接两个序列并返回合并的序列
- 字符串分为一般字符串str，unicode，basestring，basestring不能实例化，是str，unicode父类。python3无str与unicode区分
-格式化，“”%content，或者format函数（python3），“{}”.format();Template格式化，${key},用substitute，safe_substitute替换
- 前缀r或者R，原始字符串
- bif：center(width),count(str,begin,end=len(string)),len(),decode(encoding,errors),encode(encoding,errors),endsewith(),find,index,join(str),replace(),split(str,num),startswith(),strip(),title()
- '''  '''三引号所见即所得，类似前缀r
- list []；tuple(),不可变，但元组包含对象可变，单元素（element1）；dictionary{},key不可变
- 浅拷贝 创建一个类型和原对象一样，但内容是原对象元素的引用。（拷贝的对象是新的，内容不是）。序列类型对象默认类型拷贝是浅拷贝。常见：完全切片操作[:]；工厂函数如list()；copy模块的copy函数。拷贝不可变对象，创建新对象，可变，复制引用
- 深拷贝，copy.deepcopy(),创建新对象，复制后的对象id不一样；非容器类型对象无被拷贝一说；单元素元组，深拷贝不会进行

# 函数
- 无返回值，返回None；返回多个对象，自动打包成元组返回
- 多参数模拟重载，用type()区分
- func(positional_args,keyword_args,*tuple_grp_nonkw_args,**dict_grp_kw_args)
- 内部/内嵌函数，在函数体内创建的另外一个函数对象。若无对内部函数的外部应用，则除了函数内，任何地方不能对其进行调用
- @decorate(arguments) def func(): 装饰器，允许多重。带参数表示以该装是器返回绘制作为新的装饰器。装饰器装饰一个callable对象，返回一个callable对象
- 默认参数，同c++,必须指向不可变对象，如None，str
- 使用 * arg定义可变参数，函数内部表示为tuple。传入list或者tuple，前面加*
- 使用 ** kw定义关键字参数，函数内部组装为dict；同样使用 ** dict传入已有的字典
- 使用func(arg1,arg2, * ,arg3,arg4)定义命名关键字， * 后面的参数为关键字，传入时必须传入，否则失败。已有可变参数的情况下无需添加 * 
- 各种参数的组合顺序：参数，默认参数，可变参数，命名关键字参数，关键字参数`def f(a,b=2,*c,d,**kw):`
- lambda表达式，变量=lambda 变量列表（可以是*开头的列表和**开头的字典）: 表达式
- filter(过滤器（返回布尔值的函数），序列)，map(函数，多个序列参数),reduce(func,seq)(python3不是全局函数，需要引用functools包)
- global，局部作用域（函数）引用全局变量，用法gloabal，var[,var,,,],用后局部作用域不能声明同名变量
- 生成器，语法yield

# 异常
- try—catch-else-finally,except exception as(3)
- with上下文管理，简化try,except,finally关键字和资源分配相关代码，with contexr [as var]: statement
- raise字句，raise[,args,[traceback]]
- assert expression[,args],断言成功无任何显示，不成功触发AssertionError异常
- sys.exc_info(),返回一个三元组，异常类型，异常实例，traceback对象

# 模块
- 名称空间：名称（标识符）到对象的映射，sys.path访问搜索路径，sys.path.append()添加变量，sys.modules返回一个包含导入的模板和来源的相关信息的字典，模块名为键，对应物理地址为值
- python执行期间的有两到三个活动地名称空间，分别是局部名称空间，全局名称空间和内建名称空间，首先加载内建名称空间，由__builtins__模块中名字构成，然后加载全局名称空间，执行期间调用函数，就创建局部名称空间
- import导入模块，推荐风格：一个import导入一个模块，导入的顺序为Python标准库模块，Python第三方模块，程序自定义模块，至少用一个空行分割这三类模块的导入语句
- from …… import ……[as] 导入指定的模块属性（限制使用from module import *,旨在目标模块中属性太多，反复键入木块名不方便和交互解释器下使用）
- 一个模块只加载（load）一次，无论导入（import）了多少次
-  __import__(module_name[,globals[,locals[,fromlist[,level]]]]),只有第一个参数时返回module类示例，globals()返回调用者全局名称空间的字典，locals()返回局部名称空间的字典
- 使用_name的方式将变量设为私有

# 高阶函数
- map(function,list),对列表每个元素进行操作，返回一个列表。列表可是任意iterable对象
- reduce(function,list),把function迭代运用在列表上，返回一个值，从functools中导入
- filter(func,list),func是一个返回布尔值的函数，将func应用到list上，根据返回值过滤
- sorted(list,key=,reverse=),key是一个排序用的函数，数字、字符串可不用传入。reverse=True反向排序
- 对于闭包：返回函数的时候不能引用任何循环变量、任何后续会发生变化的变量。
- 装饰器，实质是用函数封装，将要装饰的函数传入装饰器函数，由装饰器提供参数，在执行了了装饰语句后，将参数传入并返回函数。实质是decorator((func).装饰器如果要传入参数，则定义装饰器的时候要使用三层封装。实质是decorator(arg)(func)。装饰过程中可使用functools.wraps工具将原始函数的__name__函数复制到wrapper函数中。示例:
```
import functools
def log(text):
   def decorator(func):
       @functools.wraps(func)
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator
```
- 偏函数。固定函数参数，例如`int2 = functools.partial(int, base=2)`，固定int的base参数为2，

# 面向对象
-  使用class定义类，是一个抽象模板，()实现继承，
- __init__实现构造器功能，对象所有方法必须传入self参数，
- 使用两个下划线__隐藏变量,函数，实现private.前后双下划线约定不可访问。通过设置getter、setter方法获取隐藏变量，或者通过_classname__attributename访问
- hasattr(),getattr(),setattr()等内置方法可以操纵对象属性
- 通过self绑定实例变量，通过类定义直接定义初始化类变量，类变量被所有实例共享
- 用__slots__限制添加位置参数
- 使用@property将函数func转变为属性，如进一步使用@func.setter装饰setter则该属性是可读写，否则只可读
- __str __ ,__repr__定义函数的tostring方法，__repr__为调试服务。__iter __，__next __,使对象可以变量可迭代，StopIteration()方法停止迭代。__getitem __方法实现下标，可实现切片.__getattr __实现添加备用属性，调用某某属性失败时使用该函数。 __call __使实例可调用
- 枚举类，Enum,默认从零开始技术，使用Enum.__members __items()返回可迭代对象
- 使用type(name ,(parent,),dict)创建名为name的新类，parent是要继承的父类，dict是要绑定的函数字典

# 调试
- 使用assert condition，message，condition不满足产生assertError
- 使用logging模块
- 使用python -m pdb file 进入pdb调试，在文件中加入pdb.set_trace()方法设置断点
- 单元测试，使用unittest模块，自定义的测试类要继承unittest.TestCase类，以test开头的为测试方法，常用判断方法asserEqual，assertRaise，setUp，tearDown方法实现测试前后的预处理和扫尾工作。

# IO
- with as,自动关闭资源
- file_like对象，实现了read方法
- open方法中，模式为wrb组合，encoding指定编码方式，errors指定编码错误时的处理方式
- StringIO，ByteIO在内存中读写，
