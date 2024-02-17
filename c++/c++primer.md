# 还没学的

144页显式转换(这个危险)
189页使用引用参数返回额外信息的程序

# Cpp基础

```C++
#include <iostream>
using namespace std;

int main(void) {
    int i = 7;
    std::cout << "i=" << i << std::endl;
    return 0;
}
```
`iostream`库包含两个基础类型：`istream`和`ostream`，用来分别表示输入流和输出流
一个流就是一个字符序列，随着时间的推移，字符是顺序生成或消耗的

`cin`为标准输入(standard input)
`cout`为标准输出(standard output)
`cerr`输出警告和错误信息(标准错误standard error)
`clog`输出程序运行时的一般性信息

输出运算符`<<`左侧是一个`ostream`对象，右侧是要打印的值
输入运算符`>>`左侧是一个`istream`对象，存入右侧运算对象

`endl`被称为操作符(manipulator)的特殊值
效果是结束当前行，并将与设备关联的缓冲区(buffer)中的内容输出，也就是刷新缓冲区

`std::`指出名字`cout`和`endl`是定义在`std`的命名空间(namespace)中的
命名空间可以帮助避免不经意的名字定义冲突，以及使用库中相同名字导致的冲突
标准库定义的所有名字都在命名空间`std`中

但通过命名空间使用标准库有一个副作用:
当使用标准库中的一个名字时，必须显式说明想使用的来自命名空间std中的名字
也就是需要通过作用域运算符`::`来指出想使用定义在命名空间std中的名字

作用域运算符`::`指编译器应从操作符左侧名字所示的作用域中寻找右侧名字
std:：cin的意思就是要使用命名空间std中的名字cin。

**防卫式声明**

```C
#ifndef//判断是否定义
#define
......
#endif//结束一个#ifnef或#ifndef区域
如果未定义一个文件就定义出来
这样可以防止相同宏被重复定义和多次包含相同头文件不会重复包含
```

## 类简介

在C++中，通过定义类(class)来定义data structure
一个类定义一个类型和与其关联的一组操作
每个类实际上都定义了一个新的类型，类型名就是类名
```C++
#include <iostream>
#include "Sales_item.hpp"

int main() {
    Sales_item book1,book2;

    std::cout << "国际标准图书编号(International Standard Book Number) 销售数 销售价格"
              << std::endl;
    while (std::cin >> book1 >> book2) {
        if (book1.isbn() != book2.isbn()) {
            std::cerr << "ISBN不相等" << "!" << std::endl;
        }
        else
        std::cout << book1 + book2 << std::endl;
        //输出ISBN 总销售数 总销售额 平均销售价格
    }
    return 0;
}
```
类在头文件中已经定义，编译器不关心头文件的后缀名，但有些IDE会有特定要求
标准库头文件一般不带后缀名

这个程序只能输入两本书，我还以为我有错，记下来，铭记
```C++
#include <iostream>
#include "Sales_item.hpp"

int main() {
    Sales_item total;
    std::cout << "国际标准图书编号(International Standard Book Number) 销售数 销售价格" << std::endl;
    if (std::cin >> total) {
        Sales_item trans;
        std::cout << "国际标准图书编号(International Standard Book Number) 销售数 销售价格" << std::endl;

        while (std::cin >> trans) {
            // 如果我们仍在处理相同的书
            if (total.isbn() == trans.isbn())
                total += trans;
            else {
                // 打印前一本书的结果
                std::cout << "ISBN 总销售数 总销售额 平均销售价格" << std::endl;
                std::cout << total << std::endl;
                total = trans;      // total 现在表示下一本书的销售额
            }
        }
        std::cout << total << std::endl;    // 打印最后一本书的结果
    }
    else {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }
    return 0;
}
```

## 变量和基本类型


**如何选择类型：**

+ 数值不可能为负数时，使用无符号类型
如8bit的unsigned char表示0~255
char则表示-128~127

+ 实际应用中shor常常太小，而long一般和int有一样尺寸
当数值超过int范围应使用long long

+ 在算术表达式中不要使用char或bool，只有在存放字符或布尔值时才使用
因为类型char在一些机器上是有符号，而在另一些机器上又是无符号
所以使用char进行运算特别容易出问题
如果你需要使用一个不大的整数
那么明确指定它的类型是signed char或者unsigned char

+ 执行浮点数运算选用double
这是因为f1oat通常精度不够而且双精度浮点数和单精度浮点数的计算代价相差无几
事实上，对于某些机器来说，双精度运算甚至比单精度还快
long double提供的精度在一般情况下是没有必要的
况且它带来的运行时消耗也不容忽视

**不要混用无符号和带符号类型，如果混用，带符号数会自动转换为无符号数**

以0开始表示八进制，以0x或0X开始表示十六进制
""是字符串字面值，最后有`\0`，''是字符字面值
可以通过前缀和后缀指定字面值类型，本书37页

对象(object)通常指一块能存储数据并具有某种类型的内存空间

初始化是创建变量时赋予其一个初始值
赋值是把对象当前值擦除，以一个新值代替

## 列表初始化

C++语言定义了初始化的好几种不同形式
例如定义一个名为units sold的int变量并初始化为0
以下的4条语句都可以做到这一点：

```C++
int units sold = 0;
int units sold ={0};
int units sold {0};
int units so1d (0);
```

作为C++11新标准的一部分，使用花括号这种初始化的形式被称为列表初始化`(list initialization)`
无论是初始化对象还是某些时候为对象赋新值，都可以使用这样一组由花括号括起来的初始值
当用于内置类型的变量时，这种初始化形式有一个重要特点：
**如果我们使用列表初始化且初始值存在丢失信息的风险，则编译器将报错**

```C
long double ld = 3.1415926536;
int a{ld},b={ld};//编译器会报错
int c(ld),d=ld;  //转换执行，但会丢失数据
```

定义在任何函数体之外的变量被初始化为0
定义在函数体内部的内置类型变量不会初始化
每个类各自决定其初始化对象的方式


**如果想声明一个变量而非定义它，就添加extern关键字且不要显式初始化变量**
`extern int i;`
i在其他文件中定义

**变量命名规范**

+ 见名知意，标识符要能体现实际含义
+ 变量名一般用小写
+ 自定义的类名一般以大写字母开头
+ 如果标识符由多个单词组成，则单词之间用下划线隔开或首字母大写

## 复合类型

compound type指基于其他类型定义的类型

### 引用

reference为对象起了另外一个名字
引用类型引用(refers to)另外一种类型
通过将声明符写成&d的形式来定义引用类型，d是声明的变量名：
```CPP
int ival = 1024;
int& refval = ival;//refval指向ival(ival的另一个名字)
int& refval2;      //error 引用必须初始化
```
定义引用时，程序把引用和它的初始值绑定在一起，而不是将初始值拷贝给引用
一旦初始化完成，引用将和它的初始值对象一直绑定在一起，无法让引用重新绑定到另一个对象
引用不是对象，只是已存在对象的别名

引用只能绑定在对象上，不能和字面值或某个表达式结果绑定在一起
```CPP
int &refval3 = 10;  //error 引用类型的初始值必须是一个对象
double dval = 3.14;
int &refval4 = dval;//error 此处引用类型的初始值必须是一个int对象
```
但有例外情况
第一种例外情况就是在初始化常量引用时允许用任意表达式作为初始值
只要表达式结果能被转换为引用类型即可
```CPP
int i = 42;
const int& r1 = i;
const int& r2 = 42;
const int& r3 = r1 * 2;
int& r4 = r1 * 2;//error 非常量引用
```
对const引用可能引用一个非const对象，会因此改变const对象的值
对非常量的引用初始化只能是左值

### 指针

32位机器上指针占4个字节，64位机器上指针占8个字节
指针本身就是一个对象，允许对指针赋值和拷贝，可以先后指向不同的对象
**指针存放的是对象的地址**
指针无须在定义时赋初值，和其他内置类型一样，在块作用域内的指针没有初始化也将拥有一个不确定的值

空指针
```CPP
int *p1 = nullptr;
int *p2 = NULL;
int *p3 = 0;
```
名为`NULL`的预处理器变量在`<cstdlib>`中定义，值为0
预处理器变量不属于命名空间std
在C++中，`nullptr`是C++11新标准引入的，它是一个特殊类型的字面量，可以被转换成任意类型的指针，应使用这种方式生成空指针

`*`运算符(解引用运算符)
解引用指针将返回该指针指向的对象
对解引用结果赋值也就是对指针指向的对象赋值

一定要初始化所有指针，不清楚指向何处的指针应初始化为`nullptr`

`void*`是一种特殊的指针类型，可用于存放任意对象的地址

引用本身不是一个对象，因此不能定义指向指针的指针
但指针是对象，所以可以对指针进行引用
```CPP
int i = 42;
int* p;
int*& r = p;//r是一个对指针p的引用

r = &i;     //r引用指针p，因此给r赋地址就相当于给p赋地址
*r = 0;     //解引用r得到i，也就是p指向的对象，将i的值改为0
```
类型修饰符(*或&)只作用于它修饰的变量，不是本次定义的全部变量
`int* p1,p2;//p2是int整形变量，不是指针`

**一条比较复杂的指针或引用的声明语句，从左到右阅读更容易弄清含义**

***
## const和constexpr

```CPP
int errNumb = 0;
int* const curErr = &errNumb;
//curBrr将一直指向errNump
const double pi = 3.14159;
const double* const pip = &pi;
//pip是一个指向常量对象的常量指针
```
指向常量的指针所指向对象不一定是常量
指向常量的指针仅表示不能通过该指针来修改它所指向的对象的值
就是其所指向的地址不可被改写，所指对象的值可以被改写

指向常量的常量指针
所指向的值和该指针存储的地址都不能改变

**顶层const(top-level const)表示本身是常量
底层const(low-level const)表示所指的对象是常量**

```CPP
    int i = 9;
    int* const p1=&i;        //不能改变p1的值，这是顶层const
    const int ch = 10;       //不能改变ch的值，这是顶层const
    
    const int* p2 = &ch;     //允许改变p2的值，这是底层const
    const int& r = ch;       //用于声明引用的const都是底层const
    
    const int* const p3 = p2;//靠右是顶层const，靠左是底层const
```
进行拷贝操作时，底层`const`要求对象必须拥有相同的底层`const`资格
或者两个对象的数据类型能够转换

`constexpr`是C++11中新增加的用于指示常量表达式的关键字
它的主要特点是一定会在编译期进行初始化
这意味着`constexpr`修饰的对象或函数必须在编译时就已知其值
而不能在运行时确定
因此，`constexpr`修饰的是一个真正的编译时常量
**声明为`constexpr`的变量一定是一个常量，而且必须用常量表达式初始化**

而`const`关键字在C++中具有两种含义：常量和只读
当我们使用`const`修饰一个对象时，表示该对象的内容是不可修改的，但并不限定这个对象是编译期常量还是运行期常量
`const`只保证了对象的只读性，但并不确保它是编译期的常量
`const`修饰的变量可以在运行时才初始化。

为了解决`const`关键字的双重语义问题，C++11标准中将“只读”的语义保留给`const`，而将“常量”的语义划分给了新添加的`constexpr`关键字

在声明常量变量的情况下，区分两者的功能：
凡是表达“只读”语义的场景都使用`const`，而表达“常量”语义的场景则使用`constexpr`。
**`constexpr`确保了编译期的常量性，而`const`保证了对象的只读性**

```CPP
    const int* p = nullptr; 
//定义了一个指向整型的常量指针，即指针所指向的对象不能被修改，但指针本身可以被修改(指向其他对象)。同时，这个指针可以在运行时进行初始化。
    constexpr int* q = nullptr;
 //定义了一个编译时常量指针，即指针所指向的对象不能被修改，指针本身也不能被修改(指向其他对象)，并且必须在编译时进行初始化。编译器会在编译时就确定其值，而不是在运行时确定。因此，constexpr指针必须在编译时就已经被初始化，否则会导致编译错误
```

指针和引用都能定义为`constexpr`，但初始值严格限制
`constexpr`指针初始值只能是nullptr或0或存储在固定地址中的对象

***

## 类型别名

定义类型别名有两种方式

```CPP
typedef double wage;
//wage是double的别名
typedef wage base, * p;
//base是double的别名，*p是double*的别名

using SI = Sales_item;
//使用别名声明来定义类型别名
//关键字using作为开始，将左侧名字规定为右侧类型的别名

//类型别名和类型名等价
wage hour, week;//double hour,week;
SI item;        //Sales_item item;
```
可以用这个方法把类型名改成中文哈哈哈
不过没什么用...

```CPP
typedef char* ptr;
const ptr p = nullptr;
//const是对给定类型的修饰，ptr是指向char的指针，const ptr是指向char的常量指针
//而不是指向常量对象的指针
const ptr* p;         //指向char常量指针的指针

```
不要直接替换的理解为`const char* p = nullptr;`
前者是声明一个指向char的const指针
后者是声明一个指向const char的指针

## auto类型说明符和decltype说明符

C++11标准引入`auto`类型说明符
它能让编译器自动去分析表达式所属的类型
`auto`让编译器通过初始值来推算变量的类型

```CPP
auto item = a + b;
//item类型由a+b的结果推断
auto i = 0;*p = &i;  //correct：i是整数，p是指向整数的指针
auto a = 5; b = 3.14;//error：a和b类型不一致
```
`auto`一般会忽略顶层const，底层const会保留下来
```CPP
const int ci = i,&cr = ci;
auto b = ci;
//b是一个整数,c1的顶层const特性被忽略掉了
auto c = cr;
//c是一个整数,cr是ci的别名,ci本身是一个顶层const

auto d = &i;
//d是一个整型指针
auto e = &ci;
//e是一个指向整数常量的指针,对常量对象取地址是一种底层const

如果希望推断出的auto类型是一个顶层const，需要明确指出
const auto f = ci;

auto &g = ci;
//g是一个整型常量引用，绑定到ci
auto &h = 42;
//错误：不能为非常量引用绑定字面值
const auto &j = 42;
//正确：可以为常量引用绑定字面值
```
设置`auto`类型的引用时，初始值中的顶层`const`会被保留
如果给初始值绑定引用，此时的常量就不是顶层常量

`decltype`指示符用于获取变量的类型
分析表达式并得到它的类型，不会实际计算表达式的值
`decltype`与`auto`不同
如果`decltype`使用的表达式是变量，`decltype`返回该变量类型(包括顶层const和引用)

```CPP
decltype(fun()) sum = x;
//sum的类型由fun()的返回类型决定
const int ci = 0,&cj = ci;
decltype(ci) x = 0;
//x的类型是const int
decltype(cj) y = x;
//y的类型是const int&，y绑定const变量x
decltype(cj) z;
//error：z的类型是const int&，必须初始化
```
如果`decltype`使用的表达式不是一个变量，则`decltype`返回表达式结果对应的类型
有些表达式将向`decltype`返回一个引用类型,一般来说当这种情况发生时，意味着该表达式的结果对象能作为一条赋值语句的左值：

```CPP
//decltype的结果可以是引用类型
int i = 42,*p = &i,&r = i;
decltype(r+0) b;
//正确：加法的结果是int，因此b是int类型
decltype(*p) c;
//错误：c是int&，必须初始化

```
因为z是一个引用，因此`decltype(r)`的结果是引用类型
如果想让结果类型是所指的类型，可以把r作为表达式的一部分
如r+0，显然这个表达式的结果将是一个具体值而非一个引用

另一方面，如果表达式的内容是解引用操作，则`decltype`将得到引用类型
因为解引用指针可以得到指针所指的对象，而且还能给这个对象赋值
因此，`decltype(*p)`的结果类型就是int&，而非int

**`decltype`的表达式如果是双层括号，其结果一定是引用**
`decltype((i)) d = 0;`
d是int&

***

# 字符串、向量和数组

`std::`比较繁琐，可以使用其他方法
最安全的一种方法就是使用`using`声明
有了`using`声明，就不需要专门的前缀`std::`
`using namespace::std`

每个using声明引入命名空间中的一个成员
`using std::cout;`

**头文件不应该使用using声明，因为如果头文件里有某个using声明，那么使用该头文件的文件就都会有这个声明，可能产生始料未及的名字冲突**

## 标准库类型string

标准库类型`string`表示可变长的字符序列
声明在`<string>`头文件中，定义在命名空间std中

初始化string对象的方式
```CPP
string s1
//默认初始化，s1是一个空串
string s2(s1)
//s2是s1的副本
string s2 = s1
//等价于s2(s1)，s2是s1的副本

string s3("value")
//s3是字面值"value"的副本，除了字面值最后的空字符外
string s3 ="value"
//等价于s3("value")，s3是字面值"value"的副本

string s4(n，'c')
//把s4初始化为由连续n个字符c组成的串
```

**string的操作**

```CPP
os<<s
将s写到输出流os当中，返回os(输出)

is>>s
从is中读取字符串赋给s，字符串以空白分隔，返回is(输入)

getline(is，s)
从is中读取一行赋给s，返回is 

s.size()
返回s中字符的个数

s.empty()
s为空返回true，否则返回false

s[n]
返回s中第n个字符的引用，位置n从0计起

s1+s2
返回s1和s2连接后的结果

s1=s2
用s2的副本代替s1中原来的字符

s1==s2
s1!=s2
如果s1和s2中所含的字符完全一样，则它们相等
string对象的相等性判断对字母的大小写敏感

<,>,<=,>=
利用字符在字典中的顺序进行比较，且对字母的大小写敏感
```
string对象会自动忽略空白从第一个字符开始读取，遇到第一个空白字符停止
```CPP
//反复读取string对象
string word;
while (cin >> word)
    cout << word << endl;

//使用getline读取一整行
string line;
while (getline(cin, line)){
    if(!line.empty())//遇到空行跳过输出
    if(line.size()>10)//只输出大于10字符的行
        cout << line << endl;
}
```
`getline`会保留字符串中的空白字符，读取到换行符停止，但不会存储换行符
`size`返回值的类型是`string::size_type`
在类string中定义，它是无符号整形数
因此不要在一条表达式中同时使用size和int
避免无符号数和有符号数的混用

![](C:\Users\M87monster\Pictures\Screenshots\学习笔记图片\Snipaste_2023-12-02_16-17-59.png)

![Snipaste_2023-12-02_16-17-59](/assets/Snipaste_2023-12-02_16-17-59_9qp6d5yw3.png)

如果想对string对象中的每个字符做点儿什么操作
目前最好的办法是使用C++11新标准提供的一种语句：

## 范围for语句

这种语句遍历给定序列中的每个元素并对序列中的每个值执行某种操作
其语法形式是
```CPP
for(declaration：expression)
statement
```
`expression`是一个对象，用于表示一个序列
`declorarion`部分负责定义一个变量，该变量将被用于访问序列中的基础元素
每次迭代，`declororion`部分的变量会被初始化为`exression`部分的下一个元素值
一个string对象表示一个字符的序列，因此string对象可以作为范围for语句
```CPP
string s("Hello,,,cpp");
    int i = 0;
    for (auto c : s) {
        if (ispunct(c))
            i++;
        cout << c;
    }
    cout<< endl<< i;
    //使用范围for语句和ispunct函数统计标点符号个数

string s("Hello World！！！");
    cout << s << endl;
    for (auto &c : s)  //这里是引用，因此赋值语句会改变c中的值
        c = toupper(c);
    cout << s << endl;
```

访问string对象中的单个字符有两种方式：下标和迭代器

下标运算符`[]`接受的参数是`string::size_type`类型值
string下标从零开始计算
`s[s.size()-1]`就是访问最后一个字符
```CPP
    string s("Hello world！！！");
    for (auto i = 0; i != s.size() && !isspace(s[i]); ++i)
        s[i] = toupper(s[i]);
    cout << s;
    //利用下标将第一个单词改为大写
```
```CPP
    const string _16("0123456789ABCDEF");
    string result;
    int n;
    while (cin >> n)
        if (n < _16.size())
            result += _16[n];
    cout << result << endl;
    //将10进制转换为16进制
```

## 标准库类型vector

标准库类型`vector`表示对象的集合，其中所有的对象类型都相同
集合中的每个对象都有一个与之对应的索引，索引用于访问对象
因为`vector`容纳着其他对象，所以它也被称为容器(container)

使用`vector`需要包含相应的头文件和using声明
```CPP
#include <vector>
using std::vector;
```
C++语言类模板(class template),也有函数模板
其中`vector`是一个类模板

模板本身不是类或函数,可以将模板看作为编译器生成类或函数编写的一份说明
编译器根据模板创建类或函数的过程称为实例化(instantiation)
当使用模板时,需要指出编译器应把类或函数实例化成何种类型

对于类模板来说，我们通过提供一些额外信息来指定模板到底实例化成什么样的类，需要提供哪些信息由模板决定。
以`vector`为例，提供信息的方式总是这样：
即在模板名字后面跟一对尖括号，在括号内放上信息。
```CPP
vector<int> ivec;            //ivec保存int类型对象
vector<Sales_item> Sales_vec;//Sales_vec保存Sales_item类型对象
vector< vector<string> > file; //该向量的元素是vector对象
```

初始化vector 对象的方法
```CPP
vector<T> v1
//v1是一个空vector，它潜在的元素是T类型的，执行默认初始化

vector<T> v2(v1)
//v2中包含有v1所有元素的副本
vector<T> v2 = v1
//等价于v2(v1)，v2中包含有v1所有元素的副本

 vector<T> v3(n，val)
//v3包含了n个重复的元素，每个元素的值都是val
vector<T> v4(n)
//v4包含了n个重复地执行了值初始化的对象

vector<T> v5{a，b，c...}
//v5包含了初始值个数的元素，每个元素被赋予相应的初始值
vector<T> v5={a，b，c...}
//等价于v5{a，b，c...}
```

允许把一个`vector`对象初始化给另一个`vector`对象，但必须具有相同类型

初始化的真实含义依赖于传递初始值时用的是花括号还是圆括号。
通过使用花括号或圆括号可以区分：
```CPP
vector<int> v1(10);
//V1有10个元素，每个的值都是0

vector<int> v2{10}
//v2有1个元素，该元素的值是10

vector<int> v3(10,1);
//v3有10个元素，每个的值都是1

vector<int> v4{10,1};
//v4有2个元素，值分别是10和1
```
圆括号表示提供的值是用来构造vector对象
花括号表示我们要列表初始化该vector对象
```CPP
vector<string> v5{"hi"};
//列表初始化：v5有一个元素
vector<string> v6("hi");
//error:不能使用字符串字面值构建vector对象
vector<string> v7(10,"hi");
//v7有10个元素，每个的值都是"hi"
veetor<string> v8{10};
//v8有10个默认初始化的元素
vector<string> v9{10,"hi"};
//v9有10个值为"hi"的元素
```
如果初始化使用了花括号但提供的值不能列表初始化，编译器将尝试使用默认值初始化`vector`对象
上述例子中只有v5是列表初始化，因为要想列表初始化`vector`对象，花括号中的值必须与元素类型相同

`vector`其实有点像C中的变长数组
使用`vector`的成员函数`push_pack()`向其中添加元素
`push_pack()`负责把一个值当成`vector`对象的尾元素push到`vector`对象的尾端(back)
```CPP
    vector<int> v2;
    for (int i = 0; i != 100; ++i)
        v2.push_back(i);
    for (int i = 0; i != 100; ++i) {
        cout << v2[i] << " ";
        if (i == 42 || i == 82)
            cout << endl;
    }
```
**关键概念：vector对象能高效增长**
C++标准要求`vector`应该能在运行时高效快速地添加元素
既然`vector`对象能高效地增长，那么在定义`vector`对象的时候设定其大小也就没什么必要，事实上如果这么做性能可能更差

只有一种例外情况，就是所有元素的值都一样
一旦元素的值有所不同，更有效的办法是先定义一个空的vector对象，再在运行时向其中添加具体值

<span style="color: red;">warning：</span>
如果循环体内部包含有向`vector`对象添加元素的语句，就不能使用范围for循环
因为能高效便捷地向`vector`对象中添加元素，所以在循环`vector`对象，特别是有可能改变容量时必须要确保所写循环的正确性

## 其它vector操作

**vector支持的操作**
```CPP
v.empty()
//如果不含有任何元素，返回真：否则返回假
v.clear()
//移除vector中的所有元素，使其大小变为0
v.swap(x,y)
//交换两个vector的内容
v.insert(begin(), element)
//在指定位置插入一个或多个元素
//插入多个元素要考虑内存溢出问题
v.emplace(begin(), element)
//可以避免额外的复制或移动操作，从而提高性能    
v.erase(position)
v.erase(begin(), end())    
//删除指定位置的元素或一段范围内的元素
v.resize()
//改变vector的当前大小，可以增加或减少元素，
v.reserve()
//用于预留空间，如果vector大小<n，则会使之大小w为n，>=时不会进行任何操作    
v.size()
//返回中元素的个数
v.push_back(t)
//向v的尾端添加一个值为t的元素
v.pop_back()
//从vector的末尾移除一个元素 
v.at()
//返回指定位置的元素的引用。如果索引越界，它将抛出一个异常   
v.front()
//返回vector的第一个元素的引用。如果vector是空的，它将抛出一个异常   
v.back()
//返回vector的最后一个元素的引用。如果vector是空的，它将抛出一个异常
v[n]
//返回v中第n个位置上元素的引用
v1 = v2
//用v2中元素的拷贝替换v1中的元素
vl ={a，b，c..}
//用列表中元素的拷贝替换v1中的元素
v1== v2
v1!= v2
//v1和v2相等，仅当它们的元素数量相同且对应位置的元素值都相同
<，<-，>，>=
//顾名思义，以字典顺序进行比较
```
- `reserve()`函数只分配内存空间，不会初始化元素；而`resize()`函数会初始化新添加的元素
- `reserve()`函数只能增加`vector`的容量，不能减少容量；而`resize()`函数可以增加或减少`vector`的大小
- `reserve()`函数只是为了提高效率而预留空间，不会影响`vector`的元素个数；而`resize()`函数会改变`vector`的元素个数

在这里`size`返回值的类型是`vecotr::size_type`
要使用`size_type`，需要指定它是由哪种类型定义的:
`vecotr<int>::size_type`
因为`vector`对象类型总是包含着元素的类型

`vector`对象和`string`对象的下标运算符只能用于访问已存在的元素
不能用于添加元素

## 迭代器

所有标准库容器和`string`对象都支持迭代器
大部分容器不支持下标运算符
迭代器`(iterator)`和指针类似，也提供对对象的间接访问
其对象是容器中的元素或`string`对象中的字符
使用迭代器可以访问某个元素，也能从一个元素移动到另外一个元素
有效的迭代器指向某个元素或指向容器中尾元素的下一位置，其他所有情况都属于无效

**有迭代器的类型同时拥有返回迭代器的成员**
`begin()`返回指向第一个元素或字符的迭代器
`end()`返回指向容器或`string`对象尾元素的下一位置的迭代器
`end()`迭代器指向的是一个本不存在的尾后元素，没有实际含义，只是一个标记，表示我们已经处理完所有元素
如果容器为空，`begin()`和`end()`返回的是同一个迭代器，都是尾后迭代器

**标准容器迭代器的运算符**

```CPP
*iter
//返回迭代器iter所指元素的引用
iter->mem
//解引用iter并获取该元素的名为mem的成员，等价于(*iter).mem

//使用迭代器实现将string对象的第一个字符改为大写
    string s("hello");
    if (s.begin() != s.end()) {
        auto it = s.begin();//声明迭代器变量it表并将其初始化为指向s的第一个元素
            //这里的it是string::iterator类型
        *it = toupper(*it);
    }
    cout << s;

++iter
//令iter指示容器中的下一个元素
--iter
//令iter指示容器中的上一个元素

//使用迭代器实现将string对象的第一个单词改为大写
    string s("hello");
    for (auto it = s.begin(); it != s.end() && !isspace(*it); ++it)
        *it = toupper(*it);
    cout << s;


iterl == iter2
iter1 != iter2
/*判断两个迭代器是否相等，如果两个迭代器指示的是同一个元素
或者它们是同一个容器的尾后迭代器，则相等；反之，不相等*/
```
`end()`返回的迭代器是尾后迭代器，它不指向任何元素，所以不能进行递增或递减操作

<span style="color: orange;">关键概念：泛型编程</span>
<span style="color: white;">使用!=是因为所有的标准库容器的迭代器都定义了==和!=，而它们大多数都没有定义<，养成使用迭代器和!=的习惯，就不用太在意使用的是哪种容器类型</span>

拥有迭代器的标准库类型使用`iterator`和`const iterator`来表示迭代器的类型：
```CPP
vector<int>::iterator it;
//it能读写vector<int>的元素
string::iterator it2;
//it2能读写string对象中的字符
vector<int>::const iterator it3;
//it3只能读元素，不能写元素
string::const iterator it4;
//it4只能读字符、不能写字符
```
`vector`对象或`string`对象是常量就只能使用`const iterator`
如果对象不是常量则两种都能使用

`begin()`和`end()`返回的具体类型由对象是否是常量决定
如果对象只需读操作而无需写操作的话最好使用常量类型
C++引入两个新函数：`cbegin()`和`cend()`
它们和`begin()`和`begin()`功能相同，但不论`vector`或`string`对象本身是否是常量都返回`const_iterator`类型值

解引用迭代器会get迭代器所指的对象
如果该对象类型是类，就可以进一步访问它的成员
一个由字符串组成的vector对象，可以用迭代器变量来检查它所指的字符串是否为空
`(*it).empty()`

`->`是C++中的成员访问运算符，用于访问结构体或类的成员
`it->empty()`等价于`(*it).empty()`

任何一种可能改变`vector`对象容量的操作，比如`push_pack()`都会使该`vector`对象的迭代器失效
凡是使用了迭代器的循环体都不要向迭代器所属的容器添加元素

`vector`和`string`迭代器支持的运算
```CPP
iter + n
/*迭代器加上一个整数值仍得一个迭代器，迭代器指示的新位置与原来相比向前移动了若干个元素
结果迭代器或者指示容器内的一个元素，或者指示容器尾元素的下一位置*/
iter - n
/*迭代器减去一个整数值仍得一个迭代器，迭代器指示的新位置与原来相比向后移动了若千个元素
结果迭代器或者指示容器内的一个元素，或者指示容器尾元素的下一位置*/
iter1 += n
//迭代器加法的复合赋值语句，将iter1加n的结果赋给iter1
iter1-= n
//迭代器减法的复合赋值语句，将iter1减n的结果赋给iter1
iter1-iter2
/*两个迭代器相减的结果是它们之间的距离
将运算符右侧的迭代器向前移动差值个元素后将得到左侧的迭代器
参与运算的两个迭代器必须指向的是同一个容器中的元素或者尾元素的下一位置*/
>、>=、<、<=
/*如果一个迭代器指向的容器位置在另一个迭代器所指位置之前，则说前者小于后者
参与运算的两个迭代器必须指向的是同个容器中的元素或者尾元素的下一位置*/
```

使用迭代器运算的一个经典算法是二分查找

```CPP
    vector<int> v;
    int s = 45;
    for (int i = 0; i != 100; ++i)
        v.push_back(i);
    auto beg = v.begin(), end = v.end();
    auto mid = v.begin() + (end - beg) / 2;
    while (mid != end && *mid != s) {
        if (s < *mid)
            end = mid;
        else
            beg = mid + 1;
        mid = beg + (end - beg) / 2;
    }
    cout << *mid;
```

## 数组

```CPP
const unsigned sz = 3;
int ial[sz] = {0,1,2};
//含有3个元素的数组，元素值分别是0，1，2
int a2[] = {0,1,2};
//维度是3的数组
int a3[5] = (0,1,2);
//等价于a3[]=(0，1，2)
string a4[3] = {"hi","bye"};
//等价于a4[]={"hi","bye"}
int a5[2] = (10,1,2);
//error：初始值过多

char al[] = {'c','+','+','\0'};
//列表初始化，含有显式的空字符
char a2[] = "c++";
//自动添加表示字符串结束的空字符
const char a4[6] = "Danie1";
//error：没有空间可存放空字符

//不允许将数组内容拷贝给其他数组作为初始值，也不能用数组为其他数组赋值
int a[] = {0，1，2}；
int a2[] = a;
//error：不允许使用一个数组初始化另一个数组
a2 = a;
//error：不能把一个数组直接赋值给另一个数组

int*ptrs[10];
//ptrs是含有10个整型指针的数组
int &refs[10]=/*？*/;
//错误：不存在引用的数组

int(*parray)[10]= &arr;
//parray指向一个含有10个整数的数组
int(&rarray)[10]= arr;
//rarray引用一个含有10个整数的数组
int *(&array)[10] = ptrs;
//array是数组的引用，该数组含有10个指针
```

数组下标通常定义为`size_t`类型，它是一个无符号整型，在`<cstddef>`头文件中定义
该文件是C标准库`stddef.h`头文件的C++版本

与`vector`和`string`一样，当需要遍历数组所有元素时，最好的办法是使用范围for语句
```CPP
    int a[10];
    for (int i = 0; i < 10; i++)
        a[i] = i;
    for (auto i : a) {
        cout << i << " ";
    }

//如果有条件语句，数组不能使用范围for来判断，vector和string可以使用
vector<int> numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

    for (int number : numbers) {
        if (number % 2 == 0) {
          cout << number << " ";
        }
    }
```
与`vector`和`string`一样，数组下标是否在合理范围由程序员负责检查
大多数常见的安全问题都源于缓冲区溢出错误
当数组或其他类似数据结构的下标越界并试图访问非法内存区域，就会产生此类错误

```CPP
int ia[]={0,1,2,3,4,5,6,7,8,9};
auto ia2(ia);
//尽管ia是由10个整数构成的数组，但使用ia作为初始值时
//编译器实际执行的初始化过程类似于 ia2(&ia[0]) 显然ia2的类型是int*

decltype(ia) ia2;
//但使用decltype关键字时，上述转换不会发生，返回类型是由10个整数构成的数组
```

尾后指针唯一的用处就是提供其地址用于初始化
```CPP
int arr[]={0,1,2,3,4,5,6,7,8,9};
int *e = &arr[10];
for(int *p=arr;p!=e;++p)
cout<<*p<<" ";
```

C++11新标准引入`begin()`和`end()`函数，与容器的同名成员功能类似
但使用方式不同，这两个函数不是成员函数，而是将数组作为它们的参数
```CPP
int arr[] = { 0,1,2,3,4,5,6,7,8,9 };
int* beg = begin(arr);
int* last = end(arr);
//指向尾元素的下一位置的指针，不能执行解引用或地址操作
```

两个指针相减结果的类型是`ptrdiff_t`的标准库类型
`size_t`和`ptrdiff_t`都定义在`cstddef`头文件中，因为差值可能为负，`ptrdiff_t`是带符号类型

**指针运算适用于空指针和所指对象不是数组的指针**

**使用数组实际上是使用指针，因为数组名隐式转换为指向数组第一个元素的指针**

可以使用数组初始化vector对象
```CPP
    int ar[] = {2,3,4,5,6,7,8};
    vector<int> ivec(ar,ar + 6);
    vector<int> ivec1(begin(ar), end(ar));
    for (auto i : ivec)
        cout << i << " ";
        cout << endl;

    for (auto i : ivec1)
        cout << i << " ";
```
<span style="color: RED;">现代C++程序应尽量使用vector和迭代器，避免使用数组和指针，尽量使用string，避免使用C风格基于数组的字符串</span>

***
# 表达式

表达式由一个或多个运算对象组成

`f()+g()*h()+j()`
如果其中有某几个函数影响同一对象，那么它是错误的表达式，产生未定义的行为
优先级只规定运算对象的组合方式，没有规定运算对象按照什么顺序求值
```CPP
(-m)/n == m/(-n) == -(m/n)
m%(-n) == m%n;
(-m)%n == -(m%n);
```

关系运算符返回的是布尔值
<span style="color: orange;">进行比较运算时除非比较对象是布尔类型，否则不要使用布尔字面值`true`和`false`
作为运算对象</span>
如果想测试一个算术对象或指针对象是否为真，最直接的方法是将其作为if语句的条件
```CPP
if(val) //val是任意非0值则为真
if(!val)//val是0则为真
```

**复合运算符只求值一次，普通运算符求值两次**
一次作为右子表达式的一部分求值
一次作为赋值运算的左侧运算对象求值
`a+=b == a=a+b`用复合运算符代替普通运算符

## 前置后置递增递减运算符
前置递增递减运算符：将运算对象加1，然后将改变后的对象作为求值结果
后置递增递减运算符：将运算对象加1，但是求值结果是运算对象改变之前那个值的副本
```CPP
    int i1 = 0, j1;
    int i2 = 0, j2;
    j1 = ++i1;
    cout << j1 << " " << i1 << endl;
    j2 = i2++;
    cout << j2 << " " << i2 << endl;
```

><span style="color: green;">advice：</span>
除非必须，否则不使用递增递减后置运算符
因为前置运算符避免不必要的工作，它把值加1后直接返回改变了的运算对象
而后置运算符将原始值存储以便于返回修改前的值
如果不需要使用修改前的值，后置的操作就是浪费

对于整数和指针类型，编译器有一定的优化，但对复杂的迭代器类型来说，这种额外的工作消耗巨大

```CPP
while (beg!=s.end() && !isspace(*beg))
    *beg = toupper(*beg++);
```
该赋值语句是未定义的，因为赋值运算符左右侧运算对象都使用beg且右侧运算对象改变了beg的值
```CPP
    int a[2] = {2,8};
    int* p = a;
    if (p != 0 && *p++)//执行左侧为真就执行右侧
        cout << *p;
```
***

## 其他运算符

**成员访问运算符**
```CPP
ptr->data == (*ptr).data
```

**条件运算符**

条件运算符也可以嵌套
```CPP
    finalgrade = (grade > 90) ? "high score"
                              : (grade < 60) ? "fail" : "pass";
```
条件运算符优先级非常低，当一条长表达式中嵌套条件运算子表达式时，通常需要加括号
```CPP
    cout << ((grade < 60) ? "fail" : "pass");
    //correct
    cout << (grade < 60) ? "fail" : "pass";
    //输出0或1
    cout << grade < 60 ? "fail" : "pass";
    //error，试图比较cout和60
```

**关于符号位如何处理没有明确规定，所以强烈建议仅将位运算符用于处理无符号类型**

**sizeof运算符**
+ 对char或类型为char的表达式，结果为1
+ 对引用类型，结果为被引用对象所占size
+ 对指针类型，结果为指针本身所占size
+ 对解引用指针，结果为指针指向的对象所占size
+ 对数组，结果为整个数组所占size，sizeof运算不会把数组转换为指针处理
数组大小/单个元素大小就是数组元素的个数
`sizeof(ar)/sizeof(*ar)`
+ 对string对象或vector对象，结果只返回该类型固定部分的大小，不会计算对象中的元素占用多少size

`sizeof`的返回类型是`size_t`类型的常量表达式

**隐式类型转换**
+ 在条件中，非布尔值转换为布尔值
+ 初始化过程中，初始值转换为变量类型
赋值语句中，右侧运算对象转换为左侧运算对象的类型
+ 函数调用时也会发生类型转换

![Snipaste_2023-12-05_15-31-47](/assets/Snipaste_2023-12-05_15-31-47.png)
![Snipaste_2023-12-05_15-32-16](/assets/Snipaste_2023-12-05_15-32-16_g171aod9s.png)

![](C:\Users\M87monster\Pictures\Screenshots\学习笔记图片\Snipaste_2023-12-05_15-31-47.png)
![](C:\Users\M87monster\Pictures\Screenshots\学习笔记图片\Snipaste_2023-12-05_15-32-16.png)

***
# try语句块和异常处理

异常指存在于运行时的反常行为，这些行为超出了函数正常功能的范围
当程序某部分检测到一个它无法处理的问题时，就需要用到异常处理
此时，检测出问题的部分应该发出某种信号以表明程序遇到故障,信号的发出方无须知道故障在何处解决,一旦发出异常信号,检测问题的部分就已经完成了任务

异常处理机制为程序中检测异常和异常处理这两部分的协作提供支持,在C++语言中，异常处理包括：
+ `throw`表达式
异常检测部分使用`throw`表达式来表示它遇到了无法处理的问题，我们说`throw`引发(raise)了异常

+ `try`语句块
异常处理部分使用`try`语句块来处理检测到的异常，`try`语句块以关键字`try`开始,以一个或多个`catch`子句(catch clause)结束
`try`语句块中代码抛出的异常通常会被某个`catch`子句处理
`catch`子句也被称为异常处理代码(exception handler)

+ 一套异常类(exception class)
用于在`throw`表达式和相关的`catch`子句之间传递异常的具体信息

## throw表达式

程序异常检测部分使用`throw`表达式引发一个异常
`throw`表达式包含关键字`throw`和一个表达式
表达式的类型就是抛出的异常类型

```CPP
if(item1.isbn() != item2.isbn())//检查两条数据是否是同一种书籍
    throw runtime_error("Data must refer to same ISBN");
```

## try语句块

```CPP
try {
      program-statement
    }
catch (exception-declaration){//异常声明
        handler-statements
    }
catch (exception-declaration){
        handler-statements
    }
```

**函数在寻找处理代码的过程中退出**

在复杂系统中，程序在遇到抛出异常的代码前，其执行路径可能已经经过了多个`try`语句块
例如，一个`try`语句块可能调用了包含另一个`try`语句块的函数，新的`try`语句块可能调用了包含又一个`try`语句块的新函数，以此类推

寻找处理代码的过程与函数调用链刚好相反
当异常被抛出时，首先搜索抛出该异常的函数。如果没找到匹配的`catch`子句，终止该函数并在调用该函数的函数中继续寻找。如果还是没有找到匹配的`catch`子句，这个新的函数也被终止，继续搜索调用它的函数，以此类推,沿着程序的执行路径逐层回退，直到找到适当类型的`catch`子句为止

如果最终还是没能找到任何匹配的`catch`子句，程序转到名为`terminate`的标准库函数。该函数的行为与系统有关，一般情况下，执行该函数将导致程序非正常退出

对于那些没有任何`try`语句块定义的异常，也按照类似的方式处理：毕竟，没有`try`语句块也就意味着没有匹配的`catch`子句。如果一段程序没有`try`语句块且发生了异常，系统会调用`terminate`函数并终止当前程序的执行

**`tip`：编写异常安全的代码非常困难**
要好好理解这句话：异常中断了程序的正常流程
异常发生时，调用者请求的一部分计算可能已经完成了，另一部分则尚未完成。
通常情况下，略过部分程序意味着某些对象处理到一半就戛然而止，从而导致对象处于无效或未完成的状态，或者资源没有正常释放等等
那些在异常发生期间正确执行了“清理”工作的程序被称作异常安全(exception safe)的代码。然而经验表明，编写异常安全的代码非常困难

对于一些程序来说，当异常发生时只是简单地终止程序。此时，我们不怎么需要担心异常安全的问题
但是对于那些确实要处理异常并继续执行的程序。就要加倍注意了。我们必须时刻清楚异常何时发生，异常发生后程序应如何确保对象有效、资源无泄漏、程序处于合理状态等等

## 标准异常

C++标准库定义了一组类用于报告标准库函数遇到的问题，它们分别定义在4个头文件中：
+ `exception`头文件定义最通用的异常类`exception`
  它只报告异常的发生，不提供任何额外信息

+ `stdexcept`头文件定义几种常用的异常类

**stdexcept定义的异常类**
|    `exception`     |                  最常见的问题                  |
| :----------------: | :--------------------------------------------: |
|  `runtime error`   |          只有在运行时才能检测出的问题          |
|   `range error`    |  运行时错误：生成的结果超出了有意义的值域范围  |
|  `overflow error`  |              运行时错误：计算上溢              |
| `underflow error`  |              运行时错误：计算下溢              |
|   `logic_error`    |                  程序逻辑错误                  |
|   `domain error`   |        逻辑错误：参数对应的结果值不存在        |
| `invalid argument` |               逻辑错误：无效参数               |
|   `lenqth error`   | 逻辑错误：试图创建一个超出该类型最大长度的对象 |
|   `out of range`   |       逻辑错误：使用一个超出有效范围的值       |

+ `new`头文件定义了`bad_alloc`异常类型

+ `type_info`头文件定义了`bad_cast`异常类型

只能以默认初始化的方式初始化`exception`、`bad_alloc`、`bad_cast`对象，不能为它们提供初始值
其它异常类型则恰好相反：应该使用`string`对象或者C风格字符串初始化这些类型的对象，但是不允许使用默认初始化的方式
当创建此类对象时，必须提供初始值，该初始值含有错误相关的信息

每个标准库异常类都定义了名为`what()`的成员函数，该函数没有参数，返回值是C风格字符串(`const char*`)
异常类型只有名为`what()`的成员函数
该函数返回的C风格字符串内容与异常对象的类型有关
如果异常类型有一个字符串初始值，则`what()`返回该字符串
对于其他无初始值的异常类型，`what()`返回的内容由编译器决定

```CPP
int i1, i2;
while (cin >> i1 >> i2)
{
	try
	{
		if (i2 == 0)
		{
			throw runtime_error("divisor can't be 0");
		}
		cout << i1 / i2 << endl;
	}
	catch (runtime_error err)
	{
		cout << err.what() << "\ntry again? enter y or n" << endl;
		char c;
		cin >> c;
		if (cin || c == 'n') break;
	}
}
```
***
# 函数

函数由返回类型、函数名、参数列表和函数体组成

形参在函数的定义中声明
实参是形参的初始值

函数返回类型不能是数组或函数，但可以是指向数组或函数的指针或引用

返回引用实际返回的是一个指向返回值的隐式指针，在内存中不会产生副本，是直接拷贝，这样就避免产生临时变量，相比返回普通类型的执行效率更高，而且这个返回引用的函数也可以作为赋值运算符的左操作数，但是这时候需要注意以下两个问题：

1. 千万别返回局部对象的引用，当函数执行完局部对象也就被销毁，栈空间被释放，从而返回的地址已经不存在，导致后面执行出错。

2. 返回堆区对象的引用，这种情况要特别注意，这时候返回函数的引用是作为一个临时变量出现，没有将它赋值给一个实际存在的变量那么这个堆区对象的内存空间就没有释放，可能造成内存泄漏。有人说这样做是非法的？其实不是绝对的，只能说这种编程习惯很不好，这样做只是容易造成内存泄漏，只是我们要记住，我们在申请堆内存以后必须记得去释放这块内存。

+ 名字的作用域是程序文本的一部分，名字在其中可见
+ 对象的生命周期是程序执行过程中该对象存在的一段时间

形参和函数体内部定义的变量都是local variable，仅在函数作用域内可见，同时局部变量还会隐藏在外层作用域中同名的其他所有声明中

**局部变量**
将局部变量定义为`static`类型从而获得局部静态变量
局部静态对象在程序的执行路径第一次经过对象定义语句时初始化，直到程序终止时才被销毁，在此期间即使对象所在函数结束执行也不会对它产生影响

**参数传递**
1. 传值参数
函数调用时将实参复制给形参，实参和形参是两个不同的对象，函数对形参的所有操作都不会影响实参

2. 传指针形参
指针行为和其他非引用类型一样，当执行指针拷贝操作时，拷贝的是指针的值
在C++中，建议使用引用形参代替指针形参


3. 传引用形参
拷贝大的类类型对象或容器对象比较低效，且有的类类型不支持拷贝
当某种类型不支持拷贝操作时，函数只能通过引用形参访问该类型的对象
当函数无需修改引用形参的值时使用常量引用

**使用引用形参返回额外信息**

定义一个名为`find_char`的函数，它返回在`string`对象中某个指定字符第一次出现的位置，也返回该字符出现的总次数
一种方法是定义一个新的数据类型，让它包含位置和数量两个成员
更简单的方法是，给函数传入一个额外的引用实参
```CPP
int main(void) {
    string s = "helloworld";
    char c = 'o';
    string::size_type occurs;
    string::size_type last_occurrence = find_char(s, c, occurs);
    cout << "字符 " << c << " 在字符串中出现了 " << occurs << " 次，最后一次出现在位置 " << last_occurrence << endl;
    return 0;
}
string::size_type find_char(const string& s, char c,
                            string::size_type& occurs) {
    auto ret = s.size(); //第一次出现位置
    occurs = 0;          //设置表示出现次数形参的值
    for (decltype(ret) i = 0; i != s.size(); ++i) {
        if (s[i] == c) {
            if (ret == s.size())
                ret = i; //记录c第一次出现位置              
            ++occurs;//出现次数加1
        }
    }
    return ret;
}
```

## 常量和非常量形参

和其他初始化过程一样，当用实参初始化形参时会忽略顶层const，也就是说，形参的顶层const被忽略掉了
当形参有顶层const时，传常量和非常量对象都是允许的

***可以使用非常量初始化一个底层const对象，但不允许底层const对象初始化一个非常量***
一个普通引用必须用同类型对象初始化

```CPP
int i = 42;
const int*cp=&i;
//正确：但是cp不能改变i
const int&r = i;
//正确：但是r不能改变i
const int &r2 = 42;
//正确
int *p = cp;
//错误：p的类型和cp的类型不匹配
int &r3 = r;
//错误：r3的类型和r的类型不匹配
int &r4 = 42;
//错误：不能用字面值初始化一个非常量引用

将同样的初始化规则应用到参数传递上可得如下形式:
void reset(int &i){
    i=0;
}
int i = 0;
const int ci = i;
string::size_type ctr = 0;

reset(&i);
//调用形参类型是int*的reset函数
reset(&ci);
//错误：不能用指向const int对象的指针初始化int*
reset(i);
//调用形参类型是int&的reset函数
reset(ci);
//错误：不能把普通引用绑定到const对象ci上
reset(42);
//错误：不能把普通引用绑定到字面值上
reset(ctr);
//错误：类型不匹配，ctr是无符号类型
find_char("Hello World！"，'o'，ctr);
//正确：find char的第一个形参是对常量的引用
```


**尽量使用常量引用**
把函数不会改变的形参定义成普通引用是常见错误
使用普通引用而非常量引用也会极大地限制函数所能接受的实参类型
不能把const对象、字面值或需要类型转换的对象传递给普通引用形参

将`find_char`函数的`string`形参定义成常量引用
```CPP
string::size_type find_char( string& s, char c,
                            string::size_type& occurs)
```
则只能将`find_char`函数作用于`string`对象
类似`find_char("Hello World！"，'o'，ctr);`这样的调用将在编译时发生错误

还有一个难以察觉的问题，假如其他函数将它们的形参定义成常量引用，那么第二个版本的`find_char`无法在此类函数中正常使用

## 数组形参

数组的两个特殊性质对我们定义和使用作用在数组上的函数有影响
1. 不允许拷贝数组
2. 使用数组时通常会将其转换成指针
使用`decltype`关键字返回数组类型时，数组不会被转换为指针
数组作为`sizeof`运算符的操作数，也不会发生转换

```CPP
//尽管形式不同，但这三个print函数是等价的
//每个函数都有一个const int*类型的形参
void print(int*);
void print(int[]);
void print(int[10]);//这里的维度表示我们期望数组含有多少元素，实际不一定

int i = 0,j[2] = {0,1};
print(&i);//&i是int*类型
print(j); //j转换成int*类型并指向j[0]
```
当为函数传递一个数组时，实际上传递的是指向数组首元素的指针

因为数组以指针形式传递给函数，所以一开始函数并并不知道数组的确切尺寸，需要提供一些额外的信息，管理指针形参有三种常用技术

**使用标记指定数组长度**

要求数组本身包含一个结束标记，使用这种方法的典型示例是C风格字符串
C风格字符串存储在字符数组中，最后一个字符是空字符，函数在处理C风格字符串时遇到空字符停止

```CPP
void print(const char* p) {
    if (p)
        while (*p)
            cout << *p++;
}
```
这种方法适用于有明显结束标记且该标记不会与普通数据混淆的情况
对于int这种所有取值都合法的数据显然不太有效

**使用标准库规范**

传递指向数组首元素和尾后元素的指针
```CPP
void print(const int* beg, const int* end) {
    while (beg != end)
        cout << *beg++ << endl;
}

//调用该函数需要传入两个指针
//一个指向要输出的首元素，一个指向尾元素的下一位置
int j[] = {0,1};
print(begin(j), end(j));
```

**显式传递一个表示数组大小的形参**

专门定义一个表示数组大小的形参
```CPP
void print(const int ia[], size_t size) {
    for (size_t i = 0; i != size; ++i)
        cout << ia[i] << endl;
}
```

3个print函数数组形参都是指向常量指针，关于引用的讨论同样适用于指针
当函数不需要对数组元素执行写操作的时候，数组形参应该是指向`const`的指针，只有当函数确实要改变元素值的时候，才把形参定义成指向非常量的指针

**数组引用形参**

C++允许变量定义为数组的引用，形参也可以是数组的引用，此时引用形参绑定到对应的实参也就是数组
```CPP
f(int& arr[10]); //error arr声明为引用的数组，没有引用数组
f(int(&arr)[10]);//correct arr声明为一个指向长度为10的整型数组的引用
```

**传递多维数组**

将多维数组传递给函数时，实际传递的是指向数组首元素的指针，因为处理的是数组的数组，所以首元素本身就是一个数组，指针就是一个指向数组的指针
数组第二维及后面所有维度大小都是数组类型的一部分，不能省略
```CPP
int arr[10][20];
int (*ptr)[20] = &arr;
//声明一个指向arr的指针，该指针指向一个包含20个整数的数组

int *matrix[10];  //10个指针组成的数组
int (*matrix)[10];//指向含有10个整数的数组的指针

void print(int (*matrix)[10],int rowsize);
void print(int *matrix[][10],int rowsize);
//等价定义，编译器会忽略第一个维度，实际是指向含有10个整数的数组的指针
```
**指针的引用**
```CPP
void swap_intp(int *&i, int *&j) {
    int temp = *i;
    *i = *j;
    *j = temp;
}

int main() {
    int x = 5;
    int y = 10;
    int *ptrX = &x;
    int *ptrY = &y;

    std::cout << "Before swapping: " << *ptrX << ", " << *ptrY << std::endl;
    swap_intp(ptrX, ptrY);
    std::cout << "After swapping: " << *ptrX << ", " << *ptrY << std::endl;

    return 0;
}

void swap_intp(int&* i, int&* j);
//error 不允许使用指向引用的指针
void swap_intp(int*& i, int*& j);
//接受两个整数指针的引用作为参数
```

## main:处理命令行选项

假定`main`函数位于可执行文件`prog`之内，我们可以向程序传递下面的选项：
`prog -d -o ofile data0`
这些命令行选项通过两个可选的形参传递给`main`函数：
```CPP
int main(int argc, char *argv[])
```
第一个形参`argc`表示数组中字符串的数量
第二个形参`argv`是一个数组，它的元素是指向C风格字符串的指针

当实参传给`main`函数之后，`argv`的第一个元素指向程序的名字或一个空字符串，接下来的元素依次传递命令行提供的实参，最后一个指针之后的元素值保证为0
```CPP
argv[0] = "prog";//也可以指向一个空字符串
argv[1] = "-d";
argv[2] = "-o";
argv[3] = "ofile";
argv[4] = "data0";
argv[5] = 0;
```
当使用`argv`中的实参时，一定要记得可选的实参从`argv[1]`开始，`argv[0]`保存程序的名字，而非用户输入

## 含有可变参数的函数

C++11新标准提供了两种主要的方法：
如果所有的实参类型相同，可以传递一个名为`initializer_list`的标准库类型
如果实参的类型不同，可以编写一种特殊的函数，也就是所谓的可变参数模板

C++还有一种特殊的形参类型(省略符)，可以用它传递可变数量的实参
这种功能一般只用于与C函数交互的接口程序


```CPP
initializer_list<T> lst;
//默认初始化，T类型元素的空列表  
 initializer_list<T> lst{a,b,c...};  
//lst的元素数量和初始化一样多，lst的元素是对应初始值的副本；列表中的元素是const

lst2(lst)
lst2=lst
//拷贝或赋值一个initializer_list对象，不会拷贝列表中的元素，拷贝后，原始列表和副本共享元素

lst.size()
//列表中的元素数量 

lst.begin()
//返回指向lst中首元素的指针

lst.end()
//返回指向lst中尾元素下一位置的指针
```

和`vector`一样，`initializer_list`也是一种模板类型
定义`initializer_list`对象时必须说明列表中所含元素的类型
和`vector`不同的是，`initializer_list`对象中的元素永远是常量值

使用如下形式编写输出错误的函数，可以作用于可变数量的实参
```CPP
void error_msg(initializer_list<string> il){
	for(auto beg=il.begin();beg!=il.end();++beg){
		cout<<*beg<<" ";
    cout<<endl;
}
}
```

如果想向`initializer_list`形参中传递一个值的序列，则必须把序列放在一对花括号中：
```CPP
    if (expected != actual)
        error_msg({ "functionX",expected,actual });
    else
        error_msg({ "functionX","OKAY"});
```

含有`initializer_list`形参的函数也可以拥有其他形参
例如调试系统可能有一个名为`ErrCode`的类用于表示不同类型的错误

```CPP
void error_msg(ErrCode e, initializer_list<string> il) {
    cout << e.msg() << ":";
    for (const auto& elem : il)
        cout << elem << " ";
    cout << endl;
}
```
调用这个函数需要额外传递一个ErrCode实参
```CPP
    if (expected != actual)
        error_msg(ErrCode(42), { "functionX",expected,actual });
    else
        error_msg(ErrCode(0).{"functionX", "okay"});

```

**省略符形参**

省略符形参只能出现在形参列表的最后一个位置
省略符形参应该仅仅只用于C和C++通用的类型
大多数类类型的对象在传递给省略符形参时都无法正确拷贝

***

## 返回类型和return语句

没有返回值的`return`语句只能用于返回类型为`void`的函数中
返回`void`的函数最后一句后面会隐式地执行`return`

返回类型`void`的函数能且只能返回另一个`void`类型的函数

有返回值函数`return`类型必须与函数返回类型相同或能隐式转换成函数的返回类型

返回一个值的方式和初始化一个变量或形参的方式完全相同：
返回的值用于初始化调用点的一个临时量，该临时量就是函数调用结果

**和其他引用类型一样，如果函数返回引用，该引用只是它所引对象的一个别名**
```CPP
const string& shorterString(const string& s1, const string& s2) {
    return s1.size() <= s2.size() ? s1 : s2;
}
```
形参和返回类型都是`const string`的引用，不管是调用函数还是返回结果都不会真正拷贝`string`对象

<span style="color: orange;">不要返回局部对象的引用或指针</span>

函数完成后，它所占的存储空间也随之被释放
返回局部对象的引用和指针都是错误的，一旦函数完成，局部对象就被释放，指针将指向一个不存在的对象，引用的行为是未定义的

**右值：取不到地址的表达式
左值：能取到地址的表达式**

左值可以当做右值，实际使用的是它的内容(值)，但不能把右值当做左值

+ 取地址符作用于一个左值运算对象，返回一个指向该运算对象的指针，这个指针是一个右值

+ 内置解引用运算符、下标运算符、迭代器解引用运算符、`string`和`vector`的下标运算符的求值结果都是左值

+ 如果表达式求值结果是左值，`decltype`作用于该表达式(不是变量)得到一个引用类型

  例如，对于`int*p`

  + 解引用运算符生成左值，所以`decltype(*p)`的结果是`int&`
  + 取地址符运算符生成右值，所以`decltype(&p)`的结果是`int**`

调用一个引用的函数得到左值，其他返回类型得到右值
可以像使用左值一样使用返回引用函数的调用

```CPP
char& get_val(string& str, string::size_type ix) {
    return str[ix];
}
int main(void) {
    string s("a value");
    cout << s << endl;
    get_val(s, 0) = 'A';
    cout << s << endl;
    return 0;
}
```
函数返回值是一个引用，因此调用是一个左值，和其他左值一样它也能出现在赋值运算符的左侧

**列表初始化返回值**

C++11规定，函数可以返回花括号包围的值的列表
此处的列表也可以用来对表示函数返回的临时量进行初始化
如果列表为空，临时量执行值初始化，否则返回的值由函数返回类型决定
`error_msg函数`，该函数的输入是一组可变数量的`string`实参，输出由这些`string`对象组成的错误信息。在下面的函数中，返回一个`vector`对象，用它存放表示错误信息的`string`对象：

```CPP
vector<string> process()
//expected 和 actual是string对象
if (expected.empty())
    return{};//返回一个空vector对象
else if (expected == actual)
    return {"functionx","okay"};//返回列表初始化的vector对象
else
    return {"funetionx",expected,actual};
```
如果函数返回的是内置类型，则花括号包围的列表最多包含一个值，而且该值所占空间不应该大于目标类型的空间
如果函数返回的是类类型，由类本身定义初始值如何使用

**主函数main的返回值**

允许`main`没有`return`语句直接结束，编译器将隐式地插入一条返回0的`return`语句
`main`函数的返回值可以看做是状态指示器，返回0表示成功，返回其他值表示失败
非0值具体含义依机器而定，为了使返回值与机器无关，`cstdlib`头文件定义了两个预处理器变量
分别表示失败和成功：`EXIT_FAILURE`和`EXIT_SCUESS`

**返回数组指针**

数组不能被拷贝，所以函数不能返回数组，但函数可以返回数组的指针或引用

定义一个返回数组的指针或引用的函数比较繁琐，最直接的简化方法是使用类型别名
```CPP
typedef int arrT[10];
using arrT = int[10];
//arrT是一个类型别名，它表示的类型是含有10个整数的数组
arrT* func(int i);
//func返回一个指向含有10个整数的数组的指针

int arr[10];
int* p1[10];
//p1是含有10个指针的数组
int(*p2)[10] = &arr;
//p2是一个指向含有10个整数的数组的指针
```

```CPP
int (*func(int i))[10];
//这个声明表示定义了一个函数指针变量
//该变量可以指向一个接受整型参数并返回大小为10的整型数组的函数

fun(int i)
//表示调用fun函数式需要一个int类型的实参

(*fun(int i))
//意味着可以对函数调用结果实行解引用操作

(*fun(int i))[10]
//意味着解引用func的调用将得到一个大小是10的数组

int (*func(int i))[10]
//表示数组中的元素是int类型
```

**使用尾置返回类型**

在C++11新标准中还有一种方法可以简化，就是使用尾置返回类型`(trailing return type)`
任何函数的定义都能使用尾置返回，但这种形式对于返回类型比较复杂的函数最有效
比如返回类型是数组的指针或数组的引用
尾置返回类型跟在形参列表后面并以`->`符号开头
为了表示函数真正的返回类型在形参列表之后，在本应出现返回类型的地方放置一个`auto`
```CPP
auto func(int i) -> int(*)[10];
//func接受一个int类型实参，返回一个指向含有10个int元素的整型数组的指针
```

**使用decltype**

如果我们知道函数返回的指针指向哪个数组，就可以使用`decltype`关键字声明返回类型
```CPP
int odd[] = { 1,3,5,7,9 };
int even[] = { 2,4,6,8,10 };

decltype(odd)* arrPtr(int i);
int(*arrPtr1(int i))[5];

int main(void) {
    cout << arrPtr(8)<<endl;
    cout << arrPtr1(8);
}
decltype(odd)* arrPtr(int i) {
    return (i % 2) ? &even : &odd;
}

int(*arrPtr1(int i))[5] {
    return (i % 2) ? &even : &odd;
}
```
`arrPtr`使用关键字`decltype`表示它的返回类型是一个指针，并且该指针所指对象与`odd`类型相同，所以`arrPtr`返回一个指向含有5个整数的数组的指针
`decltype`并不负责把数组类型转换成对应的指针，它的结果是一个数组，要想表示`arrPtr`返回指针必须在函数声明时加一个`*`

```CPP
string(&fun(string(&arr)[10]))[10];

typedef string(&arrT1)[10];
arrT1& fun(arrT1& arr);

using arrT2 = string[10];
arrT2& fun(arrT2& arr);

auto fun(string(&arrT3)[10]) -> string(&)[10];

string text[10];
decltype(text)& fun(decltype(text) &arrT4);
//使用四种方式写的函数声明
//该函数接受一个指向string数组的引用作为参数，并返回一个指向string数组的引用
```
真nm的绕

## 函数重载

如果同一个作用域内的几个函数名字相同但形参列表不同，则称为重载函数`(overloaded)`
main函数不能重载
调用同名函数时，编译器会根据传递的实参类型推断是哪个函数
函数名只是让编译器知道它调用的是哪个函数，而函数重载可以一定程度上减轻程序员起名记名的负担

**不允许两个函数除返回类型外其他所有部分都相同**

顶层`const`不影响传入函数的对象，所以一个拥有顶层`const`的形参和另一个没有顶层`const`的形参无法区分
```CPP
Record 1ookup(Phone);
Record lookup(const Phone);//重复声明Record 1ookup(Phone)
Record lookup(Phone*);
Record 1ookup(Phone* const);//重复声明Record 1ookup(Phone*)
```

如果形参是某种类型的指针或引用，则可以通过区分其指向的是常量对象还是非常量对象来实现函数重载，此时是底层`const`
```CPP
Record 1ookup(Account&);
Record lookup(const Account&);

Record lookup(Aecount*);
Record 1ookup(const Account*);
//4个独立的重载函数
```

因为`const`不能转换成其他类型，所以只能把`const`对象或指向`const`的指针传递给`const`形参
而非常量可以转换成`const`，所以上面的4个函数都能作用于非常量对象或指向非常量对象的指针
但当传递一个非常量对象或指向非常量对象的指针时，编译器会优先选用非常量版本的函数

`const_cast`只能改变运算对象的底层`const`
将常量对象转换成非常量对象的行为称之为去掉`const`性质`(cast away the const)`
如果对象本身不是一个常量，使用强制类型转换获得写权限是合法行为
如果对象本身是一个常量，使用`const_cast`执行写操作是未定义行为

只有`const_cast`能改变表达式的常量属性，使用其他形式的命名强制类型转换表达式的常量属性都会引发编译器错误，同样也不能使用`const_cast`改变表达式的类型

`const_cast`在函数重载的情景中最有用

```CPP
const string& shorterString(const string& s1, const string& s2) {
    return s1.size() <= s2.size() ? s1 : s2;
}
```
这个函数的参数和返回类型都是`const string`的引用
可以对两个非常量的`string`实参调用这个函数，返回结果仍然是`const string`的引用
如果我们需要当它的实参不是常量时，得到的结果是一个普通的引用，使用`const_cast`可以做到
```CPP
string& shorterString(string& s1, string& s2) {
    auto& r = shorterString(const_cast<const string&>(s1),
                            const_cast<const string&>(s2));
    return const_cast<string&>(r);
}
```
首先将函数的实参强制转换成对`const`的引用，然后调用了`shorterString`函数的`const`版本
`const`版本返回对`const string`的引用，这个引用实际判定在某个初始的非常量实参上
因此可以将其转换回一个普通的`string&`

### 调用重载的函数

函数匹配`(function matching)`指一个过程，在这个过程中把函数调用与一组重载函数中的某一个关联起来，因此函数匹配也叫重载确定`(overload resolution)`
编译器首先将调用的实参与重载集合中的每一个函数的形参进行比较，根据比较结果决定调用哪个函数

在很多情况下，程序员很容易判断某次调用是否合法，以及当调用合法时应该调用哪个函数。通常，重载集中的函数区别明显，它们要不然是参数的数量不同，要不就是参数类型毫无关系。此时，确定调用哪个函数比较容易
但是在另外一些情况下要想洗择函数就比较困难了，比如当两个重载函数参数数量相同且参数类型可以相互转换时

当调用重载函数时有三种可能的结果：
+ 编译器找到一个与实参最佳匹配`(best match)`的函数，并生成调用该函数的代码
+ 找不到任何一个函数与调用的实参匹配，此时编译器发出无匹配`(no match)`的错误信息
+ 有多于一个函数可以匹配但无最佳匹配函数，此时也会发生错误，称为二义性调用`(ambigous call)`

### 重载与作用域

重载对作用域的一般性质并没有什么改变，如果在内层作用域中声明名字，它将隐藏外层作用域中声明的同名实体
```CPP
string read();
void print(const string &);
void print(double);

void fooBar(int iva1){
    bool read = false;//隐藏了外层的read
    string s = read();//error，read是一个布尔值，不是函数
    
    void print(int); //隐藏了外层的print
    print("Value: ");//error，print(const string &)被隐藏
    print(iva1);     //当前print(int)可见
    print(3.14);     //调用print(int)

}
```
**在不同的作用域中无法重载函数名，只会隐藏**
当我们调用`print`函数时，编译器首先寻找该函数名的声明，找到的是接受int的局部声明，一旦在当前作用域中找到所需名字，编译器就会忽略掉外层作用域中的同名实体，剩下的工作就是检查函数调用是否有效

在C++语言中，名字查找发生在类型检查之前
***
## 特殊用途语言特性

### 默认实参

某些函数有一种形参，在函数的多次调用中它们都被赋予一个相同值，这个反复出现的值称为函数的默认实参`(default argument)`，调用含有默认实参的函数时，可以包含该实参，也可以省略该实参

例如使用`string`对象表示窗口的内容。一般情况下，希望该窗口的高宽和背景字符都使用默认值。但是同时也应该允许用户为这几个参数自由指定数值。为了使得窗口函数既能接纳默认值，也能接受用户指定的值，可以把它定义成如下的形式:
```CPP
typedef string::size_type sz;
string screen(sz ht = 24, sz wid = 80, char backgrnd = ' ');
```
默认实参作为形参的初始值出现在形参列表中，可以为一个或多个形参定义默认值，但**一旦某个形参被赋予默认值，它后面的所有形参都必须有默认值**

如果想要**使用默认实参**，只需在调用函数时省略该实参即可
默认实参负责填补函数调用缺少的尾部参数
```CPP
string window;
window = screen();
window = screen(66);
window = screen(66,256);
window = screen(66,256,'#');

window = screen(,,'?');//error，只能省略尾部实参
window = screen('?');  //调用screen('?',80,' ')
```
最后一个调用传递了一个字符值，是合法的调用，但实际效果与实际意图不同，`char`类型会隐式转换成`string::size_type`，然后作为height的值传递给函数

局部变量不能作为默认实参，除此之外，只要表达式类型能转换成形参所需类型，该表达式就能作为默认实参

### 内联函数和constexpr函数

把规模较小的操作定函数有许多好处：
+ 阅读和理解小函数的调用要比读懂等价的条件表达式容易得多

+ 使用函数可以确保行为的统一，每次相关操作都能保证按照同样的方式进行
+ 如果我们需要修改计算过程，修改函数要比先找到表达式所有出现的地方再逐一修改更容易

+ 函数可以被其他应用重复利用，省去了程序员重新编写的代价

但调用函数有一个缺点：调用函数比表达式求值要慢
**内联函数可以避免函数调用的开销**
将函数指定为内联函数`(inline)`通常就是将它在每个调用点上“内联地”展开，如果将`shorterString`函数定义为内联函数
```CPP
cout << shorterString(s1, s2) << endl;
```
将在编译过程中展开成类似于下面的形式
```CPP
cout << (s1.size() < s2.size() ? s1 : s2) << endl;
```
从而消除`shorterString`函数的运行时开销
在函数的返回类型前面加上关键字`inline`就可以将它声明为内联函数

内联说明只是向编译器发出的请求，编译器可以忽略这个请求

**constexpr函数**
`constexpr`函数指能用于常量表达式的函数，定义`constexpr`函数的方法与其他函数类似，不过要遵循几项约定：
**函数的返回类型和所有形参类型都是字面值类型，并且函数体中有且只有一条`return`语句：**
```CPP
constexpr int new_sz() { return 42; };
constexpr int foo = new_sz();
```
执行初始化任务时，编译器把对`constexpr`函数的调用替换成其结果值，为了能在编译过程中随时展开，**`constexpr`函数被隐式地指定为内联函数**

`constexpr`函数体内也可以包含其他语句，只要这些语句在运行时不执行任何操作就行。例如，`constexpr`函数中可以有空语句、类型别名以及`using`声明

允许`constexpr`函数的返回值并非一个常量
如果用一个非常量表达式调用`constexpr`函数,则返回值是一个非常量表达式

**把内联函数和`constexpr`函数放在头文件内**
和其他函数不一样，内联函数和`constexpr`函数可以在程序中多次定义
因为编译器要想展开函数仅有函数声明是不够的，还需要函数的定义
不过对于某个给定的内联函数或者`constexpr`函数来说，它的多个定义必须完全一致。基于这个原因，内联函数和`constexpr`函数通常定义在头文件中

## 调试帮助

C++程序员有时会用到一种类似头文件保护的技术，以便有选择地执行调试代码
基本思想是，程序可以包含一些用于调试的代码，但是这些代码只在开发程序时使用
当应用程序编写完成准备发布时，要先屏蔽掉调试代码。这种方法用到两项预处理功能:`assert`和`NDEBUG`

**assert预处理宏**

`assert`是一种预处理宏`(preprocessor marco)`，预处理宏是一个预处理器变量，行为类似于内联函数
`assert`宏使用一个表达式作为它的条件：
`assert(expr);`
首先对`expr`求值，如果表达式为假，`assert`输出信息并终止程序的执行
如果表达式为真，`assert`无行为

`assert`宏定义在`cassert`头文件中
预处理名由预处理器而非编译器管理，因此无需提供`using`声明，使用`assert`而非`std::assert`
和预处理器变量一样，宏名字在程序内必须唯一。含有`cassert`头文件的程序不能再定义名为`assert`的变量、函数或其他实体
在实际编程过程中，即使没有包含`cassert`头文件，也最好不要为了其他目的使用`cassert`
很多头文件都包含了`cassert`，这就意味着即使你没有直接包含 `cassert`，它也很有可能通过其他途径包含在你的程序中

`assert`宏常用于检查“不能发生”的条件。例如，一个对输入文本进行操作的程序可能要求所有给定单词的长度都大于某个阈值。此时，程序可以包含一条如下所示的语句:

```CPP
assert(word,size() > threshold);
```

**NOEBUG预处理器**

`assert`的行为依赖于一个名为`NDEBUG`的预处理变量的状态。如果定义了`NDEBUG`,则`assert`什么也不做。
默认状态下没有定义`NDEBUG`，此时`assert`将执行运行时检查

可以使用一个`#define`语句定义`NDEBUG`，从而**关闭调试状态**。同时，很多编译器都提供了一个命令行选项使我们可以定义预处理变量:
`$ CC -D NDEBUG main.c # use /D with the Microsoft compiler`
这条命令作用等价于在`main.c`文件的开头写`#define NOEBUG`

定义`NDEBUG`能避免检查各种条件所需的运行时开销，当然此时根本就不会执行运行时检查
因此，`assert`应该仅用于验证那些确实不可能发生的事情。我们可以把`assert`当成调试程序的一种辅助手段，但是不能用它替代真正的运行时逻辑检查，也不能替代程序本身应该包含的错误检查

除了用于`assert`外，也可以使用`NDEBUG`编写自己的条件调试代码。如果`NDEBUG`未定义，将执行`#fndef`和`#endif`之间的代码：如果定义了`NDEBUG`，这些代码将被忽略掉：
```CPP
void print(const int ia[],size t size);
#ifndef NDEBUG
    //__func__是编译器定义的一个局部静态变量、用于存放函数的名字
    cerr <<__func__ << ": array size is " << size << endl;
#endif
```
在这段代码中，使用变量`__func__`输出当前调试的函数的名字。编译器为每个函数都定义了`__func__`,它是`const char`的一个静态数组，用于存放函数的名字。
除了C++编译器定义的`__func__`之外，预处理器还定义了另外4个对于程序调试func很有用的名字:
```CPP
__FILE__
//存放文件名的字符串字面值
__LINE__
//存放当前行号的整型字面值
__TIME__
//存放文件编译时间的字符串字面值
__DATE__
//存放文件编译日期的字符串字面信
```
可以使用这些常量在错误信息中提供更多信息：
```CPP
if (word. size() < threshold)
    cerr << "Error: " << __FILE__ << ": in funetion" << __func__
         << "at line" << __LINE__ << endl
         << "       Compild" << __DATE__
         << "       Word read was \"" << word
         << "\":Length too short" << endl;
```
如果我们给程序提供了一个长度小于`threshold`的`string`对象，将得到下面的错误消息：
```CPP
Error:wdebug.cc : in function main at line 27
Compiled on Jul 11 2012 at 20:50:03
Word read was "foo": Length too short
```

## 函数匹配

在大多数情况下，我们容易确定某次调用应该选用哪个重载函数。然而，当几个重载函数的形参数量相等以及某些形参的类型可以由其他类型转换得来时，这项工作就不那么容易了。以下面这组函数及其调用为例：
```CPP
void f();
void f(int);
void f(int, int);
void f(double, double = 3.14);
f(3.1);//调用void f(double, double);
```
**确定候选函数和可行函数**

函数匹配的第一步是选定本次调用对应的重载函数集，集合中的函数称为候选函数`(candidate fanction)`。候选函数具备两个特征：一是与被调用的函数同名，二是其声明在调用点可见。在这个例子中，有4个名为f的候选函数。
第二步考察本次调用提供的实参，然后从候选函数中选出能被这组实参调用的函数这些新选出的函数称为可行函数`(viable function)`。可行函数也有两个特征：一是其形参数量与本次调用提供的实参数量相等，二是每个实参的类型与对应的形参类型相同，或者能转换成形参的类型。
我们能根据实参的数量从候选函数中排除掉两个。不使用形参的函数和使用两个int形参的函数显然都不适合本次调用，这是因为我们的调用只提供了一个实参，而它们分别有0个和2个形参。

使用一个int形参的函数和使用两个`double`形参的函数是可行的，它们都能用一个实参调用。其中最后那个函数本应该接受两个`double`值，但是因为它含有一个默认实参，所以只用一个实参也能调用它。

在使用实参数量初步判别了候选函数后，接下来考察实参的类型是否与形参匹配。和一般的函数调用类似，实参与形参匹配的含义可能是它们具有相同的类型，也可能是实参类型和形参类型满足转换规则。在上面的例子中，剩下的两个函数都是可行的，`f(int)`是可行的，因为实参类型`double`能转换成形参类型`int`。
`f(double, double)`是可行的，因为它的第二个形参提供了默认值，而第一个形参的类型正好是`double`，与函数使用的实参类型完全一致

**寻找最佳匹配(如果有)**

函数匹配的第三步是从可行函数中选择与本次调用最匹配的函数。在这一过程中，逐一检查函数调用提供的实参，寻找形参类型与实参类型最匹配的那个可行函数。最匹配的基本思想是：实参类型与形参类型越接近，它们匹配得越好在我们的例子中，调用只提供了一个(显式的)实参，它的类型是`double`。如果调用`f(int)`，实参将不得不从`double`转换成`int`。另一个可行函数`f(double, double)`
则与实参精确匹配。精确匹配比需要类型转换的匹配更好，因此，编译器把`f(3.1)`解析成对含有两个`double`形参的函数的调用，并使用默认值填补我们未提供的第二个实参

**含有多个形参的函数匹配**

当实参的数量有两个或更多时，函数匹配就比较复杂了。分析如下的调用会发生什么情况：
`f(12,5.2)`
选择可行函数的方法和只有一个实参时一样，编译器选择那些形参数量满足要求且实参类型和形参类型能够匹配的函数。此例中，可行函数包括`f(int, int)`和`f(double, double)`。
接下来，编译器依次检查每个实参以确定哪个函数是最佳匹配。如果有且只有一个函数满足下列条件，则匹配成功:

+ 该函数每个实参的匹配都不劣于其他可行函数需要的匹配

+ 至少有一个实参的匹配优于其他可行函数提供的匹配

如果在检查了所有实参之后没有任何一个函数脱颖而出，则该调用是错误的。编译器将报告二义性调用的信息。

在上面的调用中，只考虑第一个实参12时函数`f(int, int)`能精确匹配；要想匹配第二个函数，`int`类型的实参必须转换成`double`类型。显然需要内置类型转换的匹配劣于精确匹配，因此仅就第一个实参来说`f(int, int)`比`f(double, double)`更好
接着考虑第二个实参2.56，此时`f(double, double)`是精确匹配；要想调用`f(int, int)`必须将2.56从`double`类型转换成`int`类型。因此仅就第二个实参来说，`f(double, double)`更好

编译器最终将因为这个调用具有二义性而拒绝其请求：因为每个可行函数各自在一个实参上实现了更好的匹配，从整体上无法判断熟优孰劣。看起来我们似乎可以通过强制类型转换其中的一个实参来实现函数的匹配，但是在设计良好的系统中，不应该对实参进行强制类型转换

**调用重载函数时应尽量避免强制类型转换。如果在实际应用中确实需要强制类型转换。则说明我们设计的形参集合不合理**

**实参类型转换**

为了确定最佳匹配，**编译器将实参类型到形参类型的转换划分成几个等级**，具体排序如下所示：
1. 精确匹配，包括以下情况:
+ 实参类型和形参类型相同
+ 实参从数组类型或函数类型转换成对应的指针类型
+ 向实参添加顶层`const`或者从实参中删除顶层`const`
2. 通过`const`转换实现的匹配

3. 通过类型提升实现的匹配

4. 通过算术类型转换或指针转换实现的匹配

   **所有算术类型转换的级别都一样**

5. 通过类类型转换实现的匹配

```CPP
void manip(long);
void manip(float);
f manip(3.14);
//error，二义性调用
```
字面值3.14的类型是`double`，它既能转换成`long`也能转换成`float`。因为存在两种可能的算数类型转换，所以该调用具有二义性

**函数匹配和const实参**

如果重载函数的区别在于它们的引用类型的形参是否引用了`const`或者指针类型的形参是否指向`const`，则当调用发生时编译器通过实参是否是常量来决定选择哪个函数:
```CPP
Record lookup(Account&);
Record lookup(const Account&);
const Aecount a;
Account b;
lookup(a);//调用lookup(const Account&)
lookup(b);//调用lookup(Account&)
```
***
## 函数指针

函数指针指向的是函数而非对象。和其他指针一样，函数指针指向某种特定类型。函数的类型由它的返回类型和形参类型共同决定，与函数名无关。
```CPP
bool lenqthCompare (const string &,const string &);
//函数
bool (*pf) (const string &,const string &);
//pf指向一个函数，该函数接受两个const string &类型的参数并返回一个bool值
//(*pf)括号不能省略，如果省略括号，pf是一个返回值为bool指针的函数
```

当把函数名作为一个值使用时，该函数自动转换为指针
```CPP
pf = lengthCompare;
pf = &lengthCompare;//等价赋值语句，&是可选的
```
能直接使用指向函数的指针调用该函数，无须提前解引用指针
```CPP
bool bl = pf("hello", "goodbye");
bool b2 = (*pf) ("hello","goodbye");
bool b3 = lengthCompare("hello","goodbye");
//三条语句都是等价的
```

**重载函数的指针**
当我们使用重载函数时，上下文必须清晰地界定到底应该选用哪个函数。如果定义了指向重载函数的指针
```CPP
void ff(int*);
void ff(unsigned int);

void (*pfl) (unsiqned int) = ff;//pf1指向ff(unsigned int)

void (*pf2) (int) = ff;//error：没有任何一个ff与该形参列表匹配

double (*pf3) (int*) = ff;//error：ff和pf3的返回类型不匹配
```
***编译器通过指针类型决定选用哪个函数，指针类型必须与重载函数中的某一个精确匹配***

**函数指针形参**

和数组类似，虽然不能定义函数类型的形参，但是形参可以是指向函数的指针。此时，形参看起来是函数类型，实际上却是当成指针使用
```CPP
//第三个形参是函数类型，它会自动地转换成指向函数的指针
void useBigger (const string &s1, const string &s2,
                bool pf(const string &, const string &));

//等价的声明：显式地将形参定义成指向函数的指针
void useBigger (const string as1,const string &s2,
                bool (*pf) (const string &, const string &));

//我们可以直接把函数作为实参使用，此时它会自动转换成指针
useBigger (s1,s2,lengthCompare);
//自动将函数lengthCompare转换成指向该函数的指针
```

可以使用类型别名来简化使用函数指针的代码
```CPP
typedef bool Func(const string&, const string&);
typedef decltype (lengthCompare) Func2;
//等价函数类型

typedef bool(*FuncP) (const string&, const string&);
typedef decltype(lengthCompare) *FuncP2;

using FuncP3 = bool(*)(const string&, const string&);
using FuncP4 = decltype(lengthCompare)*;
//等价的指向函数的指针
```
需要注意的是，`decltype`返回函数类型，此时不会将函数类型自动转换成指针类型。因为`decltype`的结果是函数类型，所以只有在结果前面加上`*`才能得到指针

**返回指向函数的指针**

和数组类似，虽然不能返回一个函数，但是能返回指向函数类型的指针。但我们必须把返回类型写成指针形式，编译器不会自动地将函数返回类型当成对应的指针类型处理。要想声明一个返回函数指针的函数，最简单的办法是使用类型别名：
```CPP
using F = int(int*, int)	  //将F定义成函数类型
using PF = int(*) (int*, int);//将PF定义成指向函数类型的指针
```
必须时刻注意的是，和函数类型的形参不一样，返回类型不会自动地转换成指针
**必须显式地将返回类型指定为指针：**

```CPP
PF fl(int);//right PE是指向函数的指针
F fl(int);//error F是函数类型，不能返回一个函数
F *f1(int);//right 显式地指定返回类型是指向函数的指针
```
按照从内到外的顺序阅读声明
```CPP
int (*f1(int))(int*,int);
//直接声明
auto fi(int)->int(*)(int*,int);
//尾置声明
```

**将auto和decltype用于函数指针类型**

如果明确知道返回的函数是哪一个，就能使用`decltype`简化书写函数指针返回类型的过程
```CPP
string::size_type sumLength(const string&,const string&);
string::size_type largerLength(const string&, const string&);

decltype (sumLength) *getFen(const string&);
//接受一个string类型参数，返回一个指针，指针指向sumLength类型的函数
```
牢记当将`decltype`作用于某个函数时，它返回函数类型而不是指针类型，需要显式地加上`*`以表明我们需要返回指针而非函数本身

***

# 类

类的基本思想是数据抽象`(data abstraction)`和封装`(encapsulation)`。
数据抽象是种依赖于接口`(interface)`和实现`(implementation)`分离的编程(以及设计)技术。

+ <span style="color: white;">类的接口包括用户所能执行的操作</span>
+ <span style="color: orange;">类的实现则包括类的数据成员、负责接口实现的函数体以及定义类所需的各种私有函数</span>
+ 数据抽象(接口与实现分离)
+ 封装(将实现隐藏)
+ 封装实现了类的接口和实现的分离。封装后的类隐藏了它的实现细节，也就是说，类的用户只能使用接口而无法访问实现部分

类要想实现数据抽象和封装，需要首先定义一个抽象数据类型`(abstract data type)`
在抽象数据类型中，由类的设计者负责考虑类的实现过程，使用该类的程序员则只需要抽象地思考类型做了什么，而无须了解类型的工作细节

## 定义ADT

### 设计Sales_data类

我们的最终目的是令`Sales data`支持与`Sales item`类完全一样的操作集合
`Sales item`类有一个名为`isbn`的成员函数`(member function)`，并且支持`+、=、+=、<<和>>`运算符
执行加法和`IO`的函数不作为`Sales data`的成员，相反的，我们将其定义成普通函数；执行复合赋值运算的函数是成员函数。`Sales data`类无须专门定义赋值运算

综上所述，`Sales data`的接口应该包含以下操作：

+ 一个`isbn`成员函数，用于返回对象的ISBN编号

+ 一个`combine`成员函数，用于将一个`Sales data`对象加到另一个对象上

+ 一个`add`函数，执行两个`Sales data`对象的加法

+ 一个`read`函数，将数据从`istream`读入到`Sales data`对象中

+ 一个`print`函数，将`Sales data`对象的值输出到`ostream`

使用这些函数编写书店程序:
```CPP
Sales_data tota1;//保存当前求和结果变量
if (read(cin, total)) {
    Sales data trans;//保存下一条交易数据的变量
    while(read(cin, trans)) {//读入剩余交易
         if (total. isbn() == trans.isbn())
             total. combine(trans);//更新total当前值
        else{
            print (cout, tota1) << endl;
            total = trans;//处理下一本书
        }
    }
    print (cout, total) << end1;//输出最后一条交易
}
else
    cerr << "No data?!"<< end1;
```

### 定义Sales_data类

**所有成员都必须在类的内部声明，但是成员函数体可以定义在类内也可以定义有类外**。对于`Sales data`类来说，`isbn`函数定义在了类内，而`combine`和`avg price`定义在了类外
**定义在类内部的函数是隐式的`inline`函数**

```CPP
struct Sales_data {
    string isbn() const { return bookNo; }
    Sales_data& combine(const Sales_data&);
    double avg_price() const;
    
    string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
//Sales_data的非成员接口函数
Sales_data add(const Sales_data&, const Sales_data&);
ostream& print(ostream&, const Sales_data&);
istream& read(istream&, Sales_data&);
```

`total.isbn()`
在这里使用了点运算符来访问`total`对象的`isbn`成员，然后调用它
当我们调用成员函数时，实际上是在替某个对象调用它。如果`isbn`指向`Sales_data`的成员`(例如bookNo)`，则它隐式地指向调用该函数的对象的成员。在上面所示的调用中，当`isbn`返回`bookNo`时，实际上它隐式地返回`total.bookNo`

成员函数通过一个名为`this`的额外的隐式参数来访问调用它的那个对象。当我们调用一个成员函数时，用请求该函数的对象地址初始化`this`
例如，如果调用`total.isbn()`
则编译器负责把`total`的地址传递给`isbn`的隐式形参`this`，可以等价地认为编译器将该调用重写成了如下的形式

```CPP
//伪代码，用于说明调用成员函数的实际执行过程
Sales_data::isbn(&total)
```
其中，调用`Sales_data`的`isbn`成员时传入了`total`的地址

在成员函数内部可以直接使用调用该函数的对象的成员，而无须通过成员访问运算符
因为`this`所指的正是这个对象。任何对类成员的直接访间都被看作`this`的隐式引用，也就是说，当`isbn`使用`bookNo`时，它隐式地使用`this`指向的成员，就像书写了`this->bookNo`一样
对于我们来说，`this`形参是隐式定义的。但实际上，任何自定义名为`this`的参数或变量的行为都是非法的。我们可以在成员函数体内部使用`this`，因此尽管没有必要，但我们还是能把`isbn`定义成如下的形式

```CPP
string isbn() const{return this->bookNo};
//非法的，仅演示
```
因为 `this` 的目的总是指向“这个”对象，所以 `this`是一个常量指针`Sales_data *const`,不允许改变`this`中保存的地址

### const成员函数

`isbn`函数的另一个关键之处是紧随参数列表之后的`const`关键字，这里，`const`的作用是修改隐式`this`指针的类型

默认情况下，`this`的类型是指向类类型非常量版本的常量指针。例如在`Sales_data`成员函数中，`this`的类型是`Sales data*const`
尽管`this`是隐式的，但它仍然需要遵循初始化规则，意味着在默认情况下我们不能把`this`绑定到一个常量对象上，这一情况也就使得我们不能在一个常量对象上调用普通的成员函数

如果`isbn`是一个普通函数而且`this`是一个普通的指针参数，则应该把`this`声明成`const Sales_data*const`
在`isbn`的函数体内不会改变`this`所指的对象，所以把`this`设置为指向常量的指针有助于提高函数的灵活性

然而，`this`是隐式的并且不会出现在参数列表中，所以在哪儿将`this`声明成指向常量的指针就成为我们必须面对的问题。

**C++语言的做法是允许把`const`关键字放在成员函数的参数列表之后，此时，紧跟在参数列表后面的`const`表示`this`是一个指向常量的指针。像这样使用`const`的成员函数被称作常量成员函数`(const member function)`**

<span style="color: orange;">常量对象以及常量对象的引用和指针都只能调用常量成员函数</span>
<span style="color: white;">
类本身就是一个作用域,类的成员函数的定义嵌套在类的作用域之内
</span>
<span style="color: white;">
编译器分两步处理类：首先编译成员的声明，然后才轮到成员函数体(如果有的话)
因此，成员函数体可以随意使用类中的其他成员而无须在意这些成员出现的次序
</span>

### 在类的外部定义成员函数

像其他函数一样，当在类的外部定义成员函数时，成员函数的定义必须与它的声明匹配
也就是说，返回类型、参数列表和函数名都得与类内部的声明保持一致
**如果成员被声明成常量成员函数，那么它的定义也必须在参数列表后明确指定`const`属性。同时，类外部定义的成员的名字必须包含它所属的类名：**

```CPP
double Sales_data::avg_price() const {
    if (units_sold)
        return revenue / units_sold;
    else
        return 0;
}
```
函数名`Sales_data::avg_price`使用作用域运算符说明我们定义了一个名为`avg_price`的函数，并且这个函数声明在类`Sales_data`的作用域中

### 定义一个返回`this`对象的函数

函数`combine`的设计初衷类似于复合赋值运算符`+=`，调用该函数的对象代表的是赋值运算符左侧的运算对象，右侧运算对象则通过显式的实参被传入函数
```CPP
Sales_datas& combine(const Sales_data &rhs){
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;//返回调用该函数的对象
}
```
当调用`total.combine(trans)`时
`total`的地址被绑定到隐式的`this`参数上，而`rhs`绑定到了`trans`上
因此，当`combine`执行下面的语句时：

```CPP
    units_sold += rhs.units_sold;
```
效果等同于求`total.unit_sold`和`trans.unit_sold`的和，并把结果赋给`total.unit_sold`

该函数一个值得关注的部分是它的返回类型和返回语句
一般来说，当定义的函数类似于某个内置运算符时，应该令该函数的行为尽量模仿这个运算符。
内置的赋值运算符把它的左侧运算对象当成左值返回，因此为了与它保持一致，`combine`函数必须返回引用类型
因为此时的左侧运算对象是一个`Sales_data`的对象，所以返回类型应该是`Sales_data&`。如前所述，无须使用隐式的`this`指针访问函数调用者的某个具体成员，而是需要把调用函数的对象当成一个整体来访问:

```CPP
return *this;
```
`return`语句解引用指针以获得执行该函数的对象，换句话说，上面这个调用返回`total`的引用

### 定义类相关的非成员函数

类的作者常常需要定义一些辅助函数，比如`add、read和print`等。尽管这些函数定义的操作从概念上来说属于类的接口的组成部分，但它们实际上并不属于类本身，我们定义非成员函数的方式与定义其他函数一样，通常把函数的声明和定义分离开来

如果函数在概念上属于类但是不定义在类中，则它一般应与类声明，在同一个头文件内。在这种方式下，用户使用接口的任何部分都只需要引入一个文件

一般来说，如果非成员函数是类接口的组成部分，则这些函数的声明应该与类在同一个头文件内

**定义`read`和`print`函数**

```CPP
//输入的交易信息包括ISBN、售出总数和售出价格
istream& read(istream& is, Sales_data& item) {
    //IO对象不能拷贝，只能引用    
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price * item.units_sold;
    return is;
}
ostream& print(ostream& os, const Sales_data& item) {
    os << item.isbn() << " " << item.units_sold << " "
        << item.revenue << " " << item.avg_price();
    return os;
}
```
`read`函数从给定流中将数据读到给定的对象里，`print`函数则负责将给定对象的内容打印到给定的流中

除此之外，关于上面的函数还有两点是非常重要的
第一点，`read`和`print`分别接受一个各自IO类型的引用作为其参数，这是因为IO类属于不能被拷贝的类型，因此只能通过引用来传递它们
而且，因为读取和写入的操作会改变流的内容，所以两个函数接受的都是普通引用，而非对常量的引用
第二点，`print`函数不负责换行。一般来说，执行输出任务的函数应该尽量减少对格式的控制，这样可以确保由用户代码来决定是否换行

**定义add函数**

`add`函数接受两个`Sales_data`对象作为其参数，返回值是一个新的`Sales_data`用于表示前两个对象的和
```CPP
Sales_data add(const Sales_data &lhs, const Sales_data &rhs) {
    Sales_data sum = lhs;//把lhs的数据成员拷贝给sum
    sum.combine(rhs);    //把rhs的数据成员加到sum中
    return sum;
}
```
在函数体中定义了一个新的`Sales_data`对象并将其命名为`sum`
`sum`将用于存放两笔交易的和，我们用`ihs`的副本来初始化`sum`
默认情况下，拷贝类的对象其实拷贝的是对象的数据成员
在拷贝工作完成之后，`sum`的`bookNo、units_sold`和`revenue`将和`lhs`一致
接下来我们调用`combine`函数，将`rhs`的`units_sold`和`revenue`添加给`sum`
最后，函数返回`sum`的副本添加给`sum`

## 构造函数

每个类都分别定义了它的对象被初始化的方式，类通过一个或几个特殊的成员函数来控制其对象的初始化过程，这些函数叫做构造函数`(constructor)`
构造函数的任务是初始化类对象的数据成员，无论何时只要类的对象被创建，就会执行构造函数，在这一节中只介绍定义构造函数的基础知识

+ 构造函数的名字和类名相同。和其他函数不一样的是，构造函数没有返回类型
+ 除此之外类似于其他的函数，构造函数也有一个(可能为空的)参数列表和一个(可能为空)的函数体
+ 类可以包含多个构造函数，和其他重载函数一样，不同的构造函数之间必须在参数数量或参数类型上有所区别
+ 不同于其他成员函数，构造函数不能被声明成`const`
+ 当创建类的一个`const`对象时，直到构造函数完成初始化过程，对象才能真正取得其“常量”属性。因此，构造函数在`const`对象的构造过程中可以向其写值

### 合成的默认构造函数

`Sales_data`类并没有定义任何构造函数，但之前使用了`Sales_data`对象的程序仍然可以正确地编译和运行
因为如果类没有显式地定义构造函数，编译器会为类定义一个默认构造函数`(default constructor)`，默认构造函数无需任何实参

编译器创建的构造函数又被称为合成的默认构造函数`(sythesized default constructor)`
对于大多数类来说，这个合成的默认构造函数将按照如下规则初始化类的数据成员：

+ 如果存在类内的初始值，用它来初始化成员
+ 否则默认初始化该成员

因为`Sales_data`为`units_sold`和`revenue`提供了初始值，所以合成的默认构造函数将使用这些值来初始化对应的成员；同时，它把`bookNo`默认初始化成一个空字符串

**某些类不能依赖于合成的默认构造函数**

合成的默认构造函数只适合非常简单的类，比如现在定义的这个`Sales_data`版本
对于一个普通的类来说，必须定义它自己的默认构造函数
原因有三：
+ 第一个原因是编译器只有在发现类不包含任何构造函数的情况下才会替我们生成一个默认的构造函数。
一旦我们定义了一些其他的构造函数，那么除非我们再定义个默认的构造函数，否则类将没有默认构造函数。
这条规则的依据是，如果一个类在某种情况下需要控制对象初始化，那么该类很可能在所有情况下都需要控制。
***
+ 第二个原因是对于某些类来说，合成的默认构造函数可能执行错误的操作。
因为如果定义在块中的内置类型或复合类型比如数组和指针的对象被默认初始化，则它们的值将是未定义的。
该准则同样适用于默认初始化的内置类型成员。因此，含有内置类型或复合类型成员的类应该在类的内部初始化这些成员，或者定义一个自己的默认构造函数。否则，用户在创建类的对象时就可能得到未定义的值
***
+ 第三个原因是有的时候编译器不能为某些类合成默认的构造函数。
例如，如果类中包含一个其他类类型的成员且这个成员的类型没有默认构造函数，那么编译器将无法初始化该成员。
对于这样的类来说，我们必须自定义默认构造函数，否则该类将没有可用的默认构造函数。还有其他一些情况也会导致编译器无法生成一个正确的默认构造函数

### 定义构造函数

对于`Sales_data`类来说，我们将使用下面的参数定义4个不同的构造函数：
+ 一个`istreams&`，从中读取一条交易信息

+ 一个`conststring&`，表示ISBN编号；一个`unsigned`，表示售出的图书数量，一个`double`，表示图书的售出价格

+ 一个`conststring&`，表示ISBN编号；编译器将赋予其他成员默认值

+ 一个空参数列表即默认构造函数，正如刚刚介绍的，既然我们已经定义了其他构造函数，那么也必须定义一个默认构造函数

```CPP
struct Sales_data {
    //新增的构造函数
    Sales_data() = default;
    Sales_data(const string& s) : bookNo(s) { }
    Sales_data(const string& s, unsigned n, double p) : 
        bookNo(s), units_sold(n), revenue(p* n) { }
    Sales_data(istream&);

    string isbn() const { return bookNo; }
    Sales_data& combine(const Sales_data&);
    double avg_price() const;
    string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
```

```CPP
Sales_data() = default;
```
该构造函数不接受任何实参，所以它是一个默认构造函数
定义这个构造函数的目的仅仅是因为我们既需要其他形式的构造函数，也需要默认的构造函数，我们希望这个函数的作用完全等同于之前使用的合成默认构造函数

在C++11新标准中，如果需要默认的行为，那么可以通过在参数列表后面写上
`= defaalt`来要求编译器生成构造函数
`= default`既可以和声明一起出现在类的内部，也可以作为定义出现在类的外部。和其他函数一样，如果`= default`在类的内部，则默认构造函数是内联的：如果它在类的外部，则该成员默认情况下不是内联的

<span style="color: orange;">caution：</span>
上面的默认构造函数之所以对`Sales_data`有效，是因为为内置类型的数据成员提供了初始值。如果编译器不支持类内初始值，那么默认构造函数就应该使用构造函数初始值列表来初始化类的每个成员

**构造函数初始值列表**

```CPP
    Sales_data(const string& s) : bookNo(s) { }
    Sales_data(const string& s, unsigned n, double p) : 
        bookNo(s), units_sold(n), revenue(p * n) { }
```
这两个定义中出现了新的部分，即冒号以及冒号和花括号之间的代码
其中花括号定义了空的函数体。我们把新出现的部分称为构造函数初始值列表`(constructorinitializelist)`
它负责为新创建的对象的一个或几个数据成员赋初值
构造函数初始值是成员名字的一个列表，每个名字后面紧跟括号括起来的(或者在花括号内的)成员初始值。不同成员的初始化通过逗号分隔开来

含有三个参数的构造函数分别使用它的前两个参数初始化成员`bookNo`和`units_sold`
`revenue`的初始值则通过将售出图书总数和每本书单价相乘计算得到

只有一个`string`类型参数的构造函数使用这个`string`对象初始化`bookNo`
对于`units_sold`和`revenue`则没有显式地初始化
当某个数据成员被构造函数初始值列表忽略时，它将以与合成默认构造函数相同的方式隐式初始化

在此例中，这样的成员使用类内初始值初始化，因此只接受一个string参数的构造函数等价于：
```CPP
    Sales_data(const string& s, unsigned n, double p) : 
        bookNo(s), units_sold(0), revenue(0) { }
```
通常情况下，构造函数使用类内初始值不失为一种好的选择，因为只要这样的初始值存在我们就能确保为成员赋予了一个正确的值。不过，如果编译器不支持类内初始值，则所有构造函数都应该显式地初始化每个内置类型的成员

构造函数不应该轻易覆盖掉类内的初始值，除非新赋的值与原值不同。
如果你不能使用类内初始值，则所有构造函数都应该显式地初始化每个内置类型的成员

有一点需要注意，在上面的两个构造函数中函数体都是空的。这是因为这些构造函数的唯一目的就是为数据成员赋初值，一旦没有其他任务需要执行，函数体也就为空了

**在类的外部定义构造函数**

与其他几个构造函数不同，以`istream`为参数的构造函数需要执行一些实际的操作。
在它的函数体内，调用了`read`函数以给数据成员赋以初值：
```CPP
Sales_data::Sales_data(istream &is){
    read(is, *this);
    //read函数的作用是从is中读取一条交易信息然后存入this对象中
}
```
构造函数没有返回类型，所以上述定义从我们指定的函数名字开始。和其他成员函数一样，当我们在类的外部定义构造函数时，必须指明该构造函数是哪个类的成员。因此`Sales_data::Sales_data`的含义是定义`Sales_data`类的成员，它的名字是`Sales_data`，又因为该成员的名字和类名相同，所以它是一个构造函数

这个构造函数没有构造函数初始值列表，或者讲得更准确一点，它的构造函数初始值列表是空的。尽管构造函数初始值列表是空的，但是由于执行了构造函数体，所以对象的成员仍然能被初始化

没有出现在构造函数初始值列表中的成员将通过相应的类内初始值(如果存在的话)初始化或者执行默认初始化。对于`Sales_data`来说，这意味着一旦函数开始执行则`bookNo`将被初始化成空`string`对象，而`units_sold`和`revenue`将是0

为了更好地理解调用函数`read`的意义，要特别注意`read`的第二个参数是一个`Sales data`对象的引用。
使用`this`来把对象当成一个整体访问，而非直接访问对象的某个成员。因此在此例中，我们使用`*this`将“this”对象作为实参传递给`read`函数

***拷贝、赋值和析构***

除了定义类的对象如何初始化之外，类还需要控制拷贝、赋值和销毁对象时发生的行为
对象在几种情况下会被拷贝，如我们初始化变量以及以值的方式传递或返回一个对象等。
当我们使用了赋值运算符时会发生对象的赋值操作。
当对象不再存在时执行销毁的操作，比如一个局部对象会在创建它的块结束时被销毁，当`vector`对象(或者数组)销毁时存储在其中的对象也会被销毁。

如果不主动定义这些操作，则编译器将替我们合成它们。一般来说，编译器生成的版本将对对象的每个成员执行拷贝、赋值和销毁操作。例如在7.1.1节(第229页)的书店程序中，当编译器执行如下赋值语句时
```CPP
total = trans;
//处理下一本书的信息它的行为与下面的代码相同

//Sales_data的默认赋值操作等价于
total.bookNo = trans.bookNo;
total.units_sold = trans.units_sold;
total.revenue = trans.revenue;
```
**某些类不能依赖于合成的版本**
尽管编译器能替我们合成拷贝、赋值和销毁的操作，但是必须要清楚的一点是，对于某些类来说合成的版本无法正常工作。
特别是，当类需要分配类对象之外的资源时，合成的版本常常会失效。
第12章将介绍C++程序是如何分配和管理动态内存的而在13.1.4节(第447页)将会看到，管理动态内存的类通常不能依赖于上述操作的合成版本

不过值得注意的是，很多需要动态内存的类能且应该使用`vector`对象或者`string`对象管理必要的存储空间。使用`vector`或者`string`的类能避免分配和释放内存带来的复杂性

进一步讲，如果类包含`vector`或者`string`成员，则其拷贝、赋值和销毁的合成版本能够正常工作。
当我们对含有`vector`成员的对象执行拷贝或者赋值操作时，`vector`类会设法拷贝或者赋值成员中的元素。当这样的对象被销毁时，将销毁`vector`对象，也就是依次销毁`vector`中的每一个元素。这一点与`string`是非常类似的

###  访问控制与封装

在C++语言中，使用访问说明符`(access specifers)`加强类的封装性

+ 定义在`public`说明符之后的成员在整个程序内可被访问，`public`成员定义类的接口

+ 定义在`private`说明符之后的成员可以被类的成员函数访问，但是不能被使用该类的代码访问，`private`部分封装了(即隐藏了)类的实现细节

再一次定义`Sales_data`类，其新形式如下所示
```CPP
class Sales_data {
public:
    Sales_data() = default;
    Sales_data(const string& s, unsigned n, double p) :
              bookNo(s), units_sold(n), revenue(p * n) { }
    Sales_data(const string& s) : bookNo(s) { }
    Sales_data(istream&);
    string isbn() const { return bookNo; }
    Sales_data scombine(const Sales_data&);
private:
    double avg_price() const
        { return units_sold ? revenue / units_sold : 0; }
    string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
```

作为接口的一部分，构造函数和部分成员函数(即isbn和combine)紧跟在 `public`说明符之后
而数据成员和作为实现部分的函数则跟在`private`说明符后面
一个类可以包含0个或多个访问说明符，对于某个访问说明符能出现多少次没有严格限定。每个访问说明符指定了接下来的成员的访问级别，其有效范围直到出现下以个访问说明符或者到达类的结尾处为止

**使用`class`或`struct`关键字**
使用`class`关键字而非`struct`开始类的定义。这种变化仅仅是形式上有所不同，可以使用这两个关键字中的任何一个定义类。

唯一的一点区别是，`struct`和`class`的默认访问权限不太一样
类可以在它的第一个访问说明符之前定义成员，对这种成员的访问权限依赖于类定义的方式。
如果我们使用`struct`关键字，则定义在第一个访问说明符之前的成员是`public`的；相反，如果我们使用`class`关键字，则这些成员是`private`的。

**出于统一编程风格的考虑，当我们希望定义的类的所有成员是`public`的时，使用`struct`；反之，如果希望成员是`private`的，使用`class`**

### 构造函数初始值列表

如果成员是`const`、引用，或者属于某种未提供默认构造函数的类类型，则必须通过构造函数初始值列表为这些成员提供初始值

<span style="color: orange;">advice：使用构造函数初始值</span>
在很多类中，初始化和赋值的区别事关底层效率问题：前者直接初始化数据成员，后者则先初始化再赋值
除了效率问题外更重要的是，一些数据成员必须被初始化。养成使用构造函数初始值的习惯，这样能避免某些意想不到的编译错误，特别是遇到有的类含有需要构造函数初始值的成员时

**成员的初始化顺序与它们在类定义中的出现顺序一致：**第一个成员先被初始化，然后第二个，以此类推。构造函数初始值列表中初始值的前后位置关系不会影响实际的初始化顺序

但如果一个成员是用另一个成员来初始化，两个成员的初始化顺序很关键



```CPP
class X{
    int i;
    int j;
public:
    X(int val) : j(val),i(j) { }
};
//未定义的，i在j之前被初始化
//最好令函数初始值顺序与成员声明顺序保持一致，尽量避免使用某些成员初始化其他成员
```

**默认实参和构造函数**

```CPP
 class Sales_data {
public:
    Sales_data() = default;
//定义默认构造函数，令其与只接受一个string实参的构造函数相同
    Sale_data(std::string s = " ") : bookNo(s) { }
    Sales_data(const string& s) : bookNo(s) { }
     
    Sales_data(std::string s, unsigned n, double p) : 
        bookNo(s), units_sold(n), revenue(p* n) { }
    Sales_data(const string& s, unsigned n, double p) : 
        bookNo(s), units_sold(n), revenue(p* n) { }
    
    Sales_data(std::istream &is) { read(is,*this);}

    string isbn() const { return bookNo; }
    Sales_data& combine(const Sales_data&);
    double avg_price() const;
    string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
 }
```

因为我们不提供实参也能调用上述的构造函数，所以该构造函数实际上为我们的类提供了默认构造函数

如果一个构造函数为所有参数都提供了默认实参。则它实际上也定义了默认构造函数

### 委托构造函数

C++11新标准扩展了构造函数初始值的功能，使得可以定义所谓的**委托构造函数**

一个委托构造函数使用它所属类的其他构造函数执行它自己的初始化过程，或者说它把它自己的一些(或者全部)职责委托给了其他构造函数
和其他构造函数一样，一个委托构造函数也有一个成员初始值的列表和一个函数体，在委托构造函数内，成员初始值列表只有一个唯一的入口，就是类名本身
和其他成员初始值一样，类名后面紧跟圆括号括起来的参数列表，参数列表必须与类中另外一个构造函数匹配

```CPP
class MyClass {  
public:  
    MyClass() : MyClass(42) { // 委托给另一个构造函数  
        // 其他初始化操作  
    }  
      
    MyClass(int value) : m_value(value) { // 被委托的构造函数  
        // 成员变量初始化  
    } 
}
```

委托构造函数在某些情况下非常有用：

1. 当一个构造函数需要执行一些通用的初始化操作，而这些操作与另一个构造函数的参数无关时。通过委托，可以将这些通用的初始化操作集中到一个构造函数中，而不是分散到多个构造函数中。

2. 当一个构造函数需要依赖于另一个构造函数的初始化结果时。通过委托，可以确保在执行其他初始化操作之前，所需的成员变量已经被正确地初始化。

3. 当一个构造函数的参数与另一个构造函数的参数相同，但具有不同的默认值时。通过委托，可以避免重复编写相同的初始化代码。

+ 委托构造函数必须出现在被委托构造函数的参数列表中，并且必须是该构造函数的第一个成员初始化器
+ 当一个构造函数委托给另一个构造函数时，受委托的构造函数的初始值列表和函数体被依次执行

### 默认构造函数的作用

当对象被默认初始化或值初始化时自动执行默认构造函数。
**默认初始化在以下情况下发生：**

1. 当我们在块作用域内不使用任何初始值定义一个非静态变量或者数组时
2. 当一个类本身含有类类型的成员且使用合成的默认构造函数时
3. 当类类型的成员没有在构造函数初始值列表中显式地初始化时

+ 对于内置类型，默认初始化不会进行任何初始化操作

+ 对于类类型，默认初始化将调用该类型的默认构造函数。如果类型没有默认构造函数，则编译器将报错

**值初始化在以下情况发生：**

1. 在数组初始化的过程中如果我们提供的初始值数量少于数组的大小时

2. 当我们不使用初始值定义一个局部静态变量时

3. 当我们通过书写形如`T()`的表达式显式地请求值初始化时，其中T是类型名

+ 对于内置类型，值初始化将使用零值初始化
+ 对于类类型，值初始化将调用该类型的默认构造函数。如果类型没有默认构造函数，则编译器将报错

需要注意的是，对于类类型，如果没有显式地定义构造函数，编译器将自动生成一个默认构造函数。默认构造函数通常会进行一些基本的初始化操作，例如设置成员变量为默认值或执行其他必要的设置。如果没有定义任何构造函数，则类类型只能使用默认初始化或值初始化

****

### 隐式的类类型转换

**如果构造函数只接受一个实参，则它实际上定义了转换为此类类型的隐式转换机制**，有时我们把这种构造函数称作转换构造函数

***能通过一个实参调用的构造函数定义了一条从构造函数的参数类型向类类型隐式转换的规则***

```CPP
string null_book ="9-999-99999-9"；
//构造一个临时的Sales_data对象
//该对象的units sold和revenue等于0，bookNo等于null_book
```

在`Sales_data`类中，接受`string`和`istream`的构造函数分别定义了从这两种类型向`Sales_data`隐式转换的规则

+ **编译器只会自动地执行一步类型转换**

```CPP
//error：需要用户定义的两种转换
//(1)把“9-999-99999-9”转换成string
//(2)再把这个(临时的)string转换成Sales data
item.combine("9-999-99999-9");
    
//正确：显式地转换成string，隐式地转换成Sales_data
item.combine(string("9-999-99999-9"));
//正确：隐式地转换成string，显式地转换成Sales_data
item.combine(Sales_data("9-999-99999-9"));
```

+ **可以通过将构造函数声明为`explicit`来阻止隐式转换**
+ 关键字`explicit`只对一个实参的构造函数有效，因为需要多个实参的构造函数不能执行隐式转换
+ 只能在类内声明构造函数时使用`explicit`关键字，在类外部定义时不能重复
+ `explicit`构造函数只能用于直接初始化
```CPP
//正确：直接初始化
Sales_data item1(null_book);
//错误：不能将explicit构造函数用于拷贝形式的初始化过程
Sales_data item2 = null_book;
```
+ 编译器不会在自动转换过程中使用`explicit`构造函数

+ 为转换显式地使用构造函数
  尽管编译器不会将`explicit`构造函数用于隐式转换过程，但是我们可以使用这样的构造函数显式地强制进行转换：
```CPP
  //正确：实参是一个显式构造的Sales_data对象
  item.combine(Sales_data(nu11 book))；
  //正确：static_cast可以使用explicit的构造函数
  item.combine(static_cast<Sales_data>(cin))；
```

  

### 聚合类

聚合类使得用户可以直接访问其成员，并且具有特殊的初始化语法形式
当一个类满足如下条件时，我们说它是聚合的：

+ 所有成员都是`public`
+ 没有定义任何构造函数
+ 没有类内初始值。
+ 没有基类，也没有`virtual`函数

```CPP
//this is aggregate class
struct Data {
    int ival;
    string s;
};
//可以提供一个成员初始值列表，并用它初始化聚合类的数据成员
//val1.ival = 0; val1.s = string("Anna")
Data val1 = { 0, "Anna" };
```

+ 初始值顺序必须与声明顺序一致
+ 如果初始值列表中元素个数少于类的成员数量，则靠后的成员被值初始化

显式地初始化类的对象的成员存在三个明显的缺点：

1. 要求类的所有成员都是`public`的

2. 将正确初始化每个对象的每个成员的重任交给了类的用户而非类的作者
  因为用户很容易忘掉某个初始值，或者提供一个不恰当的初始值，所以这样的初始化过程冗长乏味且容易出错

3. 添加或删除一个成员之后，所有的初始化语句都需要更新

***

### 字面值常量类

`constexpr`函数的参数和返回值必须是字面值类型。除了算术类型、引用和指针外，某些类也是字面值类型。和其他类不同，字面值类型的类可能含有`constexpr`函数成员。这样的成员必须符合`constexpr`函数的所有要求，它们是隐式`const`的
**数据成员都是字面值类型的聚合类是字面值常量类**。如果一个类不是聚合类，但它符合下述要求，则它也是一个字面值常量类：

+ 数据成员都必须是字面值类型
+ 类必须至少含有一个`constexpr`构造函数如果一个数据成员含有类内初始值，则内置类型成员的初始值必须是一条常量表达式；或者如果成员属于某种类类型，则初始值必须使用成员自己的`constexpr`构造函数。
+ 类必须使用析构函数的默认定义，该成员负责销毁类的对象

**constexpr构造函数**

构造函数不能是`const`的，但字面值常量类的构造函数可以是`constexpr`函数，一个字面值常量类必须至少提供一个`constexpr`构造函数

`constexpr`构造函数可以声明成`=default`的形式或者是删除函数的形式
否则，`constexpr`构造函数就必须既符合构造函数的要求(意味着不能包含返回语句)，又符合`constexpr`函数的要求(意味着它能拥有的唯一可执行语句就是返回语句)
`constexpr`构造函数体一般来说应该是空的。我们通过前置关键字`constexpr`就可以声明一个`constexpr`构造函数：

```CPP
class Debug {
public:
    constexpr Debug(bool b = true) : hw(b),io(b),other(b) { }
    constexpr Debug(bool h,bool i,bool o) : hw(h),io(i),other(o) { }
    constexpr bool any() { return hw || io || other; }
    void set_io(bool b) { io = b; }
    void set hw(bool b){hw = b；}
    void set_other(bool b) { hw = b; }
private:
    //硬件错误
    bool hw;
    //IO错误
    bool io;
    //其他错误
    bool other;   
//constexpr函数是隐式的const，将不能更改数据成员    
```

`constexpr`构造函数必须初始化所有数据成员，初始值使用`constexpr`构造函数或者是一条常量表达式`constexpr`构造函数用于生成`constexpr`对象以及`constexpr`函数的参数或返回类型：

```CPP
constexpr Debug io_sub(false, true, false);//调试IO
if (io_sub.any())//等价于if(true)
    cerr << "print appropriate error messages"<<endl;
constexpr Debug prod(fa1se);//无调试
if (prod.any())//等价于if(false)
    cerr << "print an error message"<< endl;
```



## 友元

类可以允许其他类或者函数访问它的非公有成员
方法是今其他类或者函数成为它的友元
如果类想把一个函数作为它的友元，只需要增加一条以`friend`关键字开始的函数声明语句即可

```CPP
class Sales_data {
    //为Sales_data的非成员函数做友元声明
    friend Sales_data add(const Sales_data&, const Sales_data&);
    friend istream& read(istream&, Sales_data&);
    friend ostream& print(ostream&, const Sales_data&);
    //其他成员及访问说明符与之前一致
public:
    Sales_data() = default;
    Sales_data(const string& s, unsigned n, double p) :
        bookNo(s), units_sold(n), revenue(p* n) { }
    Sales_data(const string& s) : bookNo(s) { }
    Sales_data(istream&);
    string isbn() const { return bookNo; }
    Sales_data scombine(const Sales_data&);
private:
    double avg_price() const
    {
        return units_sold ? revenue / units_sold : 0;
    }
    string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;

};
    //Sales_data接口的非成员组成部分的声明
Sales_data add(const Sales_data&, const Sales_data&);
istream &read(istream&, Sales_data&);
ostream &print(ostream&, const Sales_data&);
```

<span style="color: orange;">封装的益处</span>

+ 确保用户代码不会无意间破坏封装对象的状态
+ 被封装的类的具体实现细节可以随时改变，而无需调整用户级别的代码

一旦把数据成员定义成`private`的，类的作者就可以比较自由地修改数据了。当实现部分改变时。只需要检查类的代码本身以确认这次改变有什么影响,换句话说只要类的接口不变，用户代码就无须改变。

如果数据是`public`的，则所有使用了原来数据成员的代码都可能失效。这时必须定位并重写所有依赖于老版本实现的代码之后才能重新使用该程序

把数据成员的访问权限设成`private`还有另外一个好处，这么做能防止由于用户的原因造成数据被破坏。
如果发现有程序缺陷破坏了对象的状态，则可以在有限的节围内定位缺陷：因为只有实现部分的代码可能产生这样的错误。因此。将查错限制在有限范围内将能极地降低维护代码及修正程序错误的难度

当类的定义发生改变时无须更改用户代码，但使用了该类的源文件必须重新编译

**友元的声明**

友元的声明仅仅指定了访问的权限，而非一个通常意义上的函数声明

如果希望类的用户能够调用某个友元函数，那么就必须在友元声明之外再专门对函数进行一次声明
为了使友元对类的用户可见，通常把友元的声明与类本身放置在同一个头文件中(类的外部)
因此`Sales_data`头文件应该为`read`、`print`和`add`提供独立的函数声明

+ 友元能定义在函数的内部，这样的函数是隐式内联的
+ 类可以把其他类或其他类的成员函数定义成友元
+ 友元关系不存在传递性，一个函数的友元的友元没有访问该函数的特权
+ 友元声明的作用是影响访问权限，它本身并非普通意义上的声明



## 类的其他特性

### 定义一个类型成员Screen

`Screen`表示显示器中的一个窗口。每个`Screen`包含一个用于保存`Screen`内容的`string`成员和三个`string::size_type`类型的成员，它们分别表示光标的位置以及屏幕的高和宽

除了定义数据和函数成员之外，类还可以自定义某种类型在类中的别名。**由类定义的类型名字和其他成员一样存在访问限制，可以是`public`或者`private`中的一种：**

```CPP
class Screen {
public:
    typedef string::size_type pos;
    //using pos = string::size_type;
private:
    pos cursor = 0;
    pos height = 0, width = 0;
    string contents;
};
```
在`Screen`的`public`部分定义了`pos`，这样用户就可以使用这个名字
`Screen`用户不应该知道`Sereen`使用了一个`string`对象来存放它的数据
因此通过把`pos`定义成`public`成员可以隐藏`Screen`实现的细节

用来定义类型的成员必须先定义后使用，因此类型成员通常出现在类开始的地方

### Screen类的成员函数

添加一个构造函数令用户能够自定义屏幕尺寸和内容，以及其他两个成员，分别负责移动光标和读取给定位置的字符
```CPP
class Screen {
public:
    typedef string::size_type pos;
    Screen() = default;//因为Sreen函数有另一个构造函数，所以此函数是必需的
    //cursor被其类内初始值初始化为0
    Screen(pos ht, pos wd, char c) : height(ht), width(wd),
        contents(ht* wd, c) { }

    char get() const                      //读取光标处的字符
    {
        return contents[cursor];          //隐式内联
    }
    inline char get(pos ht, pos wd) const;//显式内联
    Screen& move(pos r, pos c);           //能在之后被设为内联

private:
    pos cursor = 0;
    pos height = 0, width = 0;
    string contents;
};
```
因为我们已经提供了一个构造函数，所以编译器将不会自动生成默认的构造函数
如果我们的类需要默认构造函数，必须显式地把它声明出来。在此例中，我们使用`default`告诉编译器为我们合成默认的构造函数

第二个构造函数(接受三个参数)为`cursor`成员隐式地使用了类内初始值
如果类中不存在`cursor`的类内初始值，我们就需要像其他成员一样显式地初始化`cursor`

### 令成员作为内联函数
在类中，常有一些规模较小的函数适合于被声明成内联函数
定义在类内部的成员函数是自动`inline`的
因此，`Screen`的构造函数和返回光标所指字符的`get`函数默认是`inline`函数

可以在类的内部把`inline`作为声明的一部分显式地声明成员函数，同样的，也能在类的外部用`inline`关键字修饰函数的定义：
```CPP
inline Screen& Screen::move(pos r, pos c) {
    pos row = r * width;//计算行的位置
    cursor = row + c;   //在行内将光标移动到指定的列
    return *this;       //以左值的形式返回对象
}
char Screen::get(pos r, pos c) const {
    pos row = r * width;     //计算行的位置
    return contents[row + c];//返回给定列的字符
}
```
虽然无须在声明和定义的地方同时说明`inline`，但这么做是合法的。不过，最好只在类外部定义的地方说明`inline`，这样可以使类更容易理解

`inline`成员函数也应该与相应的类定义在同一个头文件中

**重载成员函数**

非成员函数和成员函数都可以被重载，只要函数之间在参数的数量或类型上有所区别即可
成员函数的函数匹配过程与非成员函数的匹配过程非常类似

### 可变数据成员

有时会发生这样一种情况，我们希望能修改类的某个数据成员，即使是在一个`const`成员函数内。可以通过在变量的声明中加入`mutable`关键字做到这点
一个可变数据成员永远不会是`const`，即使它是`const`对象的成员
因此，一个`const`成员函数可以改变一个可变成员的值。
举个例子，我们将给`Screen`添加一个名为`access_ctr`的可变成员，通过它我们可以追踪每个`Screen`的成员函数被调用了多少次

```CPP
class Screen {
public:
    void some_member() const;
private:
    mutable size_t access_ctr;
    //其他成员和之前版本一致
};
void Screen::some_member() const {
    ++access_ctr;
    //该成员的其他行为
}
```
虽然`some_member`是`const`成员函数，但它依然能够改变`access_ctr`的值，可变成员可以被任何成员函数包括`const`函数在内都能改变它的值

### 类数据成员的初始值

继续定义一个窗口管理类并用它表示显示器上的这个类将包含一组`Screen`
这个类将包含一个`Screen`类型的`vector`，每个元素表示一个特定的`Screen`
默认情况下，希望`window_mgr`类开始时总是拥有一个默认初始化的`Screen`
在C++11标准中最好的方式是把这个默认值声明为一个类内初始值：

```CPP
class Window_mgr {
private:
    //这个Window_mgr追踪的Screen
    //默认情况Window_mgr包含一个空白的Screen
    vector<Screen> screens{ Screen(24,80,' ') };
};
```

当初始化类类型的成员时，需要为构造函数传递一个符合成员类型的实参
在此例中我们使用一个单独的元素值对`vector`成员执行了列表初始化

这个`Screen`的值被传递给`vector<Screen>`的构造函数，从而创建了一个单元素的`vector`对象
这个`Screen`的构造函数接受两个尺寸参数和一个字符值，创建了一个给定大小的空白屏幕对象

类内初始值必须使用`=`的初始化形式(初始化`Screen`的数据成员时所用的)或者花括号括起来的直接初始化形式(初始化`screens`所用的)
**当提供一个类内初始值时，必须以符号`=`或者花括号表示**

**返回*this的成员函数**

继续添加一些函数，负责光标所在位置的字符或其他任一给定位置的字符：
```CPP
class Screen {
public:
    Screen& set(char);
    Screen& set(pos, pos, char);
    //其他成员和之前版本一致
};
inline Screen& Screen::set(char c) {
    contents[cursor] = c;//设置当前光标所在位置的新值
    return *this;        //将this作为左值对象返回
}
inline Screen& Screen::set(pos r, pos col, char ch) {
    contents[r * width + col] = ch;//设置给定位置的新值
    return *this;                  //将this作为左值对象返回
}
```

**返回引用的函数是左值的**，这些函数返回的是对象本身而不是副本

**一个const成员函数如果以引用的形式返回`*this`，那么它的返回类型将是常量引用**

### 基于const的重载

通过区分成员函数是否是`const`的，我们可以对其进行重载，其原因与指针参数是否指向`const`而重载函数的原因差不多

非常量版本的函数对于常量对象是不可用的，所以只能在一个常量对象上调用`const`成员函数，另一方面，虽然可以在非常量对象上调用常量版本或非常量版本，但显然非常量版本是一个更好的匹配

在下面的这个例子中，定义一个名为`do_display`的私有成员，由它负责打印`Screen`的实际工作。所有的`display`操作都将调用这个函数，然后返回执行操作的对象：

```CPP
class Screen {
public:
    //根据对象是否是const重载display函数
    Screen& display(ostream& os)
    {
        do_display(os); return *this;
    }
    const Screen& display(ostream& os) const
    {
        do_display(os); return *this;
    }
private:
    //该函数负责显示Screen的内容
    void do_display(ostream& os) const { os << contents; }
    //其他函数与之前版本一致
};
```

和之前所学的一样，**当一个成员调用另外一个成员时，`this`指针在其中隐式地传递**
因此，当`display`调用`do_display`时，它的`this`指针隐式地传递给`do_display`
而当`display`的非常量版本调用`do_disp1ay`时，它的`this`指针将隐式地从指向非常量的指针转换成指向常量的指针

当`do_dispay`完成后，`display`函数各自返回解引用`this`所得的对象。在非常量版本中，`this`指向一个非常量对象，因此`display`返回一个非`const`引用，而`const`成员则返回一个常量引用

**即使两个类的成员列表完全一致，它们也是不同的类型。对于一个类来说，它的成员和其他任何类(或者任何其他作用域)的成员都不是一样的**

```CPP
Sales_data item;
class Sales_data item;
//可以将类名作为类型名使用，类名也可以写在关键字class或struct后
```

## Sreen代码

```CPP
#ifndef SCREEN_H_
#define SCREEN_H_

#include <string>
#include <vector>

class Screen;

//Window_mgr类
class Window_mgr
{
public:
    using ScreenIndex = std::vector<Screen>::size_type;
    //窗口中每个屏幕的编号
    void clear(ScreenIndex);
    //按照编号将指定的Sreen重置为空白
private:
    std::vector<Screen> screens;
};

//Sreen类
class Screen {
    friend void Window_mgr::clear(ScreenIndex);
public:
    using pos = std::string::size_type;

    Screen() = default;
    //因为Screen有另一个构造函数，所以该函数是必需的
    Screen(pos ht, pos wd) :height(ht), width(wd) { }
    Screen(pos ht, pos wd, char c) :height(ht), width(wd), contents(ht* wd, c) { }

    char get() const { return contents[cursor]; }
    //读取光标处字符
    inline char get(pos r, pos c) const { return contents[r * width + c]; }
    //显式内联，返回给定列字符
    Screen& move(pos r, pos c);
    Screen& set(char);
    Screen& set(pos, pos, char);

    Screen& display(std::ostream& os) { do_display(os); return *this; }
    const Screen& display(std::ostream& os) const { do_display(os); return *this; }
    //负责显示Sreen内容，有非const和const两个版本

    void some_member() const { ++access_ctr; }
    //mutable可变数据成员，保存一个计数值，用于记录成员函数被调用次数

    pos size() const;

private:
    pos cursor = 0;
    pos height = 0, width = 0;
    std::string contents;
    void do_display(std::ostream& os) const { os << contents; }
    mutable size_t access_ctr;
};

inline Screen& Screen::move(pos r, pos c)
{
    pos row = r * width;//计算行位置
    cursor = row + c;	//在行内将光标移至指定列
    return *this;		//以左值形式返回对象
}

inline Screen& Screen::set(char c)
{
    contents[cursor] = c;//设置光标所在位置的新值
    return *this;
}

inline Screen& Screen::set(pos r, pos col, char c)
{
    contents[r * width + col] = c;//设置给定位置的新值
    return *this;
}

Screen::pos Screen::size() const
{
    return height * width;
}

void Window_mgr::clear(ScreenIndex i)
{
    Screen& s = screens[i];
    //s指向想清空的那个屏幕
    s.contents = std::string(s.height * s.width, ' ');
    //将选定的Screen重置为空白
}

#endif

int main()
{
	Screen myScreen(5, 5, 'X');

	myScreen.move(4, 0).set('#').display(cout);
	cout << "\n";
	myScreen.set(0, 2, '&').display(cout);
	cout << "\n";
	myScreen.display(std::cout);
	cout << "\n";
	char c=myScreen.get();
	cout << c;
	

	return 0;
}
```

**令成员函数作为友元时，要注意顺序，该成员函数的声明要在类之前定义，但不能定义，要在类之后定义**

## 类的作用域

**一个类就是一个作用域**

每个类都会定义它自己的作用域。在类的作用域之外，普通的数据和函数成员只能由对象、引用或者指针使用成员访间运算符来访问。对于类类型成员则使用作用域运算符访问。不论哪种情况，跟在运算符之后的名字都必须是对应类的成员

名字查找`name lookup` 寻找与所用名字最匹配声明的过程：

+ 首先，在名字所在的块中寻找其声明语句，只考虑在名字的使用之前出现的声明
+ 如果没找到，继续查找外层作用域
+ 如果最终没有找到匹配的声明，则程序报错

对于定义在类内部的成员函数来说，解析其中名字的方式与上述的查找规则有所区别，类的定义分两步处理：

+ 首先，编译成员的声明
+ 直到类全部可见后才编译函数体

这种两阶段的处理方式只适用于成员函数中使用的名字，***声明中使用的名字，包括返回类型或者参数列表中使用的名字，都必须在使用前确保可见。如果某个成员的声明使用了类中尚未出现的名字，则编译器将会在定义该类的作用域中继续查找***

+ 内层作用域会重新定义外层作用域中的名字，即使该名字已在作用域中使用过
+ 在类中，如果成员使用了外层作用域中的某个名字且该名字代表一种类型，则类不能在之后重新定义该名字，即使定义类型相同

**成员函数中使用的名字以函数使用之前出现的声明-->类内部所有成员-->成员函数定义之前的作用域的次序查找**

成员函数作用域-->类作用域-->全局作用域

```CPP
int height;
class Screen {
public: 
    typedef std::string::size type pos; 
    void dummy_fcn(pos height) {
        cursor = width * height; }
private: 
    Pos cursor = 0; 
    pos height = 0,width = 0;
}; 
//首先查找函数内部的height，不会使用外部的height
//可以通过加上类名或显式地使用this指针来强制访问对象
*this->height;//成员height
*Screen::height;//成员height
*height;//全局height
//成员函数中的名字不应该隐藏同名成员，最好的方法是给参数起其他名字
```

## 类的静态成员

静态成员可以是`public`也可以是`private`，静态数据成员的类型可以是常量、引用、指针、类类型等

```CPP
class Account {
public:
	void calculate() { amount += amount * interestRate; }
	static double rate() { return interestRate; }
	static void rate(double);
private:
	std::string owner;
	double amount;
	static double interestRate;
	static double initRate();
};
//定义一个类来表示银行账户记录
```

类的静态成员存在于任何对象之外，对象中不包含任何与静态数据成员有关的数据。因此每个`Account`对象将包含两个数据成员：`owner`和`amount`，只存在一个`interestRate`对象而且它被所有`Account`对象共享
类似的，静态成员函数也不与任何对象绑定在一起，它们不包含`this`指针。作为结果，静态成员函数不能声明成`const`的，而且我们也不能在`static`函数体内使用`this`指针。这一限制既适用于`this`的显式使用，也适用于对调用非静态成员的隐式使用

```CPP
double r;
r = Account::rate();
//使用作用域运算符访问静态成员
```

虽然静态成员不属于类的某个对象，但是我们仍然可以使用类的对象、引用或者指针来访问静态成员：

```CPP
Account ac1;
Account *ac2 = &ac1;
//调用静态成员函数rate的等价形式
//通过Account的对象或引用
r = ac1.rate();
//通过指向Account对象的指针
r = ac2->rate();
```

成员函数不用通过作用域运算符就能直接使用静态成员：

```CPP
class Account {
public:
	void calculate() { amount += amount * interestRate; }
private:
    static double interestRate;
    //其他成员与之前版本一致
};
```

当在类的外部定义静态成员时，不能重复`static`关键字，该关键字只出现在类内部的声明语句，且指向类外部的静态成员时，必须指明成员所属的类名：

```CPP
void Aecount::rate(double newRate) {
    interestRate = newRate;
}
```

因为静态数据成员不属于类的任何一个对象，所以它们并不是在创建类的对象时被定义的。这意味着它们不是由类的构造函数初始化的。**不能在类的内部初始化静态成员，静态常量整型成员和静态成员函数可以例外**；相反的，必须在类的外部定义和初始化每个静态成员，一个静态数据成员只能定义一次
静态数据成员定义在任何函数之外。因此一旦它被定义，就将一直存在于程序的整个生命周期中

通常情况下，类的静态成员不应该在类的内部初始化。然而可以为静态成员提供`const`整数类型的类内初始值，不过要求静态成员必须是字面值常量类型的`constexpr`，初始值必须是常量表达式，因为这些成员本身就是常量表达式，所以它们能用在所有适合于常量表达式的地方。例如可以用一个初始化了的静态数据成员指定数组成员的维度：
```CPP
class Account {
public:
	static double rate() { return interestRate; }
	static void rate(double);
private:
    static constexpr int period = 30;
    double daily_tbl[period];
```

如果某个静态成员的应用场景仅限于编译器可以替换它的值的情况，则一个初始化的`const`或`constexpr static`不需要分别定义。相反，如果将它用于值不能替换的场景中，则该成员必须有一条定义语句

如果`period`的唯一用途就是定义`daily_tb1`的维度，则不需要在`Account`外面专门定义`period`。此时，如果忽略了这条定义，那么对程序非常微小的改动也可能造成编译错误，因为程序找不到该成员的定义语句。举个例子，当需要把`Aecount::period`传递给一个接受`const int&`的函数时，必须定义`period`如果在类的内部提供了一个初始值，则成员的定义不能再指定一个初始值了

**即使一个常量静态数据成员在类内部被初始化了，通常情况下也应该在类的外部定义一下该成员**

静态成员独立于任何对象。因此，在某些非静态数据成员可能非法的场合，静态成员却可以正常地使用，静态数据成员可以是不完全类型，静态数据成员的类型可以就是它所属的类类型。而非静态数据成员则受到限制，只能声明成它所属类的指针或引用：

```CPP
class Bar {
public: 
    //略
private: 
    static Bar mem1;//correct
    Bar *mem2;//correct 指针和引用成员可以是不完整类型
    Bar mem3;//error
};
```

可以使用静态成员作为默认实参，非静态数据成员不能作为默认实参，因为它的值本身属于对象的一部分，这么做的结果是无法真正提供一个对象以便从中获取成员的值，最终将引发错误

```CPP
class Sereen {
public:
//bkground表示一个在类中稍后定义的静态成员
    Screen& clear(char = bkground);
private:
    static const char bkground;
};
```

# IO库

1. `iostream`库：用于基本的输入输出操作

   该库包含`istream`(从流读取)、`ostream`(向流写入)、`iostream`(读写流)等类型

2. `fstream`库：用于文件操作

   该库包含`ifstream`(从文件读取)、`ofstream`(向文件写入)、`fstream`(读写文件)等类型

3. `sstream`库：用于字符串操作

   该库包含`istringstream`(从string读取)、`ostringstream`(向string写入)、`stringstream`(读写string)等类型

概念上，设备类型和字符大小都不会影响我们要执行的IO操作
标准库使我们能忽略不同类型的流之间的差异，这是通过继承机制**inheritance**实现的，利用模板可以使用具有继承关系的类，而不必了解继承机制如何工作的细节

简单地说，继承机制使我们可以声明一个特定的类继承自另一个类。我们通常可以将一个派生类（继承类）对象当作其基类（所继承的类）对象来使用

**不能对IO对象拷贝或赋值，也不能将形参或返回类型设置为流类型，进行IO操作的函数通常以引用方式传递和返回流，读写IO对象会改变其状态，因此不能是const引用**

## 条件状态

IO操作一个与生俱来的问题是可能发生错误。一些错误是可恢复的，而其他错误则发生在系统深处，已经超出了应用程序可以修正的范围。**在使用一个流之前，应该先检查它是否处于良好状态**，下面列出IO类所定义的些函数和标志，可以帮助我们访问和操纵流的条件状态（conditionstate）

```Css
strm::iostate
/*strm是一种IO类型，iostate是一种机器相关的类型，提供了表达条件状态的完整功能*/ 
/*这个类型应作为一个位集合来使用*/
/*IO库定义了4个iostate类型的constexpr值表示特定的位模式
这些值用来表示特定类型的IO条件，可以与位运算一起使用来一次性检测或设置多个标志位*/
strm::badbit
/*用来指出流已经崩溃，属于系统级错误，如不可恢复的读写错误*/
strm::failbit
/*表示一个io操作失败了，比如读取数字却读取的是字符，这是可以恢复的*/
strm::eofbit
/*用来表示文件达到文件结尾*/
strm::goodbit
/*表示流未处于错误状态，此值保证为0*/

s.eof()
/*若流s的eofbit置位，则返回true*/
s.fail()
/*若流s的failbit或badbit置位，则返回true*/
s.bad()
/*若流s的badbit置位，则返回true*/
s.good()
/*若流s处于有效状态，则返回true*/
s.clear()
/*将流s中所有条件状态复位，将流的状态置为有效。返回void*/
s.clear(flags)
/*复位flags指定的条件状态位。flags类型是strm::iostate类型。返回void*/
s.setstate(flags)
/*复位flags指定的条件状态位。flags类型是strm::iostate类型。返回void*/
s.rdstate()
/*返回流s当前条件状态，返回值类型为strm::iostate*/
```

**查询流的状态**

IO操作主要分为三类状态：

1. 可恢复的错误状态（`failbit`，一个字符等错误，流还可以继续使用）

2. 不可恢复的错误状态（`badbit`，系统级错误，流无法继续使用）

3. 流结尾（`eofbit`，流达到文件结束）

`eofbit, failbit, badbit`任一个被置位，检测流状态的条件会失败
`goodbit: eofbit, failbit, badbit`都无效（为0） 时，`goodbit`才置位（为1）

**一个流一旦发生错误，后续的IO操作都会失败，一直到流处于无错误状态后，IO操作才能正常进行**

```CPP
// 发生failbit
int val;
cin >> val;
// 控制台输入ABC会导致cin进入错误状态failbit

// 程序检查条件状态
while(cin >> val) {
  // ok 读操作成功
  ...
}
```

标准库还定义了一组函数来查询这些标志位的状态。操作`good`在所有错误位均未置而 `bad、fail`和 `eof` 则在对应错误位被置位时返回`true`。此外在`badbit`被置位时，`fail`也会返回`true`。
这意味着，使用`qood`或`fail`是确定流的总体状态的正确方法。实际上，我们将流当作条件使用的代码就等价于`!fail()`而`eof`和`bad`操作只能表示特定的错误

**管理条件状态**

流对象的`rdstate`成员返回一个`iostate`值，对应流的当前状态
`setstate`操作将给定条件位置位，表示发生了对应错误
`clear`成员是一个重载的成员它有一个不接受参数的版本，而另一个版本接受一个`iostate`类型的参数。
`clear`不接受参数的版本清除（复位）所有错误标志位。执行`clear`()后，调用`good`会返回`true`。我们可以这样使用这些成员：

```CPP
//记住cin的当前状态
auto old_state = cin.rdstate();//记住cin的当前状态
//使cin有效
cin.clear();
//使用cin
process_input(cin);
//将cin置为原有状态
cin.setstate(old_state);
//复位failbit和badbit，保持其他标志位不变
cin.clear(cin.rdstate()& ~cin.failbit & ~cin.badbit);
```

带参数的`clear`版本接受一个`iostate`值，表示流的新状态。为了复位单一的条件状态位，我们首先用`rdstate` 读出当前条件状态，然后用位操作将所需位复位来生成新的状态。例如将`failbit`和`badbit`复位，但保持`eofbit`不变

## 管理输出缓冲

**每个输出流都会管理一个缓冲区，保存程序读写的数据**

下面的代码可能立即打印，也可能被操作系统保存在缓冲区中，之后再打印：

```c++
cout << "Hello, C++";
```

有了缓冲机制，操作系统就可以将程序的多个输出操作组合成单一的系统级写操作。由于设备的写操作可能很耗时，允许操作系统将多个输出操作组合为单一的设备写操作可以带来很大的性能提升

缓冲刷新，指的是数据真正写到输出设备或文件。发生缓冲刷新的原因主要有：

- 程序正常执行完毕，如`main`函数`return`后；
- 缓冲区满时需要刷新缓冲区；
- 使用`endl`等操作符显式刷新缓冲区；
- 用`unitbuf`操纵符设置流的内部状态，来清空缓冲区；
- 关联流，读写被关联的流时，关联到的流的缓冲区会被刷新。如`cin`和`cerr`都关联到`cout`，读`cin`或`cerr`都会导致`cout`的缓冲区被刷新

***刷新缓冲区***

```c++
cout << "hello" << endl; // 输出hello和一个换行符，然后刷新缓冲区
cout << "hello" << flush; // 输出hello，不会添加换行符，然后刷新缓冲区
cout << "hello" << ends; // 输出hello和一个空字符，然后刷新缓冲区
```

***unitbuf操纵符***

```c++
cout << unitbuf; // 所有输出操作后立即刷新缓冲区
cout << nounitbuf; // 重置流，恢复正常的刷新机制
```

<span style="color: orange;">警告：如果程序崩溃，输出缓冲区不会被新</span>
如果程序异常终止，输出缓冲区是不会被刷新的。当一个程序崩溃后，它所输出的数据很可能停留在输出缓冲区中等待打印
当调试一个已经崩溃的程序时，需要确认那些自认为已经输出的数据确实已经刷新了。否则，可能将大量时间浪费在追踪代码为什么没有执行上。而实际上代码已经执行了。只是程序崩溃后缓冲区没有被刷新，输出数据被挂起没有打印而已

***关联输入和输出流***

<span style="color: orange;">当一个输入流关联到一个输出流时，任何试图从输入流读取数据的操作，都会先刷新关联的输出流</span>
如将cout和cin关联到一起：
`cin >> val;` 
将导致cout的缓冲区被刷新

**交互式系统通常应该关联输入流和输出流。这意味着所有输出，包括用户提示信息，都会在读操作之前被打印出来**

`tie`有2个重载的版本：不带参数的版本，如果本对象当前关联到一个输出流，返回指向输出流的指针，未关联则返回空指针（nullptr）；带参数的版本，接受指向`ostream`的指针。

```c++
cin.tie(&cout);
//将cin关联到cout
ostream *old_tie = cin.tie(nullptr);
//cin不与任何流关联
cin.tie(&cerr);
//关联cin和cerr。读取cin会刷新cerr而不是cout，当然这不是好主意，cin应该关联cout
cin.tie(old_tie); 
//恢复cin和cout之间的关联关系
```

`old_tie`是一个指向流对象的指针，用于保存之前与`cin`关联的流。通过使用`old_tie`，可以在重新关联`cin`之前保存对之前关联流的引用，以便在需要时可以恢复它

**每个流最多关联到一个流，但一个输出流可以被多个流关联**

## 文件输入输出

C++11 中，文件名可以是`string`类型对象，也可以是指向C风格字符串的指针

`fstream`特有操作：

```CPP
getline(fs, s);         
//从一个输入流fs读取一行字符串存入s中
fstream fs;
//创建一个未绑定的文件流，fstream是头文件fstream中定义的一个类型
fstream fs('data.txt');        
//创建一个fstream，并打开名为data.txt的文件。这些构造函数是explicit的。默认的文件模式mode依赖于fstream的类型
fstream fs('data.txt', mode);
//与上一个构造函数类似，但是按指定mode打开文件
fs.open('data.txt');     
//打开名为data.txt的文件并将文件与fs绑定并打开该文件，默认的文件模式mode依赖于fstream的类型，返回void，如果已打开会发生错误，导致failbit被置位
fs.is_open();            
//返回一个bool值，指出与fs关联的文件是否成功打开且未关闭
fs.close();            
//关闭与fs绑定的文件，返回void
```

当我们想要读写一个文件时，可以定义一个文件流对象，并将对象与文件关联起来。
每个文件流类都定义了一个名为`open`的成员函数，它完成一些系统相关的操作，来定位给定的文件，并视情况打开为读或写模式
创建文件流对象时，我们可以提供文件名（可选的）。如果提供了一个文件名，则`open`会自动被调用

**用 fstream 代替 iostream**

在要求使用基类型对象的地方，我们可以用继承类型的对象来替代，使用 `iostream` 类型的引用或指针作为参数的函数，都可以使用 `fstream` 或`sstream`来代替

+ 如果 `open` 失败，`failbit` 会被置位，**建议每次 open 后检测 open 是否成功**
+ 不能对已打开的文件流调用`open`

+ 当文件关闭后，可以将文件流关联到另一个文件
+ 当一个`fstream`对象被销毁时，`close`函数会自动被调用

## 文件模式

每次打开文件都以某种模式打开，如未指定即以该文件流类型的默认模式打开

- **in**：以只读方式打开
- **out**：以只写方式打开
- **app**：每次写操作前均定位到文件末尾
- **ate**：打开文件后即定位到文件末尾
- **trunc**：截断文件
- **binary**：以二进制方式进行IO

***文件模式的使用***

- 每个流对象都有默认的文件模式，ifstream 默认 in 模式打开文件，ofstream 默认 out，fstream 默认 in 和 out。
- 对 ifstream 对象不能设置 out 模式，对 ofstream 对象不能设置 in 模式
- 只有设置了 out 才能设置 trunc 模式，只有设置了 out 模式才可以设置 trunc 模式
- 设置了 trunc 就不能再设置 app 模式
- 默认情况下以 out 模式打开文件会使文件内容被清空，如果要保留文件内容需要同时指定 app 模式或 in 模式。
- `ate`和`binary`可用于如何类型的文件流对象，且可以与其他文件模式组合使用

```CPP
// 以输出模式打开文件并截断文件（即清空文件内容）
ofstream fout("file1.txt");                               
// 显示指定app模式+隐含的out模式
ofstream fout("file1.txt", ofstream::app);                
// 同上，只是将out模式显式地指定了一下
ofstream fout("file1.txt", ofstream::app | ofstream::out);
//未显式指定输出模式，隐式地以out模式打开，通常情况下，out模式意味着同时使用trunc模式
fout.open("file1.txt");
```

## string流

sstream 定义了 istringstream, ostringstream, stringstream来读写 string

**stringstream 特有操作**：

```CPP
stringstream strm;
//strm是一个未绑定的stringstream对象，sstream是头文件sstream中定义的一个类型
stringstream strm(s);
//strm是一个sstream对象，保存string s的一个拷贝，此构造函数是explicit的
strm.str()
//返回strm所保存的string的拷贝
strm.str(s)    
//将string s拷贝到strm中，返回void   
```

+ `istringstream`是输入流，即读操作，要将流中的内容输入到字符串中，因此定义和使用`istringstream`时流内必须有内容，所以在使用前要提前在流内保存一个字符串
+ `ostringstream`是输出流，即写操作，将流中的内容输出到字符串中，`ostringstream`可以在定义时即在流中保存一个字符串，也可以通过`<<`操作符获得字符串

****

# 顺序容器

一个容器就是一些特定类型对象的集合

**顺序容器sequential container**提供了控制元素存储和访问顺序的能力，顺序取决于元素加入容器时的位置，与元素值无关。
标准库提供了3种容器适配器，分别为容器操作定义了不同的接口，来与容器适配

***顺序容器类型*：**



|                  |                                                              |
| ---------------- | ------------------------------------------------------------ |
| **vector**       | **可变数组**，随机访问快，一般在尾部插入/删除元素。在尾部之外的插入/删除操作可能很慢 |
| **deque**        | **双端队列**，随机访问快,头尾位置插入/删除快                 |
| **list**         | **双向链表**，只支持双向顺序访问。在list中任何位置进行插入/删除操作速度都很快 |
| **forward_list** | **单向链表**，只支持单向顺序访问。在链表任何位置进行插入/删除操作速度都很快 |
| **array**        | **固定大小数组**，随机访问快。不能添加或删除元素，与内置数组相比更安全、更容易使用 |
| **string**       | 与vector相似的容器，但**专门用于保存字符**。随机访问快，不能添加或删除元素 |

<span style="color: orange;">advice：</span>

新标准库的容器比旧版本快得多、新标准库容器的性能几乎肯定与最精心优化过的同类数据结构一样好，通常会更好。**现代C++程序应该使用标准库容器，而不是更原始的数据结构，如内置数组**

**容器选择基本原则：**
+ 通常选择使用`vector`，除非有明确地使用其他容器的理由
+ 如果不确定使用哪种容器，可以只使用`vector`和`list`的公共操作：使用迭代器，不要用下标进行操作，避免随机访问。这样在必要时选择使用`vector`或`list`都很方便
- 如果程序有很多小元素，且空间额外开销很重要，就不要使用`list`或者`forward_list`
- 如果程序要求随机访问元素，使用`vector`或`deque`
- 如果程序要求在容器中间插入/删除元素，使用`list, forward_list`
- 如果程序要求在头尾位置插入/删除元素，但不会在中间插入/删除元素，使用`deque`
- 如果程序只有在读取输入时，才需要在容器中间位置插入元素，随后需要随机访问元素，则

1. 如果需要在容器中间位置添加元素，可以先用`vector`追加数据，然后调用`sort`函数进行排序，从而避免在中间位置添加元素
2. 如果必须在中间位置插入元素，可以考虑输入阶段用`list`，输入完成后将`list`内容拷贝到一个`vector`

+ 一般来说，应用中占主导地位的操作：执行的访问操作更多还是插入/删除更多决定了容器类型的选择

## 容器库概览

容器类型有3类操作：

1. 所有容器都提供的
2. 仅针对顺序容器、关联容器、无序容器的
3. 只适用于小部分容器的

一般来说，每个容器都定义在一个头文件中，文件名与类型名相同

顺序容器几乎可以保存任意类型元素，可以定义一个容器，其元素的类型是另一个容器，这种容器的定义与任何其他容器类型完全一样，在尖括号中指定元素类型（此种情况下，是另一种容器类型）。某些容器操作对元素类型有自己特殊要求

```CPP
vector<vector<string>> lines;
//vector的vector，外层vector每个元素都是内层vector，每个内层vector中的每个元素都是一个string
```

顺序容器构造的一个函数版本接受容器大小参数，它使用了元素默认构造函数，但某些类没有默认构造函数。可以定义保存这种类型对象的容器，但构造这种容器时不能只传递一个元素数目参数：

```CPP
// 假定noDefault是一个没有默认构造函数的类型
vector<noDefault> v1(10, init); // 正确，init是元素初始化器
vector<noDefault> v2(10); // 错误，因为noDefault没有默认构造函数，必须提供一个元素初始化器
```

### 容器操作

```CPP
类型别名
//此容器类型的迭代器类型
iterator
//可以读取元素，但不能修改元素的迭代器类型
const_iterator
//无符号整型，保存容器大小
size_type
//带符号整型，保存2个迭代器之间的距离
difference_type
//元素左值类型，即(value_type&)
value_type
//元素的const左值类型，即(const value_type&)
const_reference
    
构造函数
//默认构造函数，构造空容器    
C c; 
//构造c2的拷贝c1
C c1(c2);
//构造c，将迭代器b和e指定范围内的元素拷贝到e(array不支持)
C c(b,e); 
//列表初始化c
C c{a,b,c...}; 

赋值与swap
//将c1中元素替换为c2中元素
c1 = c2
//将c1中的元素替换为列表中元素(不适应于array)    
c1 = {a,b,c...} 
//交换a和b的元素
a.swap(b) 
//等价于a.swap(b)    
swap(a,b)
    
大小  
//c中元素数目(forward_list不支持)    
c.size()
//c可保存的最大元素数目    
c.max_size() 
//判断c中是否存储了元素。c为空返回false；否则返回true     
c.empty() 
    
添加/删除元素(不适用于array，不同容器中，这些操作的接口不同)
//将args中元素拷贝进c    
c.insert(args);
//使用inits构造c中的一个元素
c.emplace(inits); 
//删除args指定的元素
c.erase(args);
//删除c中的所有元素，返回void
c.clear();  

关系运算符
//所有容器都支持相等，不相等运算符
= , !=  
//关系运算符(无序关联容器不支持)
<, <=, >, >= 
    
获取迭代器
//返回指向c的首元素和尾元素之后位置的迭代器
c.begin(), c.end() 
//返回const_iterator
c.cbegin(), c.cend()
    
反向容器的额外成员(不支持forward_list)
//按逆序寻址元素的迭代器
reverse_iterator  
//不能修改元素的逆序迭代器    
const_reverse_iterator 
//返回指向c的尾元素和首元素之前位置的迭代器    
c.rbegin(), c.rend() 
//返回const_reverse_iterator    
c.crbegin(), c.crend()     
```

### 迭代器&&容器类型成员

1. 如果一个迭代器提供某个操作，那么所有提供相同操作的实现方式都相同。例如标准容器类型提供容器元素访问操作，而所有迭代器都是通过解引用运算符实现该操作
2. `forward_list`迭代器不支持递减运算符

***迭代器范围***

一个迭代器范围由一对迭代器表示，两个迭代器分别指向同一个容器中的元素或尾元素之后的位置
即元素位置范围：左闭合区间`[begin, end)`，**编译器不会强制要求`begin`和`end`的合理位置**，要注意`end`可以与`begin`指向相同的位置，但不能指向`begin`之前的位置

```CPP
// 利用循环+迭代器，遍历容器元素
while(begin != end) {
	*begin = val; // 通过解引用方式，访问begin指向的元素并赋值
	++begin; // 移动迭代器到下一位置
}
```

每个容器都定义了多种类型，除已经使用过的`size_type, iterator, const_iterato`，还提供反向遍历容器的反向迭代器

与正向迭代器相比，各种操作的含义也都发生了颠倒，例如对一个反向迭代器执行递增操作，会得到上一个元素

通过类型别名可以在不了解容器中元素类型的情况下使用它，如果需要元素类型，使用`value_type`，如果需要元素类型的一个引用，可以使用`reference`或`const_reference`，这些元素相关的类型别名在泛型编程中十分有用，使用这些类型必须显式地使用其类名：

```CPP
//iter是通过list<string>定义的一个迭代器类型
list<string>::iterator iter;
//count是通过vector<int>定义的difference_type类型
vector<int>::difference_type count;
```

```CPP
list<string> a = {"Milton", "Sharkspeare", "Austen"};
auto it1 = a.begin();  //list<string>::iterator
auto it2 = a.rbegin(); //list<string>::reverse_iterator
auto it3 = a.cbegin(); //list<string>::const_iterator
auto it4 = a.crbegin();//list<string>::const_reverse_iterator
```

### 容器定义和初始化

除`array`外，其他容器的默认构造函数都会创建一个指定类型的空容器，且都可以接受指定容器大小和元素初始值的参数
**两个容器的元素类型必须相同或者可以类型转换，才能用一个容器初始化另外一个**

```CPP
//默认构造函数，如果C是array，则c中元素按默认方式初始化，否则c为空
C c; 

//c1初始化为c2的拷贝，c1和c2必须是相同类型（容器类型相同 and 元素类型相同）。如果C是array，c1和c2大小必须相同
C c1(c2); 
C c1 = c2;

//c初始化为初始化列表中元素的拷贝。列表元素类型必须与c的元素类型相同
C c{a,b,c...}; 
C c = {a,b,c...};

//c初始化为迭代器b和e指定范围中的元素的拷贝。范围中元素的类型必须与C的元素类型相容（不适用于array
C c(b,e);

只有顺序容器(除array外，因为array有专门指定大小的方法)的构造函数才能接受大小参数
//seq包含n个元素，对这些元素进行值初始化；此构造函数是explicit的(不适用于string)
C seq(n); 
C seq(n,t); //seq包含n个初始化为值t的元素
```

***将一个容器初始化为另一个容器的拷贝有两种方法：***

1. 直接拷贝整个容器

   要求两个容器类型和元素类型必须匹配

2. 拷贝由一对迭代器指定的元素范围（array除外）

   不要求容器类型相同，元素类型也不一定相同，但是要能将元素转换为要初始化的容器的元素类型

```CPP
list<string> authors = {"Milton", "Shakespeare", "Austen"};
vector<const char*> articles = {"a", "an", "the"};
//正确：类型匹配
list<string> list2(authors); 
//错误：容器类型不匹配
deque<string> authlist(authors);
//错误：容器类型不匹配
vector<string> words(articles); 
//正确：当参数是迭代器对时，元素类型可以转换为要初始化的类型即可，const char*可以转换为string
//因为C++标准库设计时考虑到了与C风格的字符串的互操作性。C++的 std::string内部实现通常会存储一个字符数组，并且知道如何正确地处理这个数组，包括如何处理空字符\0作为字符串的结束标志
forward_list<string> words(articles.begin(),articles.end()); 
```

***列表初始化***

列表初始化指定容器每个元素值，可以避免C风格字符串初始化可能带来的类型转换问题，从而确保类型安全。这是因为 C 风格字符串初始化可能会将字符数组隐式地转换为 `char*` 指针，这可能会导致类型不匹配的问题。而列表初始化则明确指定了容器的类型和初始值，避免了类型不匹配的问题

```CPP
//每个容器有三个元素，用给定的初始化器进行初始化
list<string> authors = {"Milton", "Shakespeare", "Austen"};
vector<const char*> articles = {"a", "an", "the"};
```

***与容器大小相关的构造函数***
除上面提到与关联容器相同的构造函数外，顺序容器（除array外）还提供另一个构造函数，它接受一个容器大小和一个(可省略，但前提是该元素类型是内置类型或有默认构造函数)元素初始值，不提供初始值时，标准库会自动创建一个值初始化器：

```CPP
//10个int元素，每个都初始化-1
vector<int> ivec(10, -1); 
//10个string元素，每个都初始化为"hi!"
list<string> svec(10, "hi!");  
 //10个int元素，每个都初始化0
forward_list<int> ivec(10);
//10个string元素，每个都初始化为空string
deque<string> svec(10);  
```

**只有顺序容器的构造函数才接受大小参数，关联容器并不支持**

***标准库array具有固定大小***
大小是`array`类型的一部分，所以`array`不支持普通的容器构造函数，这些构造函数都会确定容器的大小，或隐式或显式。注意区分与其他顺序容器在指定容器大小方式上的区别

```CPP
//保存42个int的数组
array<int, 42> 
//保存10个string的数组    
array<string, 10> 

//使用array类型需要同时指定元素类型和数组大小
array<int, 10>::size_type i; //数组类型要包括元素类型和大小
array<int>::size_type j;//错误：因为array<int>不是一个类型，没有大小

//10个默认初始化为0的int
array<int, 10> ia1; 
//列表初始化
array<int, 10> ia2 = {0,1,2,3,4,5,6,7,8};
//ia3[0]为42，剩余元素为0
array<int, 10> ia3 = {42}; 
```

**不能对内置数组进行拷贝或对象赋值，但`array`均可以**，要求初始值的类型必须与要创建的容器类型相同，还要求元素类型和大小也一致，因为大小是`array`类型的一部分

### 赋值和swap

除`array`外，容器允许直接拷贝或赋值，如果两个容器大小不同，赋值后两者大小都与右边容器原大小相同

```CPP
//c1, c2是除array外的容器对象
c1 = c2; 
//c1是除array外的容器对象
c1 = {a,b,c};  

array<int, 10> a1 = {0,1,2,3,4,5,6,7,8};
array<int, 10> a2 = {0};//10个元素均为0
//a1元素全部替换为a2中元素
a1 = a2; 
////错误：不能将花括号列表赋值给array，只有在初始化时才可以
a2 = {0}; 
```

**容器赋值运算：**

```CPP
c1 = c2;
c = {a,b,c,...};

//交换c1和c2的元素，要求c1, c2具有相同的类型。swap通常比拷贝元素(c1 = c2)快
swap(c1, c2);
c1.swap(c2);

/*assign操作不适用于关联容器和array*/
//将seq元素替换为迭代器b和e所表示的范围中的元素，迭代器b和e不能指向seq的元素
seq.assign(b, e); 
//将seq元素替换为初始化列表il中的元素
seq.assign(il); 
//将seq中的元素替换为n个值为t的元素
seq.assign(n, t);
```

<span style="color: red;">**notice**：</span>

赋值运算会导致指向左边容器内部迭代器、引用和指针失效。而`swap`操作将容器内容交换，不会导致指向容器的迭代器、引用和指针失效（除容器类型`array`和`string`外）

***使用assign***

赋值运算符要求左右两边的运算对象具有相同类型，`assign`成员允许从一个不同但相容的类型赋值，或者从容器的一个子序列赋值，类似于用一个容器的迭代器对初始化另外一个容器

```CPP
//assign支持容器类型不匹配，但是元素类型相容的赋值
list<string> name;
vector<const char*> oldstyle;
//错误：容器类型不匹配，不能直接拷贝
names = oldstyle; 
//正确：可以将const char* 转换为string，也就是说元素类型相容
names.assign(oldstyle.begin(), oldstyle.end); 
```

**assign的重载版本**

```CPP
string& assign(const string& str);
//将整个字符串str的内容复制到当前字符串对象中，并返回当前字符串对象的引用
string& assign(const string& str, size_t pos, size_t len);
//将str中从pos位置开始的长度为len的子串复制到当前字符串对象中，并返回当前字符串对象的引用
string& assign(const char* s);
//将s指向的字符数组中的内容复制到当前字符串对象中，并返回当前字符串对象的引用
string& assign(size_t n, char c);
//将n个字符c复制到当前字符串对象中，并返回当前字符串对象的引用
```

***使用swap***

、除`array`外，`swap`不对任何元素进行拷贝、删除或插入，可以保证在常数时间内完成。元素本身并没有交换，`swap`只是交换了两个容器的内部数据结构，对于一些简单的数据类型`swap`可能只是交换指针或引用

而对`array`，`swap`操作会发生真正的元素交换，交换时间与`array`元素个数成正比

```CPP
// 交换svec1和svec2，交换后svec1大小为24，svec2大小为10
vector<string> svec1(10); // 10个元素的vector
vector<string> svec2(24); // 24个元素的vector
swap(svec1, svec2);
```

元素不会被移动意味着除`string`外，指向容器的迭代器、引用和指针在`swap`操作之后不会失效，它们仍然指向之前那些元素，但`swap`之后，这些元素已经被交换

在新标准库中，容器既提供成员函数版本的`swap`，也提供非成员版本的 `swap`。而早期标准库版本只提供成员函数版本的 `swap`。**非成员版本的 `swap`在泛型编程中是非常重要的。统一使用非成员版本的`swap`是一个好习惯**

`swap`函数可以访问类型中的私有和受保护成员，但是不能访问私有方法或受保护的非公开成员

非成员版本的 `swap` 通常比成员版本的 `swap` 更通用，因为成员版本的 `swap` 只能用于相同类型的对象交换。非成员版本的 `swap` 可以用于任何支持该操作的类型，包括用户自定义类型

### 容器大小操作&&关系运算符

1. **size()：** 返回容器元素数目，除`forward_list`外的容器都支持
2. **empty()：**`size`为0是返回`true`，否则返回`false`
3. **max_size()：**容器所能容纳的最大元素的值

每个容器类型都支持相等运算符(== && !=)，除无序关联容器外的所有容器都支持关系运算符，关系运算符左右两边的运算对象必须是相同类型的容器，且必须保存相同类型的元素，比如不能用**vector<int>**与**vector<double>**或**list<int>**进行比较，最后，**比较的元素类型要支持运算符**

比较两个容器实际上是进行元素的逐对比较。这些运算符的工作方式与`string`的关系运算类似：

+ 两个容器具有相同大小且所有元素都对应相等，则这两个容器相等，否则两个容器不等
+ 两个容器大小不同，但较小容器中每个元素都等于较大容器中的对应元素，则较小容器小于较大容器
+ 两个容器都不是另一个容器的前缀子序列，则它们的比较结果取决于第一个不相等的元素的比较结果

```CPP
vector<int> v1 = {1,3,5,7,9};
vector<int> v2 = {1,3,9};
vector<int> v3 = {1,3,5,7};
vector<int> v4 = {1,3,5,7,9};

v1 < v2 //true: 因为v1[2] = 5 < v[2] = 9 
v1 < v3 //false: 所有元素都相等，但v3元素个数更少
v1 == v4 //true: 所有元素都相等，且元素个数相等
v1 == v2 //false: 元素数目不一样，肯定不相等
    
/*判断不同类型容器的方法*/    
vector<int> v1 = {1,2,3};
list<int> l1 = {1,2,3};

cout << boolalpha << (vector<int>(l1.begin(),l1.end()) == v1) << endl;    
//boolalpha一个操纵符，用于改变布尔值的输出格式，输出布尔值true或false
//用迭代器创建了一个新的整数向量，其内容与容器l1相同，然后进行比较
```

## 顺序容器操作

### 向顺序容器添加元素

<span style="color: red;">**notice：**</span>

**向一个`vector`, `string`或`deque`插入元素，会使所有指向容器的迭代器、引用和指针失效**

+ `forward_list` 有自己专有版本的`insert`和`emplace`
+ `forward_list` 不支持`push_back`和`emplace_back`
+ `vector`和`string`不支持`push_front`和`emplace_front`

```CPP
/*操作会改变容器大小；array不支持*/
//在c尾部创建一个值为t或由args创建的元素。返回void
c.push_back(t); 
c.emplace_back(args);
//在c头部创建一个值为t或由args创建的元素。返回void
c.push_front(t); 
c.emplace_front(args); 
//在迭代器p指向的元素之前创建一个值为t或由args创建的元素。返回指向新添加的元素的迭代器
c.insert(p, t);  
c.emplace(p,args);
//在迭代器p指向的元素之前插入n个值为t的元素。返回指向新添加的第一个元素的迭代器；若n为0，则返回p
c.insert(p, n, t); 
//将迭代器b和e指定的范围内的元素插入到迭代器p指向的元素之前。b和e不能指向c中的元素。返回指向新添加的第一个元素的迭代器；若范围为空，则返回p
c.insert(p, b, e); 
//il是一个花括号元素值列表，插入到迭代器p指向元素位置之前。返回第一个新添加的元素的迭代器。若列表为空，则返回p
c.insert(p, il); 
```

**当使用这些操作时，必须记得不同容器使用不同的策略来分配元素空间，这些策略直接影响性能**

+ 在一个`vector`或`string`的尾部之外的任何位置，或是一个`deque`的首尾之外的任何位置添加元素，都需要移动元素

  

+ 向一个`vector`或`string`添加元素可能引起整个对象存储空间的重新分配。重新分配一个对象的存储空间需要分配新的内存，并将元素从旧的空间移动到新的空间中

***使用push_back***

除`array`和`forward_list`外，其他顺序容器都支持`push_back`将元素插入到容器末尾，因为`array`是固定大小，而`forward_list`是单向链表，插入元素到末尾需要遍历整个链表

```CPP
vector<string> container;
string word;
while(cin >> word) {
	container.push_back(word);//在vector末尾追加string
}

void pluralize(size_t cnt,string &word){
    if(cnt>1)
        word.push_back('s')//等价于word+='s'
}
```

通过`push_back`插入到容器的元素是拷贝，而不是对象本身。对容器内容的修改，不会影响用来初始化容器元素的对象

***使用push_front***

`push_front`将元素插入到容器开头，用法类似于`push_back`
`vector`不支持`push_front`

***在容器特定位置添加元素***

`insert`提供一般的添加功能，可以指定在容器中插入位置。
`forward_list`提供特殊版本的`insert`

```CPP
vector<string> svec;
list<string> slist;
 //将元素插入到指定位置之前，等价于slist.push_front("Hello!");
slist.insert(slist.begin(), "Hello!");

//vector不支持push_front，但可以insert到begin()
//插入到vector末尾以外位置速度都可能很慢
svec.insert(svec.begin(), "Hello!"); 
```

***插入范围内元素***

要插入的范围不能是容器自身迭代器范围

```CPP
//插入多个相同元素
//将10个string元素"Anna"插入到svec末尾
svec.insert(svec.end(), 10, "Anna"); 

vector<string> v = {"quasi", "simba", "frollo", "scar"}; 
//插入另外一对容器迭代器指定范围元素
//将容器v最后2个元素插入到slist开头
slist.insert(slist.begin(), v.end() - 2, v.end());

//插入元素列表
//将花括号括起来的元素列表插入到slist末尾
slist.insert(slist.end(), {"these", "words", "will", "go", "at", "the", "end"}); 
```

***使用insert返回值***

接受元素个数或范围的`insert`返回指向第一个新加入元素的迭代器，如果范围为空，不插入任何元素，`insert`会将第一个参数返回

通过使用`insert`的返回值，可以在容器中一个特定位置反复插入元素：

```CPP
list<string> lst;

auto iter = lst.begin();
while (cin >> word) {
    //等价于lst.push_front(word);
	iter = lst.insert(iter, word);
}
```

***使用emplace操作***

`emplace_front, emplace, emplace_back`用于构造而非拷贝元素，插入元素位置分别与`push_front, insert, push_back`相同

- `emplace` 函数在容器中直接构造元素，而不是像 `insert` 那样先构造一个临时对象。
- 这意味着 `emplace` 可以减少一次或多次额外的构造和析构操作，从而提高性能。
- 例如 `std::vector<std::string>`，使用 `emplace` 可以直接在正确的位置构造字符串，而不需要先构造一个临时的字符串对象

- `insert` 函数需要一个已构造的临时对象作为参数，或者使用 `value_type` 的构造函数来构造一个临时对象。
- 例如 `std::vector<std::string>`，使用 `insert` 可能会先构造一个临时的字符串对象，然后再将其复制或移动到正确的位置

**功能区别**：

- `insert`：在容器的指定位置插入一个元素，需要先生成对象，再将其复制或移动到容器中
- `emplace`：在容器的指定位置直接构造元素，不需要先生成对象，效率更高

**效率区别**：

- `insert`：需要调用拷贝或移动构造函数
- `emplace`：不需要调用拷贝或移动构造函数，效率更高

**适用场景**：

- `insert`：当需要插入多个元素时使用
- `emplace`：当需要直接构造元素时使用，特别是在性能关键的场景中

```CPP
//在c的末尾构造一个Sales_data对象
//使用三个参数的Sales_data构造函数
c.emplace_back("978-0590353403"，25，15.99);
//错误：没有接受三个参数的push_back版本
c.push_back("978-0590353403"，25，15.99);
//正确：创建一个临时的Sales_data对象传递给push_back 
c.push_back(Sales_data("978-0590353403"，25，15.99));
```

对`emplace_back`的调用和第二个`push_back`调用都会创建新的`Sales_data`对象。在调用 `emplaceback` 时，会在容器管理的内存空间中直接创建对象。而调用`push_back`则会创建一个局部临时对象，并将其压入容器中

`emplace`函数的参数根据元素类型而变化，参数必须与元素类型的构造函数相匹配

**`emplace`函数在容器中直接构造元素，传递给`emplace`函数的参数必须与元素类型的构造函数相匹配**

```CPP
void double_and_insert(std::vector<int>& v, int some_val) {
    auto mid = [&]{ return v.begin() + v.size() / 2; };
    for (auto curr = v.begin(); curr != mid(); ++curr)
        if (*curr == some_val)
            ++(curr = v.insert(curr, 2 * some_val));
/*如果没有递增，那么在遍历向量时，迭代器curr将不会自动向前移动。这意味着，即使插入了新元素，curr仍然会指向原来的位置。这会导致遍历过程中出现重复检查和插入相同元素的情况*/    
}
int main() {
    std::vector<int> v{ 1, 9, 1, 9, 9, 9, 1, 1 };
    double_and_insert(v, 1);

    for (auto i : v) 
        std::cout << i << std::endl;
}
```

### 访问元素

`at`和下标操作只适用于`string, vector, deque, array`

`back`不适用于`forward_list`

```CPP
//返回c中尾元素的引用。若c为空，函数行为未定义
c.back();  
//返回c中首元素的引用。若c为空，函数行为未定义
c.front(); 
//返回c中下标n的元素的引用，n是无符号整数。若n >= c.size()，函数行为未定义
c[n]; 
//返回下标为n的元素的引用。如果下标越界，则抛出out_of_range异常
c.at(n);   
```

访问容器元素时，一定要先确保存在

在容器中访问元素的成员函数返回的都是引用

### 删除元素

删除元素的成员函数并不检查参数，删除元素之前必须确保待删除元素存在

+ `forward_list`有特殊版本`erase`
+ `forward_list`不支持`pop_back`
+ `vector`和`string`不支持`pop_front`

```CPP
/*操作会改变容器大小，不适用于array*/
//删除c尾元素。若c为空，行为未定义，函数返回void
c.pop_back(); 
//删除c首元素。若c为空，行为未定义，函数返回void
c.pop_front(); 
//删除迭代器p所指元素，返回指向删除元素之后元素的迭代器
c.erase(p);
//删除迭代器b和e所指定范围内的元素，返回指向最后一个被删除元素之后的迭代器
c.erase(b,e);
//删除c所有元素。返回void
c.clear(); 
```

删除 `deque` 中除首尾位置之外的任何元素都会使所有选代器、引用和指针失效。指向`vector`或 `string`中删除点之后位置的迭代器、引用和指针都会失效

***pop_front和pop_back成员函数***
`pop_front`和`pop_back`分别删除首元素，尾元素

+ `vector、string`不支持`push_front、pop_front`
+ `forward_list`不支持`pop_back`

这些操作返回void,如果需要弹出的元素的值，就必须在执行弹出操作之前保存它

```CPP
while (!ilist.empty()) {
	process(ilist.front()); // 对ilist首元素进行自定义处理
	ilist.pop_front(); // 删除首元素
}
```

***从容器内部删除一个元素***
`erase`从容器指定位置或位置范围删除元素。用法类似于`insert`

```CPP
/*删除指定位置元素*/
list<int> lst = {0,1,2,3,4,5,6};
auto it = lst.begin();
while(it != lst.end()) {
	if (*it % 2)
        //删除指定位置元素，返回删除元素下一位置
		it = lst.erase(it); 
	else ++it;
}
/*删除指定范围元素*/
//迭代器elem1指向要删除的第一个元素
//elem2指向要删除的最后一个元素之后的位置
elem1 = lst.erase(elem1, elem2); //调用后elem1 == elem2

/*删除所有元素*/
//等价于lst.clear();
lst.erase(lst.begin(),lst.end());
```

### 特殊的forward_list操作

```CPP
//返回指向链表首元素之前不存在的元素的迭代器。该迭代器不能解引用
lst.before_begin(); 
//const_iterator
lst.cbefore_begin(); 

/*注意：插入元素的迭代器，不能是待插入容器的迭代器*/
//在迭代器p之后的位置插入元素。t是一个对象，n是数量，b, e是表示范围的一对迭代器
lst.insert_after(p, t); 
//p之后，插入n个重复的t
lst.insert_after(p, n, t);
//p之后插入迭代器对[b, e)之间的元素
lst.insert_after(p, b, e); 
//il是花括号列表
lst.insert_after(p, il); 

//使用args在p指定的位置之后创建一个元素，返回一个指向这个新元素的迭代器，若p是尾后迭代器，其函数行为未定义
emplace_after(p, args); 
//删除p指向的位置之后的元素
lst.erase_after(p);
//删除迭代器对[b, e)的元素，返回被删除元素之后的元素的迭代器，若不存在该元素，则返回尾后迭代器，如果p指向lst的尾元素或者是一个尾后迭代器，则函数行为未定义
lst.erase_after(b, e); 
```

当在 `forward_list` 中添加或删除元素时，我们必须关注两个迭代器一个指向我们要处理的元素，另一个指向其前驱

```CPP
forward_list<int> flst = { 0,1,2,3,4,5,6,7,8,9 };
auto prev = flst.before_begin();
auto curr = flst.begin();
while (curr != flst.end()) {
    if (*curr % 2)
        curr = flst.erase_after(prev);
//不能直接删除当前元素，所以使用erase_after(prev)删除前一个元素，并将当前位置移动到被删除元素之后。这样，我们可以确保只删除奇数
    else {
        prev = curr;
        ++curr;
//如果当前元素是偶数，则将prev和curr都向前移动一位        
 	    }
}
```

### 改变容器大小

`resize`用来增大或缩小容器，`array`不支持`resize`

+ 如果当前size < 增大后的size，则新元素添加到容器后部
+ 如果当前size > 增大后的size，则容器后部元素删除

```CPP
//10个int，值都是42
list<int> ilist(10, 42); 
//增加5个int到ilist末尾，值为0
ilist.resize(15);  
 //增加10个int到ilist末尾，值-1
ilist.resize(25, -1); 
//减少到5个int，也就是删除末尾20个元素
ilist.resize(5); 
```

### 容器操作可能使迭代器失效

<span style="color: red;">使用失效的迭代器、指针或引用是严重的运行时错误</span>

**在向容器添加元素后：**

+ 如果容器是`vector`或`string`，且存储空间被重新分配，则指向容器的迭代器指针和引用都会失效。如果存储空间未重新分配，指向插入位置之前的元素的迭代器、指针和引用仍有效，但指向插入位置之后元素的迭代器、指针和引用将会失效
+ 对于`deque`，插入到除首尾位置之外的任何位置都会导致选代器、指针和引用失效。如果在首尾位置添加元素，迭代器会失效，但指向存在的元素的引用和指针不会失效
+ 对于`list`和 `forward_list`，指向容器的迭代器（包括尾后选代器和首前迭代器）、指针和引用仍有效

**删除元素后：**

+ 对于`list`和 `forward_list`，指向容器其他位置的迭代器（包括尾后迭代器和首前迭代器）、引用和指针仍有效
+ 对于`deque`，如果在首尾之外的任何位置删除元素，那么指向被删除元素外其他元素的迭代器、引用或指针也会失效。如果是删除`deque`的尾元素，则尾后迭代器也会失效，但其他迭代器、引用和指针不受影响，如果是删除首元素，其他迭代器、引用和指针也不会受影响
+ 对于`vector`和`string`，指向被删元素之前元素的迭代器、引用和指针仍有效
+ 当我们删除元素时，尾后迭代器总是会失效

**管理迭代器**
当使用迭代器或指向容器元素的引用或指针时，最小化要求选代器必须保持有效的程序片段是一个好的方法
由于向迭代器添加元素和从迭代器删除元素的代码可能会使选代器失效，因此**必须保证每次改变容器的操作之后都正确地重新定位选代器**。这个建议对`vector`、`string`和`deque`尤为重要

添加或删除 `vector、string` 或`deque`元素的循环程序必须考虑迭代器、引用和指针可能失效的问题。程序必须保证每个循环步中都更新迭代器、引用或指针。如果循环中调用的是`insert`或`erase`，那么更新迭代器很容易。这些操作都返回迭代器，我们可以用来更新：

```CPP
vector<int> vi ={0，1，2，3，4，5，6，7，8，9};
auto iter = vi.begin();//调用begin而不是cbegin，因为要改变v1
/*删除偶数，复制奇数*/
while(iter != vi.end()) {
	if(*iter % 2){
		iter = vi.insert(iter,iter);//复制当前元素
         iter += 2;//向前移动选代器，跳过当前元素以及插入到它之前的元素
    else
		iter=vi.erase(iter);//删除偶数元素
//不应向前移动迭代器，iter指向我们删除的元素之后的元素
}
```

**`insert`在给定位置之前插入元素，然后返回指向新插入元素的迭代器**

因此，在调用`insert`后，`iter`指向新插入元素，位于我们正在处理的元素之前。我们将迭代器递增两次，恰好越过了新添加的元素和正在处理的元素，指向下一个未处理的元素

**不要保存end返回的迭代器**

如果在一个循环中插入或删除`deque`、`string`或`vector`中的元素，不要缓存`end`返回的迭代器

**必须在每次插入操作后重新调用`end()`，而不能在循环开始前保存它返回的迭代器：**

```CPP
//更安全的方法：在每个循环步添加/删除元素后都重新计算end 
while(begin！= v.end()) {
   ++begin;//向前移动begin，因为我们想在此元素之后插入元素
   begin = v.insert(begin，42);//插入新值
   ++begin;//向前移动begin，跳过我们刚刚加入的元素  
 }
```

### vector操作是如何增长的

`vector`和`string`的实现通常会分配比新的空间需求更大的内存空间，容器预留这些空间作为备用，用来保存更多的新元素，这样就不需要每次添加新元素都重新分配容器的内存空间

这种分配策略比每次添加新元素时都重新分配容器内存空间的策略要高效得多。虽然`vector`在每次重新分配内存空间时都要移动所有元素，但使用此分配策略后，其扩张操作通常比`list`和`deque`还要快

**管理容量的成员函数**

```CPP
c.shrink_to_fit(); 
//将capacity()减少为与size()相同大小
c.capacity(); 
//不重新分配内存空间的话，c可以保存多少元素
c.reserve(n); 
//分配至少能容纳n个元素的内存空间
```

+ `shrink_to_fit`只适用于`vector`、`string`和`deque`
+ `capacity`和`reserve`只适用于`vector`和`string`
+ `reserve`不会改变容器中元素数量，仅影响`vector`预先分配多大的内存空间
  + 需求容量 > 当前容量时，`reserve`至少分配与需求一样大的内存空间(可能更大)
  + 需求容量 <= 当前容量时`reserve`什么也不做
+ `resize`只改变容器中元素数目，而不是容器容量，不能使用它来减少容器预留的内存空间
+ `shrink_to_fit`只是一个请求，不一定成功退回内存空间，具体是否成功取决于实现

确保使用`push_back`操作向`vector`添加元素有 **高效率**，从技术角度看，就是在为空的`vector`上调用n次`push_back`来创建n个元素的`vector`，所花费时间不能超过n的常数倍

## 额外的string操作

***构造string的其他方法***

```CPP
string s(cp, n);
//s是cp指向的数组中前n个字符的拷贝，此数组至少应该包含n个字符
string s(s2, pos2); 
//s是string s2从下标pos2开始的字符的拷贝
sring s(s2, pos2, len2); 
//s是string s2从下标pos2开始len2个字符的拷贝，不管len2是多少，构造函数只拷贝到s2.size()-pos2个字符
```

n、len2和pos2都是无符号数

***substr操作***

```CPP
s.surstr(pos,n)
//返回一个s中从pos开始的n个字符的拷贝
string s("hello world");
string s2 = s.substr(0, 5); 
//s2 = hello = s[0..5), s[0]开始5个字符
string s3 = s.substr(6);
//s3 = world = s[6..end)    
```

### 改变string的其他方法

`string`类型支持顺序容器的赋值运算符以及`assign`、`insert`和`erase`操作

`string`还定义了额外的`insert`和`erase`版本，除了接受迭代器参数的版本，还有接受下标的版本。`insert`, `assign`还有接受C风格字符串的版本

```CPP
//接受下标版本insert，erase
s.insert(s.size(), 5, '!'); 
//在s末尾插入5个感叹号
s.erase(s.size()-5, 5); 
//从s删除末尾5个字符

//接受C风格字符串版本insert, assign
const char *cp = "Stately, plump Buck";
s.assign(cp, 7); 
//s == "Stately"
s.insert(s.size(), cp + 7); 
//s == "Stately, plump Buck", 在s末尾插入cp[7]到cp末尾的字符
s.insert(0,cp,1,6);
//在s[0]之前插入cp中cp[1]开始的6个字符
```

***append和replace函数***

+ `append`是在`string`末尾进行插入操作的简写方式
+ `replace`是在指定位置删除指定长度的元素

```CPP
//append
string s("C++ Primer"), s2 = 5;
s.append(" 4th Ed."); 
//等价于
s.insert(s.size(), " 4th Ed."); // s == "C++ Primer 4th Ed."

//replace
s2.replace(11, 3, "5th"); // 将s2[11]开始3个字符替换为"5th"
//等价于
s2.erase(11, 3);
s2.insert(11, "5th"); // s == "C++ Primer 5th Ed."

//替换的文本内容不一定跟删除的文本内容一样长
s2.replace(11, 3, "Fifth"); // s == "C++ Primer Fifth Ed."
//删除3个字符，但插入了5个字符
```

**修改string操作**

```CPP
s.insert(pos,args);
//在pos之前插入args指定的字符，pos是下标返回指向s的引用，pos是迭代器返回指向第一个插入字符的迭代器
s.erase(pos,len);
//删除从位置pos开始的len个字符，如果省略len则删至末尾，返回指向s的引用
s.assign(args);
//将s中字符替换为args指定的字符，返回一个指向s的引用
s.append(args);
//将args追加到s，返回一个指向s的引用
s.replace(range,args);
//删除s中range内的字符，替换为args指定字符，range可以是一个下标或长度，也可以是一对指向s的迭代器，返回一个指向s的引用
```

+ `append`和`assign`可以使用所有形式
+ `str`不能与s相同，迭代器b和e不能指向s
+ `replace`和`insert`允许的`args`形式依赖于range和pos如何指定
  + `replace(pos,len,args)`不允许初始化列表和迭代器对
  + `replace(b,e,args)`不允许`str,pos,len`
  + `insert(pos,args)`不允许初始化列表、迭代器和指针
  + `insert(iter,args)`只允许迭代器、初始化列表和`n,c`

```CPP
str 
//字符串
str,pos,len
//str中从pos开始len个字符
cp,len
//从cp指向的字符数组前len个字符    
cp
//cp指向的以空字符结尾的字符数组
n,c
//n个字符c
b,e    
//迭代器b和e指定范围内的字符
初始化列表
//花括号包围的，以逗号分隔的字符列表    
```



### string搜索操作

每个搜索操作都返回一个`string::size_type`值，表示匹配发生位置的下标，如果搜索失败，则返回一个`string::npos`的`static`成员，标准库将`npos`定义为一个`const string::size_type`类型，并初始化值为-1，`npos`类型是一个`unsigned`类型，因此用带符号类型保存这些函数的返回值是不建议的

搜索操作返回指定字符出现的下标，如果未找到则返回`npos`

```CPP
s.find(args); 
//查找s中args第一次出现的位置
s.rfind(args); 
//查找s中args最后一次出现的位置
s.find_first_of(args); 
//在s中查找args中任一字符第一次出现的位置
s.find_last_of(args);  
//在s中查找args中任何一个字符最后一次出现的位置
s.find_frist_not_of(args); 
//在s中查找第一个不在args中的字符
s.find_last_not_of(args);  
//在s中查找最后一个不在args中的字符

args必须是以下形式之一
c, pos 
//从s中位置pos开始查找字符c。pos默认为0
s2, pos
//...查找字符串s2.pos默认为0
cp, pos 
//...查找指针cp指向的以空字符结尾的C风格字符串。pos默认0
cp, pos, n 
//...查找cp指向的数组的前n个字符。pos和n无默认值
```



<span style="color: orange;"></span>

<span style="color: orange;"></span>

<span style="color: orange;"></span>