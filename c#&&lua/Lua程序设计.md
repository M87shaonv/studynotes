未学：第10章URL编码及以后的示例

13章位和字节

# Lua语言基础

一组命令或表达式组成的序列叫chunk程序段，因为Lua语言可以被用作数据定义语言，chunk的大小没有限制，几MB的程序段也很常见，Lua语言的解释器可以支持非常大的程序段

在Lua交互模式下运行`>dofile (文件名)`即可运行指定文件

**词法规范**

+ Lua语言中的标识符或名称是由任意字母、数字和下划线组成的字符串(不能以数字开头)
+ _+大写字母组成的标识符通常用作特殊用途，避免用作其他用途
+ Lua对大小写敏感
+ `--`单行注释
+ `--[[...]]`多行注释

```lua
注释代码技巧
--[[
print(12)
--]]
当需要重新使用时，只需行首添加一个连字符
---[[
print(12)
--]]
```

Lua中，连续语句之间的分隔符不是不需的，如果需要可以用分号来分隔

```lua
--以下程序段都合法且等价
a=1
b=a*2

a=1;
b=a*2

a=1;b=a*2;
```

***全局变量***

Lua中的global variable无须声明即可使用，使用未经初始化的全局变量不会导致错误，未初始化的全局变量值为`nil`

当把`nil`赋给全局变量时，Lua会回收该全局变量

Lua语言不区分未初始化变量和被赋值为nil的变量

***类型和值***

8种基本类型：

+ nil 空
+ boolean 布尔
+ number 数值
+ string 字符串
+ userdata 用户数据
+ function 函数
+ thread 线程
+ table 表

使用`type(x)`函数可以获取任意`x`变量的值，变量没有预定义的类型，任何变量都可以包含任何类型的值

***nil***

用于与其他所有值进行区分，Lua使用`nil`来表示无效值

***Boolean***

在Lua语言中，Boolean值并非是用于条件测试的唯一方式，任何值都可以表示条件

**条件测试将除Boolean值`false`和`nil`外的所有其他值视为真，0和空字符串也视为真**

```lua
--and：左操作数为假返回左数，为真返回右数
4 and 5 --> 5
nil and 6 --> nil

--or：左操作数为假返回右数，为真返回左数
0 or 5 --> 5
false or "Hi" --> "Hi"

在Lua语言中，形如x=x or v的惯用写法非常有用，它等价于：
if not x then x = v end
--x为假时设为v
(x>y) and x or y
--选出x和y中大的一个

--not：取反
not nil -->true
not 0 -->false
not not 1 -->true
not not nil -->false
```

***独立解释器***

如果源代码文件第一行以#开头，那么解释器在加载该文件时会忽略这一行，这个特征是为了方便做脚本，比如shell脚本

# 数值

```lua
integer --64位整型
float --双精度浮点型，Lua中不是单精度，但精简Lua有
```

可以使用科学计数法书写数值常量

```lua
4.57e-3 -->0.00457
5E+4 -->50000.0
```

具有十进制小数或者指数的数值会被当作浮点型值，否则会被当作整型值，但它们的类型都是number，因此它们是可以相互转换的，具有相同算术值的整型值和浮点型值在Lua语言中也是相等的

```lua
--在少数情况下需要区分整型值和浮点型值时，可以使用函数
math.type(3) -->integer
```

Lua支持`0x`开头的16进制常量，还支持16进制的浮点数，浮点数由小数部分和以p或P开头的指数部分组成

```lua
0xff -->255
0x0.2 -->0.125
0xa.bp2 -->42.75

string.format("%a",42.75) --转换为16进制
```

**虽然这种格式很难阅读，但是这种格式可以保留所有浮点数的精度，并且比十进制的转换速度更快**

**算术运算**

如果两个操作数都是整型值，那么结果也是整型值；否则结果就是浮点型值，会在进行算术运算前先将整型值转换为浮点型值

```lua
1*2 -->2
1*2.0 -->2.0
```

为了避免不能整除导致两个整型值相除和两个浮点型值相除不一样的结果，除法运算操作的永远是浮点数且产生浮点型值的结果

```lua
3.0/2.0 -->1.5
3/2 -->1.5
--floor除法会对得到的商向负无穷取整，从而保证结果是一个整数
3.0//2 -->1.0
-9//2 -->-5
-21//8 -->-3

--%取模运算结果的符号永远与第2个操作数的符号保持一致
--支持幂运算，使用符号^表示，幂运算的操作数也永远是浮点类型
```

**关系运算**

```lua
< <= > >= 
== --相等
~= --不等
-- 比较数值时应永远忽略数值的子类型，数值究竟是以整型还是浮点型类型表示并无区别，只与算术值有关
--但比较相同类型的数值时效率更高
```

## 位运算

```lua
& 按位与
| 按位或
~ 按位异或 一元则是取反，要注意^在Lua中不是异或，是幂运算
>> 按位左移
<< 按位右移
两个移位操作都会使用0来填充空出的位，这种行为称为逻辑移位
print(string.format("%x",12<<1)) --12*2
print(string.format("%x",12>>1)) --12/2
```

**数学库**

Lua语言提供了标准数学库`math`。标准数学库由一组标准的数学函数组成，包括三角函数（sin、cos、tan、asin等）、指数函数、取整函数、最大和最小函数`max`和`min`、用于生成伪随机数的伪随机数函数`random`以及常量`pi`和`huge`（最大可表示数值，在大多数平台上代表inf ）

**随机数发生器**

`math.random()`函数用于生成伪随机数

```lua
--返回一个在[0,1)范围内均匀分布的伪随机数
math.random()
--返回一个在[min,max]之间的随机数
--只带一个数字调用，返回[1,n)范围内的随机数
math.random(min,max)
--使用当前时间作为随机数生成器的种子
math.randomseed(os.time()) 
```

**取整函数**

```lua
--向负无穷取整
math.floor()
--向正无穷取整
math.ceil()
--向零取整
math.modf()
函数modf会返回小数部分作为第二个结果
math.modf(-3.3) -->-3  -0.3
```

**表示范围**

Lua使用64bits来存储整型值，其最大值为2^63^-1，约等于10^19^

`math.maxinteger`和`math.mininteger`定义了整型值的最大和最小值

双精度浮点数可范围从-2^53^到2^53^

```LUA
--可以通过增加0.0将整型值强制转换为浮点型值
-3 + 0.0 -->-3.0
--通过与零进行按位或运算，可以把浮点型值强制转换为整型值
2.2 | 0 --error，对小数进行取整必须显式地调用取整函数
2^53 | 0 --幂运算操作数是浮点型

--使用函数把数值强制转换为整型值
math.tointeger(5.01) --如果无法转换返回nil
```

## 运算符优先级

从高到低依次为：

![](https://img2024.cnblogs.com/blog/3357226/202402/3357226-20240206202545829-1268834126.png)

二元运算符中，只有幂运算和连接操作符是右结合

# 字符串

Lua中的字符串是不可变值，不能直接改变某个字符串中的某个字符，可以通过创建一个新字符串的方式来达到修改的目的

和其他对象（表、函数等）一样，Lua中的字符串也是自动内存管理的对象之一。这意味着Lua语言会负责字符串的分配和释放，开发人员无须关注

```lua
--长度操作符#
a = "Hello"
print(#a)
该操作符返回字符串占用的字节数，在某些编码中，这个值可能与字符串中字符的个数不同

--连接操作符..
"Hello" .. "Lua" -->Hello Lua
3 .. 4 -->34
连接字符串，如果操作数中存在数值，那么会先把数值转换成字符串
```

在Lua中，字符串是不可变量，字符串连接是创建一个新字符串，而不会改变原来作为操作数的字符串

**字符串常量**

```lua
--使用一对双引号或单引号来声明字符串常量
a = "a line"
使用双引号和单引号声明字符串是等价的
它们两者唯一的区别在于，使用双引号声明的字符串中出现单引号时，单引号可以不用转义；使用单引号声明的字符串中出现双引号时，双引号可以不用转义
```

Lua语言中的字符串支持下列C语言风格的转义字符

![](https://img2024.cnblogs.com/blog/3357226/202402/3357226-20240206210025404-369988809.png)

## 多行字符串

```lua
--使用方括号声明多行字符串常量
page = [[
<html>
 <head>
	<title>An HTML Page</title>
 </head>
 <body>
	<a href="http://www. lua. org">Lua</a>
 </body>
</htm1>
]]
write(page)

--字符串中可能有类似a=b[c[i]]这样的代码
--可以在两个左方括号之间加上任意数量的等号，字符串常量只有在遇到了包含相同数量等号的两个右方括号时才会结束
Test=[==[
a=b[c[i]]
]==]

--使用\z可以会跳过其后的所有空白字符，直到遇到第一个非空白字符
data= "是\Z		否" -->是否
```

## 强制类型转换

算术运算的规则就是只有在两个操作数都是整型值时结果才是整型

字符串不是整型值，所以任何有字符串参与的算术运算都会被当作浮点运算处理

```Lua
--显式字符串转换为数值
tonumber("字符串",n进制) --2进制到36进制之间的任意进制
tonumber("10010",2) --转换为2进制18
无法转化返回nil

--显式数值转换为字符串
tostring(数值)
tostring(12e+3) -->12000.0
```

**与算术操作不同，比较操作符不会对操作数进行强制类型转换**

0和"0"是不同的，2<15为真，但"2"<"15"却为假 （字母顺序），为了避免出现不一致的结果，当比较操作符中混用了字符串和数值会抛出异常

## 字符串标准库

Lua语言解释器本身处理字符串的能力十分有限，Lua语言处理字符串的完整能力来自其字符串标准库

```Lua
--返回字符串s的长度，等价于#s
string.len(s)

--返回字符串s重复n次的结果
string.rep(s,n)
string.rep("a",2^20) --2的20次方个a组成的字符串

--反转字符串s
string.reverse(s)
对于非常长的字符串，反转操作会占用较多的内存和处理器时间，因为它需要创建一个与原字符串等长的新字符串，并将字符逐个复制到新位置

--将字符串s中的大写字母转换为小写字母
string.lower(s)
--将字符串s中的小写字母转换为大写字母
string.upper(s)

--从字符串s中提取第i个到第j个字符，j为可选参数
--字符串索引起始为1，索引-1代表字符串的最后1个字符
string.sub(s,i,j)

--接受n个整数作为参数，转换为对应的ASCLL码值，返回组成的字符串
string.char(n1,...)
utf8.char()在UTF-8环境下等价于该函数

--获取字符串s中i位置字符的ASCII码值，索引可以是多个
string.byte(s,i1,...)
utf8.codepoint()在UTF-8环境下等价于该函数，返回的索引是以字节为单位，使用的索引可以是负值

--格式化字符串，格式化字符串中的占位符与C语言中函数printf的规则类似
string.format()
string.format("x=%x",200) -->返回200的16进制c8
string.format("%s is %5d years old.",name,age)

--在字符串s中进行模式搜索
string.find(s,pattern)
找到返回模式的开始和结束位置，否则返回nil

--在字符串中查找匹配i的地方，使用j进行替换
string.gsub(s,i,j)
该函数会在第2个返回值中返回发生替换的次数

--返回字符串s中UTF-8字符个数
utf8.len(s)

--遍历UTF-8字符串s中的每一个字符
utf8.codes(s)
```

# 表

table是Lua语言中最主要和强大的数据结构

使用表，Lua可以以一种简单、统一且高效的方式表示数组、集合、记录和其他很多数据结构，也使用表来表示包和其他对象

当调用函数`math.sin`时，我们可能认为是 “调用了`math`库中函数`sin`”；而对于Lua语言来说，其实际含义是 “以字符串`sin`为键检索表`math`”

Lua中的表本质上是一种辅助数组，这种数组不仅可以使用数值作为索引，也可以使用字符串或其他任意类型的值作为索引(nil除外)

表要么是值要么是变量，它们都是对象，可以认为，表是一种动态分配的对象，程序只能操作指向表的引用或指针，除此以外，Lua不会进行隐藏的拷贝或创建新的表

```lua
--构造器表达式创建新表
A = {} 
I = "x"
A[I] = 10 --new element，key=x，value=10
A[20] = "great" --new element，key=20，value=great
R = A["x"] -->R=10

--表永远是匿名的，表本身和保存表的变量之间没有固定的关系
a = {}
a["x"] = 10
b = a --b和a引用同一张表
b["x"] -->10
b["X"] = 20
a["x"] -->20
a = nil --b仍指向表
b = nil --没有指向该表的引用了
对于一个表而言，当程序中不再有指向它的引用时，垃圾收集器会最终删除这个表并重用其占用的内存
```

## 表索引

同一个表中存储的值可以具有不同的类型索引，并可以按需增长以容纳新的元素

```lua
a = {}
for i = 1, 1000 do a[i] = i*2 end
a[9] -->
a["x"] = 10
a["x"] -->10
a["y"] -->nil
Lua实际上就是使用表来存储全局变量，未经初始化的表元素为nil，将nil赋值给表元素可以将其删除

--当把表当作结构体使用时，可以把索引当作成员名称使用
--a.name == a["name"]
a.x = 10
a.x
a.y

a.x代表由字符串x索引的表
a[x]代表由变量x对应的值索引的表
```

虽然确实能用数字0和字符串"0"对同一个表进行索引，但这两个索引的值及其所对应的元素是不同的。同样，字符串"+1"、"01"和"1"指向的也是不同的元素。**当不能确定表索引的真实数据类型时，使用显式的类型转换**

**如果不注意这一点，就会很容易在程序中引入诡异的Bug**

整型和浮点型类型的表索引则不存在上述问题。由于2和2.0的值相等，所以当它们被当作表索引使用时指向的是同一个表元素

当被用作表索引时，任何能够被转换为整型的浮点数都会被转换成整型数，不能被转换为整型数的浮点数则不会发生上述的类型转换

```lua
A = {} 
I = tonumber("2.2") --字符串转为数值 I=2.2
A[I] = 10
R = A[I]
```

## 表构造器

表构造器是用来创建和初始化表的表达式，是Lua独有的也是最有用、最灵活的机制之一

```lua
--列表式语法
days = {"Sunday", "Nonday", "Tuesday", "Wednesday",
"Thursday", "Friday", "Saturday"} --构造器索引从1开始
print(days[4]) -->Wednesday

--记录式语法
a = {x = 10,y = 20}
等价于：
a = {}; a.x = 10; a.y = 20
不过在第一种写法中，由于能够提前判断表的大小，所以运行速度更快

--在同一个构造器中可以混用列表式和记录式写法
polyline = {color="blue"
    thickness=2, 
    npoints=4,
    {x=0, y=0},  --polyline[1]
    {x=-10,V=0}, --polyline[2]
    {x=-10,y=1}, --polyline[3]
    {X=0,y=13},  --polyline[4]
    }
创建了嵌套表和构造器以表达更加复杂的数据结构，每一个polyline[i]都是代表一条记录的表
print(polyline[2].x) -->-10
print(polyline[4].y) -->13
--要注意，使用这两种构造器时，不能使用负数索引初始化表元素，也不能使用不符合规范的标识符作为索引

--对于这类需求，可以使用另一种更加通用的构造器，即通过方括号括起来的表达式显式地指定每一个索引
opnames = {["+"] = "add",["-"] = "sub"}
这种构造器虽然冗长，但非常灵活，不管是记录式构造器还是列表式构造器均是其特殊形式
{x = 0, y = 0} 等价于：{["x"] = 0, ["y"] = 0,}
{"r", "g", "b"} 等价于：{[1] = "r", [2] = "g", [3] = "b",}
--最后一个逗号是否加上是可选的

```

## 数组、列表和序列

如果想表示常见的array或list，只需要使整型作为索引的表即可，不需要预先声明表的大小，只需要直接初始化需要的元素

```lua
--读取10行，然后保存在一个表中
a = {}
for i = 1, 10 
do 
    a[i] = io.read()
end
--Lua中数组索引按照惯例是从1开始的，Lua中其他很多机制也遵循这个惯例
```

当操作列表时，往往必须事先获取列表的长度。列表的长度可以存放在常量中，也可以存放在其他变量或数据结构中。通常把列表的长度保存在表中某个非数值类型的字段中

列表的长度经常也是隐式的。由于未初始化的元素均为`nil`，所以可以利用`nil`值来标记列表的结束

```lua
--Lua提供了获取序列长度的操作符#
对于字符串，该操作符返回字符串的字节数
对于表，该操作符返回表对应序列的长度
用此方法输出上例读入内容
for i = 1, #a do
    print(a[i])
    end
a[#a + 1] = v --把v加到序列的最后
对于中间存在nil的列表，长度操作符不可靠

a = {10,20,30,nil,nil}
对Lua来说，一个为nil的字段和一个不存在的元素没有区别，该表长度为3

```

## 遍历表

```Lua
--pairs迭代器会遍历表中所有元素，无论索引是否连续
t = {10, print,x = 12, "hi"}
for k, v in pairs(t) 
	do print(k, v)
end
--> 1 10 
--> 3 hi 
--> 2 function: 0x420610 
--> X 12 
受限于Lua中表的底层实现机制，遍历过程中元素的出现顺序是随机的，相同的程序在每次运行时也可能产生不同的顺序，但遍历过程中每个元素只会出现1次

--ipairs迭代器遍历表，仅适用于连续索引的列表
t = {10, print, 12, "hi"}
for k, v in ipairs(t) 
	do print(k, v)
end
--> 1 10
--> 2 function: 0x420610
--> 3 12
--> 4 hi
此时Lua会确保遍历是按照顺序进行的

--数值型for循环
t = {10, print,12, "hi"}
for k = 1, #t
	do print(k, t[k])
end 
--> 1 10
--> 2 function: 0x420610
--> 3 12
--> 4 hi
```

**安全访问**

```lua
Lua不提供安全访问操作符，可以使用其他语句模拟安全访问操作符
E = {} --可以在其他类似表达式中复用
zip = (((company or E).director or E).address or E).zipcode
--从company对象中获取director的address的zipcode。如果在任意点遇到nil或false，它将停止并返回E
```

## 表标准库

```Lua
--在表t中的j位置插入i，不指定j则默认在最后位置插入i
table.insert(t,i,j)
table.insert(t,3,15) --在表t的3位置插入15

--移除t表的i位置上的元素，返回被移除元素的值，然后将其后的元素向前移动填充删除元素后造成的空洞，不指定i则默认删除最后一个元素
table.remove(t,i)
```

借助这两个函数可以很容易地实现stack、queue、double queue

table标准库中的这些函数是使用C语言实现的，所以移动元素所涉及循环的性能开销也并不是太昂贵。因而对于几百个元素组成的小数组来说这种实现已经足够

```lua
--将表t中从i到j索引上的元素(包含i和j)移动到位置n
table.move(t,i,j,n)
它不会返回任何值，直接修改了传入的表。此外table.move会覆盖表中的元素，因此在使用它时需要小心，确保不会意外地丢失重要数据
table.move(a,1,#a,2)
--a表所有元素向后移动1位
table.move(a,2,#a,1)
--a表2到end元素移动到1位置
计算机领域中，move实际上是将一个值从一个地方copy到另一个地方，因此需要在移动后显式删除原来的值

--table.move()还支持使用一个表作为可选的参数
--将t表中i到j的元素移动到y表的n位置
table.move(t,i,j,n,y)
```

# 函数

function是对语句和表达式进行抽象的主要方式

函数调用时都需要使用一对圆括号把参数列表括起来。即使被调用的函数不需要参数，也需要一对空括号()

**唯一的例外就是：当函数只有一个参数且该参数是字符串常量或表构造器时，括号是可选的**

```lua
print "hello world"      -- print("hello world")
-dofile 'a.lua'          -- dofile('a.lua')
print [[a line message]] --print([[a line message]])
f { x = 10, y = 20 }     -- f({x=10,y=20})
type {}                  --type({})
它们都是等价的
```

Lua程序可以调研Lua编写的函数，也可以调用C语言或宿主程序使用的其他任意语言编写的函数

Lua语言标准库中所有的函数就都是使用C语言编写的。不过无论一个函数是用Lua编写的还是C编写的，在调用它们时都没有任何区别

一个函数定义具有函数名、参数列表、函数体

```lua
function add(a) --对序列a中的元素求和
    local sum = 0
    for i = 1,#a do
        sum = sum + a[i]
	end
    return sum
end    
```

> 调用函数时使用的参数个数可以与定义函数时使用的参数个数不一致。Lua语言会通过抛弃多余参数和将不足的参数设为`nil`的方式来调整参数的个数

虽然这种行为可能导致编程错误，但是有用的，比如默认参数的情况：

```lua
function incrCount(n) --递增全局计数器的函数
    n = n or 1 
--调用无参数函数时，参数n初始化为nil，or表达式返回第2个参数
    globalCounter = globalCounter + n
end    
```

## 多返回值

Lua的一个特性是允许一个函数返回多个结果

```lua
function maximum(a)
    local max = 1 --最大值索引
    local m = a[max] --最大值
    for i = 1, #a do
        if a[i] > m then
            max = i;m = a[i]
		end
	end
    return m,max --返回最大值及其索引
end    
```

**Lua根据函数的被调用情况调整返回值的数量：**

+ 当函数作为一条单独语句调用时，所有返回值都会丢弃

+ 当函数作为表达式调用时，只保留第一个返回值

+ **只有函数调用是是一系列表达式中的最后一个表达式或者唯一一个表达式时，所有的返回值才能被获取到**

  一系列表达式在Lua中表现为4种情况：多重赋值、函数调用时传入的实参列表、表构造器、return语句

```lua
function maximum(A)
    local max = 1    --最大值索引
    local m = A[max] --最大值
    for i = 2, #A do
        if A[i] > m then
            max = i; m = A[i]
        end
    end
    return m, max --返回最大值及其索引
end
A = { 1, 2, 3, 4, 5, 20, 7, 8, 9, 10 }
local n,i=maximum(A) --这个吊
print(maximum(A))
print(n,i)
```

在多重赋值中，如果一个函数调用是一系列表达式中的最后或者是唯一的一个表达式，则该函数调用将产生尽可能多的返回值以匹配待赋值变量

在多重赋值中，如果一个函数没有返回值或者返回值个数不够多，Lua使用`nil`来补充缺失的值

```lua
function test() return "a","b" end
x,y = test()      --x="a",x="b"
x = test()        --x="a","b"被丢弃
x,y,z = 10,test() --x=10,y="a",z="b"
x,y,z = test()    --x="a",y="b",z=nil
```

当一个函数调用是另一个函数调用的最后一个或者是唯一实参时，第一个函数的所有返回值都会被作为实参传给第二个函数

```lua
print(test()) --返回test的所有返回值
print(test(),"x") --在表达式中调用，只返回第一个返回值：ax 
当调用f(g())时，如果f的参数是固定的，那么Lua会把g返回值的个数调整为与f的参数个数一致，这就是多重赋值的逻辑

--函数调用时用一对圆括号括起来可以强制其只返回一个结果
--形如return f()的语句会返回f返回的所有结果值
print( (test()) )
return (test()) --强制只返回一个值
```

表构造器会完整地接收函数调用的所有返回值，而不会调整返回值的个数，不过这种行为只有当函数调用是表达式列表中的最后一个时才有效，在其他位置上的函数调用总是只返回一个结果

```Lua
--假设函数名就是返回值个数
t = {zero()} --t = {}
t = {one()}  --t = {1}
t = {two()}  --t = {1,2}

t = {zero(),one(),2} --t[1]=nil,t[2]=1,t[3]=4
```

## 可变长参数函数

Lua的函数可以支持数量可变的参数，`...`表示参数是可变长的，当该函数被调用时，Lua内部会把它的所有参数收集起来，称为额外参数

当函数要访问这些参数时需要用到`...`，此时`...`作为一个表达式来使用

```lua
function add(...)
    local s = 0
    for _, v in ipairs { ... } do 
        --因为只关心值，因此使用_来忽略索引
        s = s + v
    end
    return s
end
print(add(3, 5, 7))   
```

将`...`组成的表达式称为可变长参数表达式，其行为类似于一个具有多个返回值的函数，返回的是当前函数的所有可变长参数

```lua
function reversed(...) --逆序输出
    local a = {...}
    for i = #a, 1,-1 do 
        --从索引1开始循环，每次步长-1，向后移动一个位置
        print(a[i])
    end
end
reversed(1, 2, 3)
--可以通过变长参数来模拟Lua的参数传递机制

function test(index,number,...)
具有可变长参数的函数也可以具有任意数量的固定参数，但固定参数必须放在变长参数之前,Lua会将前面的参数赋给固定参数，然后将剩余参数作为可变长参数
```

要遍历可变长参数，函数可以使用表达式`{...}`将可变长参数放在一个表中，就像`reversed`函数一样，但如果可变长参数中包含无效的`nil`，那么`{...}`获得的表可能不是一个有效序列

此时无法在表中判断原始参数究竟是不是以`nil`，对于这种情况，Lua提供函数`table.pack`，该函数像表达式`{...}`一样保存所有的参数，然后将其放在一个表中返回，但该表还有一个保存参数个数的额外字段`n`

```lua
function nonils(...) --检测参数中是否有nil
    local arg = table.pack(...)
    for i = 1,arg.n do
        if arg[i] == nil then return false end
	end
    return true
end
print(nonils(2,nil,3)) --false
print(nonils()) --true 空和nil是不同的

--函数select遍历可变长参数函数，函数返回第n个参数后的所有参数
select(n,...)
print(select(1, "a", "b", "c")) --a b c
print(select(2, "a", "b", "c")) --b c
print(select(3, "a", "b", "c")) --c
print(select("#", "a", "b","c"))--3 #返回额外参数的总数

--对于参数较少情况，更快的add函数版本
function add(...)
    local s = 0
    for i = 1,select("#",...) do
        s = s + select(i,...)
	end
    return s
end
因为该版本避免了每次调用时创建一个新表。不过对于参数较多的情况，多次带有很多参数调用函数select会超过创建表的开销，因此第一个版本会更好
```

多重返回值还还涉及一个特殊的函数`table.unpack`，该函数的参数是一个数组，返回值为数组内的所有元素

> **`table.pack`把参数列表转换成Lua语言中一个真实的列表（一个表），而 `table.unpack`则把Lua语言中的真实的列表（一个表）转换成一组返回值，进而可以作为另一个函数的参数被使用**

```Lua
print(table.unpack[10,20,30}) -->10 20 30
a,b = table.unpack{10,20,30} --a=10,b=20,30被丢弃
```

`unpack`函数的重要用途之一体现在泛型调用机制中，泛型调用机制允许我们动态地调用具有任意参数的任意函数

例如在ISO C中，无法编写泛型调用的代码，只能声明可变长参数的函数（使用stdarg.h）或使用函数指针来调用不同的函数

但是仍然不能调用具有可变数量参数的函数，因为C语言中的每一个函数调用的实参个数是固定的，并且每个实参的类型也是固定的。 而在Lua语言中，却可以做到这一点

```lua
--通过数组a传入可变长参数来调用函数f
f(table.unpack(a))
unpack会返回a中所有的元素，而这些元素又被用作f的参数

print(string.find("hello","l"))
--可以使用如下的代码动态地构造一个等价的调用
f = string.find
a = {"hello","l"}
print(f(table.unpack(a)))

table.unpack函数本身并不直接获取表的长度，通常使用长度操作符获取返回值的个数，因此该函数只能用于序列，如果有需要，可以显式限制返回元素的范围
print(table.unpack({"Sun","Mon","Tue","wed"},2,3))
--> Mon Tue
```

预定义的函数`unpack`是用C语言编写的，但是可以利用递归在Lua语言中实现

```lua
function unpack(t,i,n) --n为序列长度，注意调用函数时使用的参数个数可以与定义函数时使用的参数个数不一致。Lua语言会通过抛弃多余参数和将不足的参数设为`nil`的方式来调整参数的个数
    i = i or 1
    n = n or #t
    if i <= n then
        return t[i],unpack(t,i+1,n)
	end
end
第一次调用该函数时，只传入一个参数，此时i为1
然后函数返回t[1]及unpack(t,2,n)返回的所有结果
unpack(t,2,n)又会返回t[2]及unpack(t,3,n)返回的所有结果
依此类推，直到处理完n个元素为止
```

## 正确的尾调用

Lua支持尾调用消除，这意味着Lua可以正确地尾递归

当一个函数的最后一个动作是调用另一个函数而没有再进行其他工作时就形成了尾递归

```lua
function f(x)
    x = x + 1
    return g(x)
end
被调用函数执行结束后，程序不会返回最初的调用者，因此尾调用之后，程序不需要在调用栈中保存有关调用函数的任何信息
当g返回时，程序执行路径会直接返回到调用f的位置
利用了这个特点，使得在进行尾调用时不使用任何额外的栈空间，这就是尾调用消除 tail-call elimination 

--因为尾调用不会使用栈空间，所以一个程序中能够嵌套的尾调用的数量是无限的
function tail(n)
    if n > 0 then
        return tail(n-1)
	end
该函数永远不会发生栈溢出

--以下所有调用都不符合尾调用的定义
return g(x) + 1 --必须进行加法
return x or g(x)--必须把返回值限制为1个
return (g(x))   --必须把返回值限制为1个
    
在Lua中，只有形如return function(arguments)的调用才是尾调用
不过Lua会在调用前对函数及其参数求值，因此函数及其参数都可以是复杂的表达式
return x[i].tail(x[j] + a * b,i + j)
```

# 输入输出

Lua语言强调可移植性和嵌入性，所以Lua本身并没有提供太多与外部交互的机制，Lua只提供ISO C语言标准支持的功能

在Lua程序中，从图形、数据库到网络的访问等大多数I/O操作，要么由宿主程序实现，要么通过不包括在发行版中的外部库实现

## 简单I/O模型

简单模型虚拟一个当前输入流和一个当前输出流，其I/O操作是通过这些流实现的。I/O库把当前输入流初始化为进程的标准输入(C中的`stdin`)，把当前输出流初始化为进程的标准输出(C中的`stdout`)

```lua
--以只读模式打开指定文件，并将文件设置为当前输入流
io.input(file)
io.output(file)

--写入当前输入流
io.write()
--..字符串连接操作符
调用该函数时可以使用多个参数，因此应该避免使用io.write(a..b..c)，应该调用io.write(a,b,c)，后者可以用更少的资源达到同样的效果，并且可以避免更多的连接动作
```

作为原则，应该只在用后即弃的代码或调试代码中使用`print`，当需要完全控制输出时，应该使用函数`io.write`。`print`会在每个参数之后自动添加一个空格，并在所有参数输出之后添加一个换行符，`io.write`不会在最终的输出结果中添加制表符或换行符这样的额外内容

此外，`print`只能使用标准输出，而`io.write`允许对输出进行重定向

`print`会自动调用`tostring`函数来确保它的参数被转换为字符串形式，这一点对于调试而言非常便利，但这也容易导致一些诡异的Bug

`io.write`在将数值转换为字符串时遵循一般的转换规则，如果想要完全地控制这种转换，则应该使用函数`string.format`

```Lua
io.write(string.format("sin(3) = %.4f\n",math.sin(3)))
math.sin的结果将替换%.4f，生成格式化后字符串，io.write输出字符串到标准输出，并在末尾添加一个换行符
```

`io.read`可以从当前输入流中读取字符串，其参数决定了要读取的数据

```Lua
"a" 从当前位置开始读取输入文件的全部内容，如果当前位置处于文件的末尾或文件为空，那么该函数返回一个空字符串
"l" 读取下一行，丢弃换行符，是read的默认参数
"L" 读取下一行，保留换行符
"n" 读取一个数值
num 以字符串读取num个字符

--Lua可以高效处理长字符串，编写过滤器的一种简单技巧就是将整个文件读取到一个字符串中，然后对字符串进行处理
t = io.read("a")
t = string.gsub(t,"bad","good") --将t中bad替换为good
io.write(t)

--将当前输入复制到当前输出中的同时对每行进行编号
for count = 1,math.huge do --循环条件永远为真
    local line = io.read("L")
    if line == nil then break end --停止循环
    io.write(string.format("%6d",count),line)
end

--不过逐行迭代一个文件，使用io.lines迭代器会更简单
它返回一个迭代器，该迭代器每次调用时都会返回文件中的下一行。当文件中的所有行都被读取后，迭代器将返回nil
local count = 0
for line in io.line() do
    count = count + 1
    io.write(string.format("%6d",count),line,"\n")
end   

--对文件进行排序
local lines = {}
--将所有行读取到表lines中
for line in io.lines() do
    lines[#lines + 1] = line
end
--排序
table.sort(lines)
--输出所有行
for _,l in ipairs(lines) do
    io.write(l,"\n")
end

--在调用函数read时还可以用一个数字n作为其参数
在这种情况下，read会从输入流中读取n个字符。如果无法读取到任何字符则返回nil；否则则返回一个由流中最多n个字符组成的字符串
--将文件从stdin复制到stdout
while true do
    local block = io.read(2^13) --2的13次方
    if not block then break end 
    io.write(block)
end    
io.read(0)是一个特例，常用于测试是否到达了文件末尾，如果有数据可供读取，返回一个空字符串，否则返回nil

--假设有一个每行由3个数字组成的文件，调用read来一次性同时读取每行中的3个数字，然后打印每行的最大值
while true do
    local n1,n2,n3 = io.read("n","n","n")
    if not n1 then break end
    print(math.max(n1,n2,n3))
end
```

## 完整I/O模型

对于同时读写多个文件等更高级的文件操作就需要使用完整I/O模型

```Lua
--打开文件，第一个参数是文件路径和名称，第二个参数是打开文件的模式，都需要使用引号括起来
"r"：只读
"w"：写入，文件存在清空文件内容，不存在则创建新文件
"a"：追加，文件存在在文件末尾追加内容，不存在则创建新文件
"r+"：读写
"w+"：读写，文件存在清空文件内容，不存在则创建新文件
"a+"：读写，文件存在在文件末尾追加内容，不存在则创建新文件

--在Lua中，assert用于进行条件测试。给定条件为真，无任何动作；给定条件为假，assert会抛出一个错误并显示一个可选的错误消息
assert(condition, errorMessage)
errorMessage：可选参数，condition为假时，将作为错误信息显示

--检查错误的一种典型方法是使用函数assert
local f = assert(io.open(file,mode))
local t = f:read("a")
f:close()
如果io.open执行失败，错误信息会作为assert的第二个参数被传入，之后assert会将错误信息展示出来
打开文件后使用read和write从流中读取和向流中写入，使用冒号运算符将它们当作流对象的方法来调用

--I/O库提供3个预定义的C语言流的句柄
io.stdin 标准输入
io.stdout 标准输出
io.stderr 标准错误输出
io.stderr:write("this is error message")

--io.input和io.output允许混用完整和简单I/O模型，调用无参数的io.input可以获取当前输入流，可以设置它的参数来设置当前输入流，类似的调用同样适用于io.output
local temp = io.input() --保存当前输入流
io.input("newinput")    --打开新的当前输入流
io.input():close()	    --关闭当前流
io.input(temp)	        --恢复之前的当前输入流
```

`io.read()`实际上是`io.input():read()`的简写，即函数`read`是用在当前输入流上的，同样，`io.write()`是`io.ouput():write()`的简写

还可以用`io.lines()`从流中读取内容，它返回一个可以从流中不断读取内容的迭代器，可以接受和`io.read`一样的参数

## 其他文件操作

`io.tmpfile`用于创建一个临时文件，并返回一个操作临时文件的句柄，该句柄是以读写模式打开的，当程序运行结束后，该临时文件会被自动删除，它提供了一种方便的方式来在 Lua 中运行过程中创建和使用临时文件，无需担心文件的管理和清理问题

```lua
local tmpfile = io.tmpfile() --创建一个临时文件并获取其句柄
tmpfile:write("some data written to the temporary file")
tmpfile:close() --关闭临时文件句柄，临时文件一旦关闭就无法打开
--此时，临时文件已经被自动删除，它们通常用于存储临时数据

--立即将缓冲区中的数据写入文件
flush将所有缓冲数据写入文件，可以当作io.flush()使用以刷新当前输出流；也可以当作f:flush()使用以刷新流f
local file = io.open("example.txt", "w")  
if file then  
    file:write("Hello, World!")  
    file:flush()  --确保数据被立即写入文件  
    file:close()  
    end

--设置文件流的缓冲策略
file:setvbuf (mode [,size])
mode有三种：
"no"：无缓冲
"line"：输出一直被缓冲直到遇到换行符或从一些特定文件（例如终端设备）中读取到了数据
"full"：缓冲区满或者显式刷新文件时才写入
size：指定缓冲区大小，按字节单位

--获取和设置文件的当前位置，返回从文件开头计算起的文件的位置，按字节单位
file:seek([whence][,offset])
whence设置文件指针位置：
"set"：从文件头开始
"cur"：从当前位置开始[默认]
"end"：从文件尾开始
offset：文件指针要移动的字节数
local fileindex = ("/log","r+")
local current fileindex:seek() --保存当前位置
print(fileindex:seek("end"))   --文件末尾位置，即文件大小
fileindex:seek("set",current)  --恢复当前位置
seek函数受到换行符转换的影响，如果需要精确控制文件位置，最好以二进制模式"b"打开文件

--重命名或移动文件
os.rename(oldname, newname)
oldname：要重命名或移动的文件的当前名称或路径
newname：文件的新名称或路径，如果它与oldname不在同一个目录下，则文件会被移动

--删除文件
os.remove(要删除文件的路径)
这两个函数处理的是真实文件而非流，所以它们位于os库而非io库中
```

## 其他系统调用

```LUA
--终止当前程序运行
os.exit([status])
接受一个状态码，用于向操作系统指示程序的退出状态

--获取环境变量值
os.getenv("pathname")
local path = os.getenv("PATH")  
if path then  
    print("PATH:", path)  
else  
    print("环境变量PATH未设置")  
end
```

**运行系统命令**

```lua
os.execute等价于C语言中的函数system
参数为表示待执行命令的字符串，返回值为命令运行结束后的状态
function s(dirname)
    local status = os.execute("mkdir "..dirname)
    if status then
        print("目录创建成功")
        else
        print("目录创建失败")
        end
end

--执行外部命令并获取其输出
该函数运行一条系统命令，但该函数还可以重定向命令的输入/输出，从而使得程序可以向命令中写入或从命令的输出中读取
io.popen(prog [, mode])
prog：要执行的外部命令
mode：指定文件句柄的模式。默认值"r"读取，"w"写入
--使用当前目录中的所有内容构建了一个表
local f = io.popen("dir ","r") --根据系统不同，也可能是ls
local dir = {}
for entry in f:lines() do
    dir[#dir + 1] = entry
end
```

```lua
--小脚本
options = {
    "ipconfig", --显示网络配置信息
    "netstat",  --显示网络连接、路由表和网络接口信息
    "msconfig", --打开系统配置实用程序
    "regedit",  --打开注册表编辑器
    "taskmgr",  --打开任务管理器
    "calc",     --打开计算器
    "notepad",  --打开记事本
}
comment = {
    "显示网络配置信息",
    "显示网络连接、路由表和网络接口信息",
    "打开系统配置实用程序",
    "打开注册表编辑器",
    "打开任务管理器",
    "打开计算器",
    "打开记事本",
}
io.write("请输入数字选择要打开的程序：\n")
for k, v in pairs(comment) do
    io.write(k .. ". " .. v .. "\n")
end

key = io.read("n")
tab = string.format("%s", options[key])
if key == 1 or key == 2 then tab = "start cmd /k " .. tab end
io.popen(tab, "r")
```

# 基础语法

不能理解作者为什么这里才开始讲最基础的东西，非得先讲Lua的特性，但我承认Lua的特性确实可以

**局部变量和代码块**

Lua中的变量在默认情况下是全局变量，所有的局部变量在使用前必须声明

局部变量的生效范围仅限于声明它的代码块，一个代码块是一个控制结构的主体，或是一个函数的主体，或是一个代码段(即变量被声明时所在的文件或字符串)

```Lua
x = 10
local i = 1 --对于代码段是局部的

while i <= x do
    local x = i * 2 --对于循环体是局部的
    print(x)
    i = i + 1 
end

if i > 20 then
    local x --对于then是局部的
    x = 20
    print(x + 2) --22
else
    print(x) --使用全局变量x 10
end

print(x) --使用全局变量x 10
```

> <span style="color: orange;">尽可能地使用局部变量是一种良好的编程风格</span>

+ 局部变量可以避免由于不必要的命名而造成全局变量的混乱
+ 局部变量可以避免同一程序中不同代码部分中的命名冲突
+ 访问局部变量比访问全局变量更快
+ 局部变量会随着其作用域的结束而消失，从而使得垃圾收集器能够将其释放

局部变量的声明可以包含初始值，其赋值规则与常见的多重赋值相同：多余的值会丢弃，多余的变量赋值为`nil`，如果一个声明中没有赋初始值，则初始化为`nil`

```LUA
local a, b = 1, 10
if a < b then
    print(a) --1
    local a  --=nil是隐式的
    print(a) --nil
end
print(a, b)  --1 10
```

`do-end`一种代码块的构造方式，允许将多条语句组合成一个单一的语句块，当需要更好地控制某些局部变量的生效范围时非常有用

```lua
--创建独立代码块
do  
    -- 这个代码块独立于其他代码执行  
    local variable = "I am local to this block"  
    print(variable)  
end
下面的代码无法访问上面的variable，因为它只在do-end块中有效
```

可以使用`do-end`在函数或语句内创建局部作用域，对于组织代码和确保作用域的正确性非常有用，可以确保变量不会泄漏到外部作用域

lua中有一种常见的用法：

```lua
local niu = niu
声明一个局部变量niu，然后用全局变量niu对其赋初值
这个用法在需要提高全局变量的访问速度时很有用，当其他函数改变了全局变量的值，而代码段又需要保留原始值时这个用法也很有用
```

## 控制结构

```Lua
--根据条件是否满足执行相应的then部分或else部分，else部分是可选的
if a ~= 0 then a = 0 end
if a > b then return a; else return b; end

if opt == "+" then
    res = a + b
elseif opt == "-" then
    res = a - b
elseif opt == "*" then
	res = a * b
elseif opt == "/" then
    res = a / b
else
	io.stderr:write("invalid operation")
end
如果要编写嵌套的if，可以使用elseif，可以避免重复使用end
lua不支持switch语句，只能使用elseif来实现

--while条件为真重复执行循环体，为假结束循环
while condition do
    ...
end

--repeat-until语句会重复执行其循环体直到条件为真时结束，因为条件测试在循环体之后执行，所以循环体至少会执行一次
--输出第一个非空行
repeat
    line = io.read()
until line ~= ""    
print(line)
局部变量的作用域是从它们被声明的点开始，直到包含它们的代码块结束
```

for语句有数值型`for`和泛型`for`

```lua
--数值型for
for var = expr1,expr2,expr3 do
    ...
end
var的值从expr1到expr2之前的每次循环都会执行循环体，每次循环结束后将步长expr3增加到var上，expr3是可选的，默认步长值为1，可以设为负值向前移动来逆序循环
不想设置循环上限可以使用常量math.huge来表示正无穷大
不想设置循环下限可以使用常量-math.huge来表示负无穷大
在循环开始前，3个表达式都会运行1次
控制变量是被for语句自动声明的局部变量，其作用范围仅限于循环体内
local save = nil
for i = 1,10 do --i是局部变量
    io.write(i)
    save = i    --保存i的值
end

--泛型for
泛型for遍历迭代函数返回的所有值，它的功能非常强大，使用恰当的迭代器可以在保证代码可读性的情况下遍历几乎所有的数据结构
在后面介绍

break结束当前的循环，如果嵌套只能结束所在层级及以下循环
return返回函数的执行结果或简单地结束函数运行，所有函数最后都有隐式的return，因此无需在每一个没有返回值的函数最后添加return语句

::标签名::
使用goto跳转时，标签遵循常见的可见性规则，因此不能直接跳转到一个代码块中的标签，因为代码块内的标签对外不可见
不能跳转到函数外
不能跳转到局部变量的作用域
for i = 1,math.huge do
    if a then
        goto continue
    else
        goto redo
    end
    ::continue::
end
::redo::
```

# 闭包

Lua中，函数是严格遵循词法定界的第一类值

**第一类值**意味着Lua中的函数与其他常见类型的值(例如数值和字符串)具有同等权限：一个程序可以将某个函数保存到变量或表中，也可以将某个函数作为参数传递给其他函数，还可以将某个函数作为其他函数的返回值返回

**词法定界**意味着Lua中的函数可以访问包含其自身的外部函数中的变量

**函数是第一类值**

```Lua
A = { p = print }     --A.p指向print函数
A.p("Hello, World")   --Hello world
print = math.sin      --print现在指向sin函数
A.p(print(1))         --0.8414709848079
math.sin = A.p        --sin现在指向print函数
math.sin(10,20)       --10 20

--函数也是值，当然有创建函数的表达式
function sugar(x) return x * 2 end
它是下面这种写法的一种美化格式
sugar = function(x) return x * 2 end
赋值语句的右边表达式就是函数构造器，因此函数定义实际上就是创建类型为function的值并把它赋值给一个变量
```

**Lua中的所有函数都是匿名的**，函数并没有名字。当讨论函数名时，实际上是指保存该函数的变量

`table.sort`以一个表为参数对其中的元素排序，`table.sort`并没有试图穷尽所有的排序方式，而是提供了一个可选参数(排序函数)，排序函数接受两个参数并根据第一个元素是否应排在第二个元素之前返回不同的值

```Lua
network = {
    { name = "grauna",  IP = "210.26.30.34"},
    { name = "arraial", IP = "210.26.30.23"},
    { name = "lua",     IP = "210.26.23.12"},
    { name = "derain",  IP = "210.26.23.20"},
}
--如果想针对name字段，按字母顺序逆序对该表排序：
table.sort(network,function(a,b) return (a.name > b.name) end)
如果返回true，那么a排在b之前，false则反之
```

像`table.sort`这样以另一个函数为参数的函数称之为高阶函数，高阶函数是一种强大的编程机制， 利用匿名函数作为参数是其灵活性的主要来源

## 非全局函数

因为函数是一种第一类值，因此函数不仅可以被存储在全局变量中，还可以被存储在表字段和局部变量中

```lua
lib = {} --创建空表来用作库
lib.add = function(a, b) return a + b end
lib.mult = function(a, b) return a * b end
print(lib.add(1, 2),lib.mult(1, 2)) -- 3  2 调用表中的函数

--也可以使用表构造器
lib = {
    add = function(a, b) return a + b end,
    mult = function(a, b) return a * b end
}

--还有另一种特殊的语法来定义
lib = {}
function lib.add(a, b) return a + b end
function lib.mult(a, b) return a * b end
```

**在表字段中存储函数是Lua中实现面向对象编程的关键要素**

把一个函数存储到局部变量内就得到了一个局部函数，即一个被限定在指定作用域中使用的函数

局部函数对于包非常有用，因为Lua将每个程序段作为一个函数处理，所以在程序段中声明的函数就是局部函数，只在该程序段中可见，词法定界保证程序段中的其他函数可以使用这些局部函数

```lua
local fact = function(n)
if n == 0 then return 1
    else return n*fact(n-1)
    end
end
错误代码，当Lua编译函数体中的fact(n-1)调用时，局部的fact尚未定义，因此该表达式会尝试调用全局fact而非局部fact
--可以通过先定义局部变量再定义函数的方式解决
local fact
fact = function(n)
if n == 0 then return 1
    else return n*fact(n-1) 
    end
end

local function sugar(params) dody end
--Lua展开局部函数语法糖
local sugar; sugar = function(params) dody end

--这个技巧对于间接递归函数是无效的，在间接递归情况下，必须使用与明确的前向声明等价的形式
local f --前向声明
local function g()
	some code f() some code
end
function f()
	some code g() some code
end
不能在最后一个函数定义前加上local，否则会创建一个全新的局部变量f，从而使先前声明的f变为未定义状态
```

## 词法定界

编写一个被函数B包含的函数A时，被包含的函数A可以访问包含它的函数B的所有局部变量，这种特性就是词法定界

```lua
--假设有一个表，其中包含了学生的姓名和对应的成绩，基于分数对学生姓名排序，分数高者在前
Names = { "xx", "ss", "aa" }
Grades = { aa = 30, xx = 10, ss = 20 }
table.sort(Names, function(n1, n2)
    return Grades[n1] > Grades[n2] 
end)
print(table.concat(Names, ",")) 

--创建一个函数来完成
function SortByGrade(n1, n2)
    table.sort(Names, function(n1, n2)
        return Grades[n1] > Grades[n2]
    end)
end
传给table.sort的匿名函数可以访问Grades，Grades是包含匿名函数的外层函数SortByGrade的形参，在该匿名函数中，Grades既不是全局变量也不是局部变量，而是非局部变量
```

函数作为第一类值，能够跳出它们变量的原始定界范围

```lua
function Counter()
    local count = 0
    return function()
        count = count + 1
        return count 
    end
end
C = Counter()
print(C()) --1
print(C()) --2
```

匿名函数访问一个非局部变量`count`，创建变量的函数`Counter`已经返回，因此当调用匿名函数时，`count`似乎已经超出了作用范围，但其实没有，因为闭包概念的存在，Lua能正确地应对这种情况

**一个闭包就是一个函数外加能够使该函数正确访问非局部变量所需的其他机制**

```lua
再次调用Counter，会创建一个新的局部变量count和一个新的针对新变量的闭包
D = Counter()
print(D()) --1
print(C()) --3
print(D()) --1
C和D是不同的闭包，它们建立在相同的函数之上，但是各自拥有局部变量
```

开发一个用来表示几何区域的系统，其中区域即为点的集合，能够利用该系统表示各种各样的图形，同时可以通过多种方式（旋转、变换、并集等）组合和修改这些图形

```lua
--[[南半球所能看到的峨眉月
c1 = disk(0, 0, 1)
plot(difference(c1,translate(c1,0.3,0)),500,500)
--]]
---@diagnostic disable: lowercase-global
--利用高阶函数和词法定界
--可以很容易地定义一个根据指定的圆心和半径创建圆盘的函数
function disk(cx, cy, r)
    return function(x, y)
        return (x - cx) ^ 2 + (y - cy) ^ 2 <= r ^ 2
    end
end

--表示一个以点（1.0,3.0）为圆心、半径4.5的圆盘（一个圆形区域）


--创建指定边界的轴对称矩形的函数
function rect(left, right, bottom, up)
    return function(x, y)
        return left <= x and x <= right and
            bottom <= y and y <= up
    end
end

--创建任何区域的补集
function complement(r)
    return function(x, y)
        return not r(x, y)
    end
end

--并集
function union(r1, r2)
    return function(x, y) return r1(x, y) or r2(x, y) end
end
--交集
function intersection(r1, r2)
    return function(x, y) return r1(x, y) and r2(x, y) end
end
--差集
function difference(r1, r2)
    return function(x, y) return r1(x, y) and r2(x, y) end
end

--按照指定的增量平移指定的区域
function translate(r, dx, dy)
    return function(x, y) return r(x - dx, y - dy) end
end

function plot(r, m, n)
    io.write("P1\n", m, " ", n, "\n") --文件头
    for i = 1, n do                   --对于每一行
        local y = (n - i * 2) / n
        for j = 1, m do               --对于每一列
            local x = (j * 2 - m) / n
            io.write(r(x, y) and "1" or "0")
        end
        io.write("\n")
    end
end
```

# 模式匹配

`string.find`用于在指定的目标字符串中搜索指定的模式，成功返回两个值：匹配到模式开始位置和结束位置的索引。没有找到任何匹配则返回`nil`

```Lua
s = "hello world"
i,j = string.find(s,"hello")
print(i,j) --1  5
--使用sub函数获取目标字符串中匹配相应模式的子串
print(string.sub(s,i,j))

--string.find具有两个可选参数
第3个参数是一个索引，用于说明从目标字符串的哪个位置开始搜索
第4个参数是一个布尔值，用于说明是否进行简单搜索
简单搜索就是忽略模式而在目标字符串中进行单纯的“查找子字符串”的
动作
string.find("[word]","[")
--[在模式中有特殊意义，会报错
string.find("[word]","[",1,true)
```

`string.match`用于在字符串中查找与指定模式匹配的第一个子串。成功返回匹配的子串，没有找到匹配项返回`nil`

```lua
--当模式是变量时，这个函数也可以使用
date = "Today is 17/7/2020"
d = string.match(date,"%d+/%d+/%d+")
date = "Today is 17/7/2023"
print(d) --17/7/2023
```

`string.gsub`有3个必选参数：目标字符串、模式、替换字符串，基本用法是将目标字符串中所有出现模式的地方换成替换字符串

```lua
s = string.gsub("Lua is cute", "cute", "great")
print(s) -- Lua is great
--还有一个可选的第4个参数，用于限制替换次数
s = string.gsub("xxxxxx", "x", "n", 2)
print(s) --nnxxx
```

`string.gmatch`通过返回的函数可以遍历一个字符串中所有出现的指定模式

```lua
for iter in string.gmatch(string, pattern) do 
    ...
end
string要进行模式匹配的源字符串
pattern是返回一个函数，是用于匹配的模式字符串

local text = "he price is 100 dollars and 25 cents"  
local pattern = "%d+"  
-- 使用gmatch获取所有匹配的单词  
for word in text.gmatch(text,pattern) do
    io.write(word)
    io.write(" ")
end
```

## 模式

`string.find`、`string.match`、`string.gsub`和`string.gmatch`都使用相同的模式语法

```css
%a：字母
%d：数字
%w：字母或数字
%l：小写字母
%u：大写字母
%c：控制字符
%p：标点符号
%s：空白字符（包括空格、制表符、换行符等）
%x：十六进制数字
%z：任何代表零的字符
它们的大写形式是它们的补集，比如%A代表任意非字母的字符
.：除换行符以外的任何字符
%：用于转义特殊字符，使其表示普通字符
*：表示前面的模式元素出现零次或多次
+：表示前面的模式元素出现一次或多次
-：非贪婪匹配，尽可能少地匹配字符
^：表示模式的开始
$：表示模式的结束
^和$字符只有位于模式的开头和结尾时才具有特殊含义；否则它们仅是与其自身相匹配的普通字符
()：用于捕获匹配的子串，以便后续使用
%f[...]：匹配任何不在方括号中的字符
%i[...]：匹配任何在方括号中的字符（不区分大小写）
```

**捕获**

捕获机制允许根据一个模式从目标字符串中抽出与该模式匹配的内容，可以通过把模式中需要捕获的部分放到一对圆括号内来指定捕获

```lua
--函数string.match会将所有捕获到的值作为单独的结果返回
pair = "name = Anna"
key,value = string.match(pair,"(%a+)%s*=%s*(%a+)")
print(key,value) --name Anna

date = "Today is 17/7/2023"
d,m,y = string.match(date,"(%d+)/(%d+)/(%d+)")
print(d,m,y) --17 7 2023

s = [[then he said:"it's all right"!]]
q,qa = string.match(s,"([\"'])(.-)%1")
--([\"']):匹配一个单引号或一个双引号
--(.-):匹配任意字符（除换行符）零次或多次
--%1:引用第一个捕获组的内容，即之前匹配的引号
print(qa) --it's all right
print(q)  --"

--匹配长字符串模式
p = "(%a).(=).%[==%[(%a*%d*%a*%d*%a*)"
s = "a = [==[somet123hig]==]"
io.write(string.match(s, p)) --a=somet123hig
对于字符串内有数字的长字符串，我直接在里面加%d，超过就再加！
```

函数`string.gsub`也可以使用被捕获对象，替代字符串可以包括像`%n`一样的字符分类，当发生替换时会被替换为相应的捕获。`%0`意味着整个匹配，并且替换字符串中的百分号必须被转义为`%%`

```lua
--重复字符串中的每个字母，并且在每个被重复的字母之间插入1个减号
print((string.gsub("hello lua!","%a","%0-%0")))
--h-he-el-ll-lo-o l-lu-ua-a!

--交换相邻字符串
print((string.gsub("hello lua","(%a+).(%a+)", "%2 %1")))

--剔除字符串两端空格
function trim(s)
    s = string.gsub(s, "^%s*(.-)%s*$", "%1")
    return s
end
print(trim("  text  "))
```

**替换**

`string.gsub`的第3个参数不仅可以是字符串，还可以是一个函数或表

+ 当第3个参数是一个函数时，`string.gsub`会在每次找到匹配时调用该函数，参数是捕获到的内容，而返回值则被作为替换字符串
+ 当第3个参数是一个表时，`string.gsub`会把第一个捕获到的内容作为键，然后将表中对应该键的值作为替换字符串

如果函数的返回值为`nil`或表中不包含这个键或表中键的对应值为`nil`，那么`gsub`不改变这个匹配

```lua
--把字符串中所有出现的$varname替换为全局变量varname的值
function expand(s)
    return (string.gsub(s, "$(%w+)", _G))
    --_G是Lua中预先定义的包括所有全局变量
end
name = "Lua"; status = "great"
print(expand("$name is $status,isn't it?"))
--Lua is great,isn't it?

--如果不确定是否指定变量具有字符串值，那么可以对它们的值调用函数tostring
function expand(s)
    return (string.gsub(s,"$(%w+)",function(n)
    return tostring(_G[n])
    end))
end
print(expand("print = $print;a=$a"))
--print = function: 00007FFC94B871D0;a=nil
对于所有匹配的地方，都会调用给定的函数，传入捕获到的名字作为参数，并使用返回字符串替换匹配到的内容

--将LATEX风格的命令"\command{text}"转换成XML风格的"<comma>text</command>"
--允许嵌套命令
function toxml(s)
    s = string.gsub(s, "\\(%a+)(%b{})", function(tag, body)
        body = string.sub(body, 2, -2)
        body = toxml(body)
        return string.format("<%s>%s</%s>", tag, body, tag)
    end)
    return s
end
```



# 日期和时间

```lua
print(os.time())
--可以使用一些基本的数学运算分离这个数值
local date = os.time()
local dayyear=365
local sechour=60 * 60
local secday=sechour * 24
local secyear = secday * dayyear
--years
print(date // secyear + 1970)
--hours(UTC)UTC时间与中国时间是不同的，存在8个小时的时差
print(date % secday // sechour)
--minutes
print(date % sechour // 60)
--seconds
print(date % 60)

--os.date可以将一个表示日期和时间的数字转换为某些高级的表示形式，要么是日期表要么是字符串
os.date([format, [time]])
format：可选的字符串，指定了日期和时间的输出格式，默认%c
time：可选参数，表示要格式化的时间戳，默认使用当前时间
所有的表现形式取决于当前的区域设置
%a：星期简写
%A：星期全名
%b：月份简写
%B：月份全名
%y：后两位数年份
%Y：完整的年份
%c：日期和时间
%x：日期
%X：时间
%m：月份
%M：分钟
%S：秒数
%w：星期（0=星期日，1=星期一，...，6=星期六）
%p："am"或"pm"
%d：一个月中的第几天
%j：一年中的第几天
%W：一年中的第几周
%H：24小时制小时数
%I：12小时制小时数
%z：时区
%%：百分号
如果格式化字符串以!开头，会以UTC格式对其进行解析
```

`os.date`会生成日期表，有以下字段：

```css
year
month 1-12
day   1-31
hour  0-23
min   0-59
sec   0-60
wday 星期天数1-7
yday 年份天数1-366
isdst 布尔值，表示是否使用夏令时DST
```

```Lua
t = os.date("*t")  --获取当前时间
print(os.date("%Y/%m/%d",os.time(t)))
local n =io.read("n")
t.day=t.day+n
print(os.date("%Y/%m/%d",os.time(t)))
io.write("hours:",t.hour," minutes:",t.min," seconds:",t.sec)
```

# 数据结构

表并不是一种数据结构，它是其他数据结构的基础。可以用表来实现其他语言提供的数据结构，用Lua中的表实现这些数据结构还很高效

**数组**

使用整数来索引表即可实现数组

```lua
--任何访问范围1～1000之外的元素都会返回nil而不是0
local a = {}
for i = 1, 1000 do
    a[i] = 0
end

--使用表构造器在一条表达式中同时创建和初始化数组
squares = {1, 4, 6, 9, 12, 16, 24}
这种表构造器根据需求要多大就能多大
```

Lua中一般以1作为数组的起始索引，Lua的标准库和长度运算符都遵循这个惯例，如果数组的索引不从1开始，那就不能使用这些机制

**矩阵及多维数组**

Lua中有两种方式来表示矩阵，第一种方式是使用一个不规则数组，即数组的数组

```lua
local mt = {}
for i = 1, n do  --二维表，需要两个表
    local row = {}
    mt[i] = row
--Lua中的表是引用类型，操作的不是表本身，而是指向表数据的引用，这里不是复制row的内容到mt，而是让mt指向row所指的表，和C++的引用差不多
    for j = 1, m do
        row[j] = i
    end
end
for i = 1, n do  -- 外层循环遍历行  
    for j = 1, m do  -- 内层循环遍历列  
        print("Row:", i, "Column:", j, "Value:", mt[i][j])  
    end  
end
```

表在Lua中是一种对象，因此在创建矩阵时必须显式地创建每一行，比在C语言中直接声明一个多维数组更加具体，也提供了更多的灵活性

将前例中的内层循环改为`for j=1,i do...end`就可以创建一个三角形矩阵。使用这套代码，三角形矩阵较原来的矩阵可以节约一半的内存

第二种方式是将两个索引合并为一个，通过将第一个索引乘以一个合适的常量再加上第二个索引来实现这种效果

```lua
local mt = {}  --只需要一个表，铠甲合体！
for i = 1, n do
    local aux = (i - 1) * m
    for j = 1, m do
        mt[aux + j] = i
    end
end
for index, value in ipairs(mt) do
    print("Index:", index, "Value:", value)
end
```



**链表**

每个节点用一个表表示，也只能使用表表示，链接则为一个包含指向其他表的引用的简单表字段

```lua
Node = {}  
Node.__index = Node  
  
function Node.new(value, nextNode)  
    local self = {}  
    setmetatable(self, Node)  
    self.value = value  
    self.next = nextNode  
    return self  
end  
  
-- 定义链表类  
LinkedList = {}  
LinkedList.__index = LinkedList  
  
function LinkedList.new()  
    local self = {}  
    setmetatable(self, LinkedList)  
    self.head = nil  
    return self  
end  
  
-- 在链表末尾添加节点  
function LinkedList:append(value)  
    local newNode = Node.new(value, nil)  
    if self.head == nil then  
        self.head = newNode  
    else  
        local current = self.head  
        while current.next ~= nil do  
            current = current.next  
        end  
        current.next = newNode  
    end  
end  
  
-- 遍历链表并打印节点值  
function LinkedList:printList()  
    local current = self.head  
    while current ~= nil do  
        print(current.value)  
        current = current.next  
    end  
end  
  
-- 测试代码  
local list = LinkedList.new()  
list:append(1)  
list:append(2)  
list:append(3)  
list:printList() 
```

**队列及双端队列**

在Lua中实现队列的一种简单方法是使用`table`标准库中的函数`insert`和`remove`，另一种更高效的实现是使用两个索引，一个指向第一个元素，另一个指向最后一个元素的双端队列

```lua
-- 定义双端队列类  
Deque = {}  
Deque.__index = Deque  
  
-- 构造函数，初始化双端队列  
function Deque.new()  
    local self = {}  
    setmetatable(self, Deque)  
    self.items = {} -- 使用表来存储队列中的元素  
    return self  
end  
  
-- 在双端队列的前端插入元素  
function Deque:pushFront(value)  
    table.insert(self.items, 1, value) -- 在表的第一个位置插入元素  
end  
  
-- 在双端队列的后端插入元素  
function Deque:pushBack(value)  
    table.insert(self.items, value) -- 在表的末尾插入元素  
end  
  
-- 从双端队列的前端移除并返回元素  
function Deque:popFront()  
    if #self.items == 0 then  
        return nil -- 如果队列为空，则返回nil  
    end  
    return table.remove(self.items, 1) -- 移除并返回表的第一个元素  
end  
  
-- 从双端队列的后端移除并返回元素  
function Deque:popBack()  
    if #self.items == 0 then  
        return nil -- 如果队列为空，则返回nil  
    end  
    return table.remove(self.items) -- 移除并返回表的最后一个元素  
end  
  
-- 获取双端队列前端的元素，但不移除  
function Deque:front()  
    if #self.items == 0 then  
        return nil -- 如果队列为空，则返回nil  
    end  
    return self.items[1] -- 返回表的第一个元素  
end  
  
-- 获取双端队列后端的元素，但不移除  
function Deque:back()  
    if #self.items == 0 then  
        return nil -- 如果队列为空，则返回nil  
    end  
    return self.items[#self.items] -- 返回表的最后一个元素  
end  
  
-- 检查双端队列是否为空  
function Deque:isEmpty()  
    return #self.items == 0  
end  
  
-- 打印双端队列中的所有元素  
function Deque:printAll()  
    for i, v in ipairs(self.items) do  
        print(v)  
    end  
end  
  
-- 测试代码  
local deque = Deque.new()  
  
-- 在双端队列的前后端分别插入元素  
deque:pushFront("Front1")  
deque:pushBack("Back1")  
deque:pushFront("Front2")  
deque:pushBack("Back2")  
  
-- 打印双端队列中的所有元素  
print("Initial deque:")  
deque:printAll()  
  
-- 从双端队列的前后端分别移除元素  
print("After popping front and back:")  
print("Popped front:", deque:popFront())  
print("Popped back:", deque:popBack())  
deque:printAll()  
  
-- 获取双端队列的前后端元素  
print("Front element:", deque:front())  
print("Back element:", deque:back())  
  
-- 检查双端队列是否为空  
print("Is deque empty?", deque:isEmpty())
```

**索引表**

```Lua
days = { "Sunday", "Monday", "Tuesday", "Wednesday",
    "Thursday", "Friday", "Saturday" }
--手动输入[[revdays = {
    ["Sunday"] = 1,
    ["Monday"] = 2,
    ["Tuesday"] = 3,
    ["Wednesday"] = 4,
    ["Thursday"] = 5,
    ["Friday"] = 6,
    ["Saturday"] = 7
}
--]]
revdays = {}
for k, v in pairs(days) do --键是数字，值是字符串
    revdays[v] = k
end
for i, j in pairs(revdays) do
    print(revdays[i], days[j])
end
```

**字符串缓冲区**

```lua
--逐行地读取一个文件
local buff = ""
for line in io.lines() do
    buff = buff .. line .. "\n"
end
```

虽然这段Lua代码看似能够正常工作，但实际上在处理大文件时却可能导致巨大的性能开销。在笔者的新机器上用这段代码读取一个4.5MB大小的文件需要超过30秒的时间

可以把一个表当作字符串缓冲区

```Lua
local t = {}
for line in io.lines() do
    t[#t + 1] = line .. " "
end
local s = table.concat(t," ")
print(s)

--table.concat是一个内置函数，用于拼接表中的所有元素成一个字符串。它接受一个表作为第一个参数，接受一个分隔符字符串和一个起始索引作为可选参数
```

# 数据文件和序列化

**数据文件**

在写入数据时将数据文件写成Lua代码就可以在读取数据非常容易

如果处理的是出于自身需求而创建的数据文件，那么就可以将Lua的构造器用于格式定义

```LUA
Donald E. Knuth, Literate Programming, CSLI,1992
Jon Bentley, Nore Programming Pearls, Addison-Wesley,1990
--数据文件就可以改为：
Entry{"Donald E. Knuth",
"Literate Programming"
"CSLI"
19923}
Entry{"Jon Bentley",
"Nore Programming Pearls",
"Addison-Wesley",
19903}
```

`Entry{code}`和`Entry({code})`相同的，后者以表作为唯一的参数来调用函数`Entry`

当需要读取该文件时，只需要定义一个合法的`Entry`， 然后运行这个程序即可

```lua
--计算某个数据文件中数据条目的个数
local count = 0
function Entry() count = count + 1 end
dofile("文件路径")
--打开该名字的文件，并执行文件中的Lua代码块
--不带参数调用，dofile执行标准输入的内容.返回该代码块的所有返回值
print("number of entries:"..count)

--获取某个数据文件中所有作者的姓名并打印
local authors = {} --保存作者姓名的集合
function Entry(b) authors[b[1]] = true end
--在authors表中以作者姓名b[1]为键设置一个值，确保每个作者姓名在表中只出现一次
dofile("文件路径")
for name in pairs(authors) do print(name) end
```

上述的代码段中使用了事件驱动的方式：函数`Entry`作为一个回调函数会在函数`dofile`处理数据文件中的每个条目时被调用

当文件的大小并不是太大时，可以使用键值对的表示方法：这种格式是所谓的自描述数据格式，其中数据的每个字段都具有一个对应其含义的简略描述

当需要修改时，自描述数据也易于手工编辑；此外，自描述数据还允许在不改变数据文件的情况下对基本数据格式进行细微的修改。例如， 当我们想要增加一个新字段时，只需对读取数据文件的程序稍加修改，使其在新字段不存在时使用默认值

```Lua
--数据文件格式
Entry {
    author = "Donald E. Knuth",
    title = "Literate Programming",
    publisher = "CSLI",
    year = 1992,
}
Entry {
    author = "Jon Bentley",
    title = "More Programming Pearls",
    year = 1990,
    publisher = "Addison-Wesley",
}
--使用键值对格式时，获取作者姓名的程序将变为
local authors = {}
function Entry(b) authors[b.author] = true end
dofile("文件路径")
for name in pairs(authors) do print(name) end
```

此时，字段的次序就无关紧要了。即使有些记录没有作者字段， 也只需要修改`Entry`函数

```Lua
function Entry(b)
    authors[b.author or "unknown"] = true
    --没有作者输出unknown
    end
```

**序列化**

序列化是将数据结构或对象状态转换为可以存储或传输的格式的过程

将某些数据序列化/串行化就是将数据转换为字节流或字符流，以便将其存储到文件中或者通过网络传输；也可以将序列化后的数据表示为Lua代码，当这些代码运行时，被序列化的数据就可以在读取程序中得到重建

```lua
local fmt={integer="%d",float="%a"}
--用十进制格式保存浮点数可能损失精度，可以利用十六进制格式来避免这个问题，%a以十六进制输出，可以保留被读取浮点型数的原始精度
function serialize(o)
    if type(o) =="number" then
       return string.format(fmt[math.type(o)],o)
--math.type函数用于确定数字的类型，返回integer或float，这个信息随后被用来从fmt表中选择正确的格式化字符串
--return才能使用返回值来赋值        
    end
end
print(serialize(1.134))
local r= serialize(1.134)
print(tonumber(r,16)) --16进制转10进制
```

对于字符串类型的值，最简单的序列化方式形如：

```lua
if type(o)=="string" then
    io.write("'",o,"'")
end
```

不过如果字符串包含特殊字符，比如引号或换行符，那么结果就会错误，修改引号的方式是不安全的，可以使用函数`string.format`的`%q`选项，该选项用于将参数转换为可安全嵌入到Lua代码中的字符串形式。具体来说，它会为字符串添加双引号，并转义字符串中的任何特殊字符

```Lua
a='a"problem"\\string'
print(string.format("%q",a))
```

通过使用这个特性，函数`serialize`将变为

```lua
function serialize(o)
    if type(o) == "number" then
        return string.format(fmt[math.type(o)], o)
    elseif type(o) == "string" then
        return string.format("%q", o)
    else io.stderr:write("error:")
    end
end
local r1, r2 = serialize(), serialize("ww'ww")
print(r1, r2)
```

`%q`还可以用于 `nil`和`Boolean`类型

另一种保存字符串的方式是使用主要用于长字符串的`[=[...]=]`。，不过这种方式主要是为不用改变字符串常量的手写代码提供的

Lua语言总是会忽略长字符串开头的换行符，要解决这个问题可以通过一种简单方式，即总是在字符串开头多增加一个换行符（这个换行符会被忽略）

```lua
--将输入的字符串格式化为一个被等号边框包围的形式，边框的长度取决于s中最长等号序列的长度。如果没有等号序列，则边框长度将为0
function quote(s)
    --存储字符串中最长等号序列的长度
    local n = -1
    --循环遍历字符串中的等号序列
    for w in string.gmatch(s, "]=*") do
        --减1是去掉]，max接受两个参数，并返回这两个数中的较大值
        n = math.max(n, #w - 1)
    end
    --最长等号序列长度加1
    local eq = string.rep("=", n)
    --rep用于重复一个字符串指定的次数。它接受两个参数：要重复的字符串和重复的次数
    return string.format("[%s[%s]%s]", eq, s, eq)
end
a = "no equals here"
b="[1]=wew"
c = "]=========]hello["
print(quote(a))
print(quote(b))
print(quote(c))
```

**保存不带循环的表**

```Lua
--不使用循环序列化表
function serialize(o)
    local t = type(o)
    if t == "number" or t == "string" or t == "nil"
        or t == "boolean" then
        io.write (string.format("%q", o))
    elseif t == "table" then --Lua中，表是唯一的复合数据类型
        io.write("{\n")
        --输出左大括号和一个换行符，表示表的开始
        for k, v in pairs(o) do --遍历表中所有键值对
            io.write(" ", k, " = ")
            --调用 `serialize` 函数来序列化值
            serialize(v)
            io.write("\n")
        end
        io.write("}\n")
    else
        error("cannot serialize a" .. type(o))
    end
end
local myTable = {  
    name = "Alice",  
    age = 30,  
    isStudent = false,  
    subjects = { "Math", "English", "Science" }  
}  
serialize(myTable)
```

只要表结构是一棵树（即没有共享的子表和环），那么该函数甚至能处理嵌套的表，即表中还有其他的表，在输出中以缩进形式输出嵌套表看上去会更具美感

上例中的函数假设了表中的所有键都是合法的标识符，如果一个表的键是数字或者不是合法的Lua标识符，那么就会有问题

**保存带有循环的表**

由于表构造器不能创建带循环的或共享子表的表，所以如果要处理表示通用拓扑结构（例如带循环或共享子表）的表，需要引入名称来表示循环

下面的函数把值外加其名称一起作为参数。另外还必须使用一个额外的表来存储已保存表的名称，以便在发现循环时对其进行复用。这个额外的表使用此前已被保存的表作为键，以表的名称作为值

```lua
function basicSerialize(o)
    --假设o是数字或字符串
    return string.format("%q", o)
end

function save(name, value, saved)
    --变量名，要保存的值，一个表，用于跟踪已经保存过的表
    saved = saved or {}
    io.write(name, " = ")
    if type(value) == "number" or type(value) == "string" then --检查值类型来决定调用函数序列化还是递归处理
        io.write(basicSerialize(value), "\n")
    elseif type(value) == "table" then
        if saved[value] then --如果是表递归处理
--如果该表已经被保存过，即saved[value]存在，则直接输出它的名称
            io.write(saved[value], "\n")
        else
            saved[value] = name
            io.write("{}\n")
--否则将该表的名称保存到saved表中，并输出一个空表，然后遍历表的每一个键值对，并递归地调用save函数来保存它们            
            for k, v in pairs(value) do
                k = basicSerialize(k)
                local fname = string.format("%s[%s]", name, k)
                save(fname, v, saved)
            end
        end
    else
        error("cannot save a" .. type(value))
    end
end
a = { x = 1, y = 2, { 3, 4, 5 } }
a[2] = a
a.z = a[1]
save("a", a)
```

假设要序列化的表只使用字符串或数值作为键。`basicSerialize`用于对这些基本类型进行序列化并返回序列化后的结果，另一个函数`save`则完成具体的工作，其参数`saved`就是之前所说的用于存储已保存表的表

也可以在保存一个值时不指定全局名称而是通过一段代码来创建一个局部值并将其返回，也可以在可能的时候使用列表的语法等

# 编译、执行和错误

虽然Lua是解释型语言，但Lua总是在运行前预编译源码作为中间代码

解释型语言的区分不在于源码是否被编译，而在于是否有能力且轻易地执行动态生成的代码

**编译**

函数`dofile`是运行Lua代码段的主要方式之一，实际上它是一个辅助函数，函数`loadfile`才完成了真正的核心工作

函数`loadfile`也是从文件中加载Lua代码段，但它不会运行代码，只是编译代码，然后将编译后的代码段作为一个函数返回，与`dofile`不同，`loadfile`只返回错误码而不抛出异常

可以认为`dofile`就是：

```lua
function dofile(filename)
    local f = assert(loadfile(filename))
    return f()
end
```

如果`loadfile`执行失败，那么函数`assert`会引发一个错误

对于简单的需求而言，由于`dofile`在一次调用中就做完了所有工作，所以该函数非常易用。不过`loadfile`更灵活，在发生错误的情况中，`loadfile`会返回`nil`及错误信息，以允许我们按自定义的方式来处理错误

此外，如果需要多次运行同一个文件，那么只需调用一次`loadfile`函数后再多次调用它的返回结果即可。由于只编译一次文件，因此这种方式的开销要比多次调用函数`dofile`小得多

函数`load`与函数`loadfile`类似，不同之处在于该函数从一个字符串或函数中读取代码段，而不是从文件中读取

```lua
i = 0
f = load("i = i + 1")--这行代码执行后，变量f就变成执行i=i+1的函数
f(); print(i)
f(); print(i)
```

尽管函数`load`的功能很强大，但还是应该谨慎地使用。相对于其他可选的函数而言，该函数的开销较大并且可能会引起诡异的问题。 请先确定当下已经找不到更简单的解决方式后再使用该函数

```lua
--两行代码是等价的，但第2行代码会与其外层的函数一起被编译，所以其执行速度要快得多
f = load("i = i + 1")
f = function() i = i + 1 end
```

> ###### `load`函数总是在全局环境中编译代码段，因此一个相同的全局或局部变量，总是使用全局变量

函数`load`最典型的用法是执行外部代码，即那些来自程序本身之外的代码段或动态生成的代码

如果想运行用户定义的函数，由用户输入函数的代码后调用函数`load`对其求值。请注意，函数`load`期望的输入是一段程序，也就是一系列的语句。如果需要对表达式求值，那么可以在表达式前添加`return` ，这样才能构成一条返回指定表达式值的语句：

```lua
print "enter your expression:"
local line = io.read()
local func = assert(load("return " .. line))
print("the value of your expression is " .. func())
```

+ `load`所返回的函数就是一个普通函数，因此可以反复对其进行调用

+ 代码段中可以声明局部变量

Lua中函数定义是在定义时而不是编译时发生的一种赋值操作

```lua
--compile文件
compile = compile(x) --和function定义是一样的
    print(x)
end

--另一文件
f = loadfile("compile.lua")
print(compile) --nil
f()            --运行代码，无输出
compile("ok")  --ok
```

**预编译代码**

Lua会在运行源代码之前先对其预编译，Lua也允许我们以预编译的形式分发代码

```shell
$luac -o 预编译文件 生成的预编译文件
```

Lua解析器会像执行普通Lua代码一样执行预编译文件，完成与原 来代码完全一致的动作，**几乎Lua语言中所有能够使用源码的地方都可以使用预编译代码**

**错误**

由于Lua语言是一种经常被嵌入在应用程序中的扩展语言，所以当错误发生时并不能简单地崩溃或退出

可以显式地通过调用函数`error`并传入一个错误信息作为参数来引发一个错误，通常这个函数就是在代码中提示出错的合理方式

```lua
```



<span style="color: orange;"></span>

<span style="color: orange;"></span>
