#  算法效率评估

算法**`algorithm`**是在有限时间内解决特定问题的一组指令或操作步骤

数据结构**` data structure`**是计算机中组织和存储数据的方式

数据结构与算法高度相关、紧密结合，具体表现以下三个方面：

1. 数据结构是算法的基石

   数据结构为算法提供了结构化存储的数据，以及用于操作数据的方法

2. 算法是数据结构发挥作用的舞台

   数据结构本身仅存储数据信息，结合算法才能解决特定问题

3. 算法通常可以基于不同的数据结构进行实现

   但执行效率可能相差很大，选择合适的数据结构是关键

在算法设计中，先后追求以下两个层面的目标

1. 找到问题解法：算法需要在规定的输入范围内，可靠地求得问题的正确解

2. 寻求最优解法：同一个问题可能存在多种解法，找到尽可能高效的算法

也就是说，在能够解决问题的前提下，算法效率已成为衡量算法优劣的主要评价指标，它包括以下两个维度

+ 时间效率：算法运行速度的快慢
+ 空间效率：算法占用内存空间的大小

渐近复杂度分析**`(asymptotic complexity analysis)`**，简称「复杂度分析」

复杂度分析体现算法运行所需的时间（空间）资源与输入数据大小之间的关系。它描述了随着输入数据大小的增加，算法执行所需时间和空间的增长趋势

+ 时间和空间资源分别对应时间复杂度**`time complexity`**和空间复杂度 **`space complexity`**
+ 随着输入数据大小的增加意味着复杂度反映了算法运行效率与输入数据体量之间的关系
+ 时间和空间的增长趋势表示**复杂度分析关注的不是运行时间或占用空间的具体值，而是时间或空间增长的快慢**

如果函数在返回前的最后一步才进行递归调用，则该函数可以被编译器或解释器优化，使其在空间效率上与迭代相当。这种情况被称为尾递归**`tail recursion`**

```CPP
/* 尾递归 */
int tailRecur(int n, int res) {
    // 终止条件
    if (n == 0)
        return res;
    // 尾递归调用
    return tailRecur(n - 1, res + n);
}
```

但许多编译器或解释器不支持尾递归优化



当处理与分治相关的算法问题时，递归往往比迭代的思路更加直观、代码更加易读。以“斐波那契数列”为例:

```CPP
/* 斐波那契数列：递归 */
int fib(int n) {
    // 终止条件 f(1) = 0, f(2) = 1
    if (n == 1 || n == 2)
        return n - 1;
    // 递归调用 f(n) = f(n-1) + f(n-2)
    int res = fib(n - 1) + fib(n - 2);
    // 返回结果 f(n)
    return res;
}
```

|          | 迭代                                   | 递归                                                         |
| :------: | :------------------------------------- | :----------------------------------------------------------- |
| 实现方式 | 循环结构                               | 函数调用自身                                                 |
| 时间效率 | 效率通常较高。无函数调用开销           | 每次函数调用都会产生开销                                     |
| 内存使用 | 通常使用固定大小的内存空间             | 累积函数调用可能使用大量的栈帧空间                           |
| 适用问题 | 适用于简单循环任务，代码直观、可读性好 | 可适用于子问题分解。如树、图、分治、回溯等，代码结构读性好、简洁、清晰 |

操作数量𝑇 (𝑛)中的各种系数、常数项都可以被忽略

1. 忽略常数项
2. 省略所有系数
3. 循环嵌套时使用乘法

```CPP
void algorithm(int n) {
    int a = 1;  // +0（技巧 1）
    a = a + n;  // +0（技巧 1）
    // +n（技巧 2）
    for (int i = 0; i < 5 * n + 1; i++) {
        cout << 0 << endl;
    }
    // +n*n（技巧 3）
    for (int i = 0; i < 2 * n; i++) {
        for (int j = 0; j < n + 1; j++) {
            cout << 0 << endl;
        }
    }
}
//算法时间复杂度为n的2次方

/*平方阶（冒泡排序）*/
int bubbleSort(vector<int> &nums) {
    int count = 0; // 计数器
    // 外循环：未排序区间为 [0, i]
    for (int i = nums.size() - 1; i > 0; i--) {
        // 内循环：将未排序区间 [0, i] 中的最大元素交换至该区间的最右端
        for (int j = 0; j < i; j++) {
            if (nums[j] > nums[j + 1]) {
                // 交换 nums[j] 与 nums[j + 1]
                int tmp = nums[j];
                nums[j] = nums[j + 1];
                nums[j + 1] = tmp;
                count += 3; // 元素交换包含 3 个单元操作
            }
        }
    }
    return count;
}
```

![常数阶、线性阶和平方阶的时间复杂度](https://www.hello-algo.com/chapter_computational_complexity/time_complexity.assets/time_complexity_constant_linear_quadratic.png)

```CPP
/*指数阶（循环实现）*/
int exponential(int n) {
    int count = 0, base = 1;
    // 细胞每轮一分为二，形成数列 1, 2, 4, 8, ..., 2^(n-1)
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < base; j++) {
            count++;
        }
        base *= 2;
    }
    // count = 1 + 2 + 4 + 8 + .. + 2^(n-1) = 2^n - 1
    return count;
}

/*指数阶（递归实现）*/
int expRecur(int n) {
    if (n == 1)
        return 1;
    return expRecur(n - 1) + expRecur(n - 1) + 1;
}
```

![指数阶的时间复杂度](https://www.hello-algo.com/chapter_computational_complexity/time_complexity.assets/time_complexity_exponential.png)

```CPP
/* 对数阶（循环实现） */
int logarithmic(float n) {
    int count = 0;
    while (n > 1) {
        n = n / 2;
        count++;
    }
    return count;
}

/* 对数阶（递归实现） */
int logRecur(float n) {
    if (n <= 1)
        return 0;
    return logRecur(n / 2) + 1;
}
```

![对数阶的时间复杂度](https://www.hello-algo.com/chapter_computational_complexity/time_complexity.assets/time_complexity_logarithmic.png)

```CPP
/* 线性对数阶 */
int linearLogRecur(float n) {
    if (n <= 1)
        return 1;
    int count = linearLogRecur(n / 2) + linearLogRecur(n / 2);
    for (int i = 0; i < n; i++) {
        count++;
    }
    return count;
}
//主流排序算法的时间复杂度通常为线性对数阶，例如快速排序、归并排序、堆排序等
```

![线性对数阶的时间复杂度](https://www.hello-algo.com/chapter_computational_complexity/time_complexity.assets/time_complexity_logarithmic_linear.png)

```CPP
/* 阶乘阶（递归实现） */
int factorialRecur(int n) {
    if (n == 0)
        return 1;
    int count = 0;
    // 从 1 个分裂出 n 个
    for (int i = 0; i < n; i++) {
        count += factorialRecur(n - 1);
    }
    return count;
}
```

![阶乘阶的时间复杂度](https://www.hello-algo.com/chapter_computational_complexity/time_complexity.assets/time_complexity_factorial.png)

# 数据结构分类

+ 逻辑结构：线性与非线性

+ 物理结构：连续与分散

**数字都是以补码的形式存储在计算机中**

+ 原码：我们将数字的二进制表示的最高位视为符号位，其中0表示正数，1表示负数，其余位表示数字的值
+ 反码：正数的反码与其原码相同，负数的反码是对其原码除符号位外的所有位取反
+ 补码：正数的补码与其原码相同，负数的补码是在其反码的基础上加1



**计算机内部的硬件电路主要是基于加法运算设计的，通过将加法与一些基本逻辑运算结合，计算机能够实现各种其他的数学运算**

根据 IEEE 754 标准，32‑bit 长度的float由以下三个部分构成

+ 符号位 S ：占 1 bit ，对应𝑏~31~
+ 指数位 E ：占 8 bits ，对应𝑏~30~𝑏~29~... 𝑏~23~
+ 分数位 N ：占 23 bits ，对应𝑏~22~𝑏~21~... 𝑏~0~
+ 二进制数float对应的值的计算方法：
+ val= (−1)𝑏31× 2^(𝑏30𝑏29...𝑏23)2−127^× (1.𝑏~22~𝑏~21~... 𝑏~0~)~2~


+ 转化到十进制下的计算公式：
+ val= (−1)^S^× 2^E-127^× (1 +N)

其中各项的取值范围：

S∈{0, 1},E∈ {1, 2, ... , 254}

(1 +N) =(1 +^23^∑~𝑖=1~𝑏~23−𝑖~2^−𝑖^) ⊂ [1, 2 − 2^−23^]

+ UTF‑8是国际上使用最广泛的 Unicode 编码方法。它是一种可变长的编码，使用1到4个字节来表示一个字符，根据字符的复杂性而变。ASCII 字符只需要 1 个字节，拉丁字母和希腊字母需要 2 个字节，常用的中文字符需要 3 个字节，其他的一些生僻字符需要 4 个字节。
+ UTF‑8 的编码规则并不复杂，分为以下两种情况：
+ 对于长度为 1 字节的字符，将最高位设置为0、其余 7 位设置为 Unicode 码点。值得注意的是，ASCII字符在 Unicode 字符集中占据了前 128 个码点。也就是说，UTF‑8 编码可以向下兼容 ASCII 码。这意味着我们可以使用 UTF‑8 来解析年代久远的 ASCII 码文本
+ 对于长度为𝑛字节的字符（其中𝑛 > 1），将首个字节的高𝑛位都设置为1、第𝑛 + 1位设置为0；从第二个字节开始，将每个字节的高 2 位都设置为10；其余所有位用于填充字符的 Unicode 码点

+ UTF‑16 和 UTF‑32 是等长的编码方法。在编码中文时，UTF‑16比 UTF‑8 的占用空间更小。Java 和 C# 等编程语言默认使用 UTF‑16 编码

# 内存与缓存

![计算机存储系统](https://www.hello-algo.com/chapter_array_and_linkedlist/ram_and_cache.assets/storage_pyramid.png)

硬盘用于长期存储大量数据，内存用于临时存储程序运行中正在处理的数据，而缓存则用于存储经常访问的数据和指令

程序运行时，数据会从硬盘中被读取到内存中，供 CPU 计算使用。缓存可以看作 CPU 的一部分，它通过智能地从内存加载数据，给 CPU 提供高速的数据读取，从而显著提升程序的执行效率，减少对较慢的内存的依赖

***在内存空间利用方面，数组和链表各自具有优势和局限性***

+ **内存是有限的，且同一块内存不能被多个程序共享**

  数组的元素紧密排列，不需要额外的空间来存储链表节点间的引用（指针），因此空间效率更高。

  数组需要一次性分配足够的连续内存空间，这可能导致内存浪费

  数组扩容也需要额外的时间和空间成本

  链表以“节点”为单位进行动态内存分配和回收，提供了更大的灵活性。

+ **在程序运行时，随着反复申请与释放内存，空闲内存的碎片化程度会越来越高，从而导致内存的利用效率降低**

  数组由于其连续的存储方式，相对不容易导致内存碎片化

  链表的元素是分散存储的，在频繁的插入与删除操作中，更容易导致内存碎片化

当 CPU 尝试访问的数据不在缓存中时，就会发生缓存未命中 `cache miss`，此时CPU不得不从速度较慢的内存中加载所需数据。

显然，**“缓存未命中”越少，CPU 读写数据的效率就越高**，程序性能也就越好。我们将CPU从缓存中成功获取数据的比例称为缓存命中率 `cache hit rate`，这个指标通常用来衡量缓存效率

为了尽可能达到更高的效率，缓存会采取以下数据加载机制：

- **缓存行**：缓存不是单个字节地存储与加载数据，而是以缓存行为单位。相比于单个字节的传输，缓存行的传输形式更加高效。
- **预取机制**：处理器会尝试预测数据访问模式（例如顺序访问、固定步长跳跃访问等），并根据特定模式将数据加载至缓存之中，从而提升命中率。
- **空间局部性**：如果一个数据被访问，那么它附近的数据可能近期也会被访问。因此，缓存在加载某一数据时，也会加载其附近的数据，以提高命中率。
- **时间局部性**：如果一个数据被访问，那么它在不久的将来很可能再次被访问。缓存利用这一原理，通过保留最近访问过的数据来提高命中率。

实际上，**数组和链表对缓存的利用效率是不同的**，主要体现在以下几个方面：

- **占用空间**：链表元素比数组元素占用空间更多，导致缓存中容纳的有效数据量更少。
- **缓存行**：链表数据分散在内存各处，而缓存是“按行加载”的，因此加载到无效数据的比例更高。
- **预取机制**：数组比链表的数据访问模式更具“可预测性”，即系统更容易猜出即将被加载的数据。
- **空间局部性**：数组被存储在集中的内存空间中，因此被加载数据附近的数据更有可能即将被访问。

总体而言，**数组具有更高的缓存命中率，因此它在操作效率上通常优于链表**。这使得在解决算法问题时，基于数组实现的数据结构往往更受欢迎。

需要注意的是，**高缓存效率并不意味着数组在所有情况下都优于链表**。实际应用中选择哪种数据结构，应根据具体需求来决定

- 在做算法题时，我们会倾向于选择基于数组实现的栈，因为它提供了更高的操作效率和随机访问的能力，代价仅是需要预先为数组分配一定的内存空间。
- 如果数据量非常大、动态性很高、栈的预期大小难以估计，那么基于链表实现的栈更加合适。链表能够将大量数据分散存储于内存的不同部分，并且避免了数组扩容产生的额外开销。

# 数组

```CPP
/* 初始化数组 */
// 存储在栈上
int arr[5];
int nums[5] = { 1, 3, 2, 5, 4 };
// 存储在堆上（需要手动释放空间）
int* arr1 = new int[5];
int* nums1 = new int[5] { 1, 3, 2, 5, 4 };

```

**访问元素**

**元素内存地址 = 数组内存地址 + 元素长度 x 元素索引**


![img](https://www.hello-algo.com/chapter_array_and_linkedlist/array.assets/array_memory_location_calculation.png)

**插入元素**

数组是连续的内存，如果想在数组中间插入一个元素，则需要将该元素之后的所有元素都向后移动一位，之后再把元素赋值给该索引

```CPP
/* 在数组的索引 index 处插入元素 num */
void insert(int *nums, int size, int num, int index) {
    // 把索引 index 以及之后的所有元素向后移动一位
    for (int i = size - 1; i > index; i--) {
        nums[i] = nums[i - 1];
    }
    // 将 num 赋给 index 处的元素
    nums[index] = num;
}
```



![数组插入元素示例](https://www.hello-algo.com/chapter_array_and_linkedlist/array.assets/array_insert_element.png)

**删除元素**

```CPP
/* 删除索引 index 处的元素 */
void remove(int *nums, int size, int index) {
    // 把索引 index 之后的所有元素向前移动一位
    for (int i = index; i < size - 1; i++) {
        nums[i] = nums[i + 1];
    }
}
```

![数组删除元素示例](https://www.hello-algo.com/chapter_array_and_linkedlist/array.assets/array_remove_element.png)

数组存储在连续的内存空间内，且元素类型相同。这种做法包含丰富的先验信息，系统可以利用这些信息来优化数据结构的操作效率：

- **空间效率高**：数组为数据分配了连续的内存块，无须额外的结构开销
- **支持随机访问**：数组允许在O(1)时间内访问任何元素
- **缓存局部性**：当访问数组元素时，计算机不仅会加载它，还会缓存其周围的其他数据，从而借助高速缓存来提升后续操作的执行速度

连续空间存储的局限性：

- **插入与删除效率低**：当数组中元素较多时，插入与删除操作需要移动大量的元素
- **长度不可变**：数组在初始化后长度就固定了，扩容数组需要将所有数据复制到新数组
- **空间浪费**：如果数组分配的大小超过实际所需，那么多余的空间就被浪费了

数组是一种基础且常见的数据结构，既频繁应用在各类算法之中，也可用于实现各种复杂数据结构：

- **随机访问**：如果我们想随机抽取一些样本，那么可以用数组存储，并生成一个随机序列，根据索引实现随机抽样。
- **排序和搜索**：数组是排序和搜索算法最常用的数据结构。快速排序、归并排序、二分查找等都主要在数组上进行。
- **查找表**：当需要快速查找一个元素或其对应关系时，可以使用数组作为查找表。假如我们想实现字符到 ASCII 码的映射，则可以将字符的 ASCII 码值作为索引，对应的元素存放在数组中的对应位置。
- **机器学习**：神经网络中大量使用了向量、矩阵、张量之间的线性代数运算，这些数据都是以数组的形式构建的。数组是神经网络编程中最常使用的数据结构。
- **数据结构实现**：数组可以用于实现栈、队列、哈希表、堆、图等数据结构。例如，图的邻接矩阵表示实际上是一个二维数组

# 链表

**链表定义与存储方式**

![链表定义与存储方式](https://www.hello-algo.com/chapter_array_and_linkedlist/linked_list.assets/linkedlist_definition.png)

- 链表的首个节点被称为“头节点”，最后一个节点被称为“尾节点”
- 尾节点指向的是“空”

```CPP
/* 链表节点结构体 */
struct ListNode {
    int val;         // 节点值
    ListNode *next;  // 指向下一节点的指针
    ListNode(int x) : val(x), next(nullptr) {}  // 构造函数
};
```

**插入节点**

```CPP
/* 在链表的节点 n0 之后插入节点 P */
void insert(ListNode *n0, ListNode *P) {
    ListNode *n1 = n0->next;
    P->next = n1;
    n0->next = P;
}
```



![链表插入节点示例](https://www.hello-algo.com/chapter_array_and_linkedlist/linked_list.assets/linkedlist_insert_node.png)

**删除节点**

```cpp
/* 删除链表的节点 n0 之后的首个节点 */
void remove(ListNode *n0) {
    if (n0->next == nullptr)
        return;
    // n0 -> P -> n1
    ListNode *P = n0->next;
    ListNode *n1 = P->next;
    n0->next = n1;
    // 释放内存
    delete P;
}
```

![链表删除节点](https://www.hello-algo.com/chapter_array_and_linkedlist/linked_list.assets/linkedlist_remove_node.png)

**访问节点**

```CPP
/* 访问链表中索引为 index 的节点 */
ListNode *access(ListNode *head, int index) {
    for (int i = 0; i < index; i++) {
        if (head == nullptr)
            return nullptr;
        head = head->next;
    }
    return head;
}
```

**查找节点**

```CPP
/* 在链表中查找值为 target 的首个节点 */
int find(ListNode *head, int target) {
    int index = 0;
    while (head != nullptr) {
        if (head->val == target)
            return index;
        head = head->next;
        index++;
    }
    return -1;
}
```

![image-20231218220353125](C:\Users\M87monster\AppData\Roaming\Typora\typora-user-images\image-20231218220353125.png)

单向链表通常用于实现栈、队列、哈希表和图等数据结构：

- **栈与队列**：当插入和删除操作都在链表的一端进行时，它表现出先进后出的特性，对应栈；当插入操作在链表的一端进行，删除操作在链表的另一端进行，它表现出先进先出的特性，对应队列
- **哈希表**：链式地址是解决哈希冲突的主流方案之一，在该方案中，所有冲突的元素都会被放到一个链表中
- **图**：邻接表是表示图的一种常用方式，其中图的每个顶点都与一个链表相关联，链表中的每个元素都代表与该顶点相连的其他顶点

双向链表常用于需要快速查找前一个和后一个元素的场景：

- **高级数据结构**：比如在红黑树、B 树中，我们需要访问节点的父节点，这可以通过在节点中保存一个指向父节点的引用来实现，类似于双向链表
- **浏览器历史**：在网页浏览器中，当用户点击前进或后退按钮时，浏览器需要知道用户访问过的前一个和后一个网页。双向链表的特性使得这种操作变得简单
- **LRU 算法**：在缓存淘汰（LRU）算法中，我们需要快速找到最近最少使用的数据，以及支持快速添加和删除节点。这时候使用双向链表就非常合适

环形链表常用于需要周期性操作的场景，比如操作系统的资源调度：

- **时间片轮转调度算法**：在操作系统中，时间片轮转调度算法是一种常见的 CPU 调度算法，它需要对一组进程进行循环。每个进程被赋予一个时间片，当时间片用完时，CPU 将切换到下一个进程。这种循环操作可以通过环形链表来实现
- **数据缓冲区**：在某些数据缓冲区的实现中，也可能会使用环形链表。比如在音频、视频播放器中，数据流可能会被分成多个缓冲块并放入一个环形链表，以便实现无缝播放

![常见链表种类](https://www.hello-algo.com/chapter_array_and_linkedlist/linked_list.assets/linkedlist_common_types.png)

# 列表

列表`list`是一个抽象的数据结构概念，它表示元素的有序集合，支持元素访问、修改、添加、删除和遍历等操作，无须使用者考虑容量限制的问题。列表可以基于链表或数组实现。

- 链表天然可以看作一个列表，其支持元素增删查改操作，并且可以灵活动态扩容
- 数组也支持元素增删查改，但由于其长度不可变，因此只能看作一个具有长度限制的列表

当使用数组实现列表时，长度不可变的性质会导致列表的实用性降低，许多编程语言中的标准库提供的列表是基于动态数组实现的

```CPP
/* 初始化列表 */
// 无初始值
vector<int> nums1;
// 有初始值
vector<int> nums = { 1, 3, 2, 5, 4 };
```

**访问元素**

```CPP
/* 访问元素 */
int num = nums[1];  // 访问索引 1 处的元素
/* 更新元素 */
nums[1] = 0;  // 将索引 1 处的元素更新为 0
```

**插入和删除元素**

在列表尾部添加元素的时间复杂度为O(1)，但插入和删除元素的效率仍与数组相同，时间复杂度为O(n)

```CPP
/* 清空列表 */
nums.clear();
/* 在尾部添加元素 */
nums.push_back(1);
nums.push_back(3);
nums.push_back(2);
nums.push_back(5);
nums.push_back(4);
/* 在中间插入元素 */
nums.insert(nums.begin() + 3, 6);  // 在索引 3 处插入数字 6
/* 删除元素 */
nums.erase(nums.begin() + 3);      // 删除索引 3 处的元素
```

**拼接列表**
```CPP
* 拼接两个列表 */
vector<int> nums1 = { 6, 8, 7, 10, 9 };
// 将列表 nums1 拼接到 nums 之后
nums.insert(nums.end(), nums1.begin(), nums1.end());
```

许多编程语言内置了列表

实现一个简易版列表，包括以下三个重点设计：

- **初始容量**：选取一个合理的数组初始容量
- **数量记录**：声明一个变量 `size` ，用于记录列表当前元素数量，并随着元素插入和删除实时更新
- **扩容机制**：若插入元素时列表容量已满，则需要进行扩容。先根据扩容倍数创建一个更大的数组，再将当前数组的所有元素依次移动至新数组

```CPP
/* 列表类 */
class MyList {
private:
    int* arr;             // 数组（存储列表元素）
    int arrCapacity = 10; // 列表容量
    int arrSize = 0;      // 列表长度（当前元素数量）
    int extendRatio = 2;  // 每次列表扩容的倍数

public:
    /* 构造方法 */
    MyList() {
        arr = new int[arrCapacity];
    }
    /* 析构方法 */
    ~MyList() {
        delete[] arr;
    }
    /* 获取列表长度（当前元素数量）*/
    int size() {
        return arrSize;
    }
    /* 获取列表容量 */
    int capacity() {
        return arrCapacity;
    }
    /* 访问元素 */
    int get(int index) {
        // 索引如果越界则抛出异常，下同
        if (index < 0 || index >= size())
            throw out_of_range("索引越界");
        return arr[index];
    }
    /* 更新元素 */
    void set(int index, int num) {
        if (index < 0 || index >= size())
            throw out_of_range("索引越界");
        arr[index] = num;
    }
    /* 在尾部添加元素 */
    void add(int num) {
        // 元素数量超出容量时，触发扩容机制
        if (size() == capacity())
            extendCapacity();
        arr[size()] = num;
        // 更新元素数量
        arrSize++;
    }
    /* 在中间插入元素 */
    void insert(int index, int num) {
        if (index < 0 || index >= size())
            throw out_of_range("索引越界");
        // 元素数量超出容量时，触发扩容机制
        if (size() == capacity())
            extendCapacity();
        // 将索引 index 以及之后的元素都向后移动一位
        for (int j = size() - 1; j >= index; j--) {
            arr[j + 1] = arr[j];
        }
        arr[index] = num;
        // 更新元素数量
        arrSize++;
    }
    /* 删除元素 */
    int remove(int index) {
        if (index < 0 || index >= size())
            throw out_of_range("索引越界");
        int num = arr[index];
        // 索引 i 之后的元素都向前移动一位
        for (int j = index; j < size() - 1; j++) {
            arr[j] = arr[j + 1];
        }
        // 更新元素数量
        arrSize--;
        // 返回被删除元素
        return num;
    }
    /* 列表扩容 */
    void extendCapacity() {
        // 新建一个长度为原数组 extendRatio 倍的新数组
        int newCapacity = capacity() * extendRatio;
        int* tmp = arr;
        arr = new int[newCapacity];
        // 将原数组中的所有元素复制到新数组
        for (int i = 0; i < size(); i++) {
            arr[i] = tmp[i];
        }
        // 释放内存
        delete[] tmp;
        arrCapacity = newCapacity;
    }
    /* 将列表转换为 Vector 用于打印 */
    vector<int> toVector() {
        // 仅转换有效长度范围内的列表元素
        vector<int> vec(size());
        for (int i = 0; i < size(); i++) {
            vec[i] = arr[i];
        }
        return vec;
    }
};
```

# 栈

栈 stack是一种遵循先入后出的逻辑的线性数据结构

+ 堆叠元素的顶部称为栈顶，底部称为栈底
+ 把元素添加到栈顶的操作叫作入栈，删除栈顶元素的操作叫作出栈

![栈的先入后出规则](https://www.hello-algo.com/chapter_stack_and_queue/stack.assets/stack_operations.png)

cpp语言内置的栈类
```CPP
/* 初始化栈 */
stack<int> stack;
/* 元素入栈 */
stack.push(1);
stack.push(3);
stack.push(2);
stack.push(5);
stack.push(4);
/* 访问栈顶元素 */
int top = stack.top();
/* 元素出栈 */
stack.pop(); // 无返回值
/* 获取栈的长度 */
int size = stack.size();
/* 判断是否为空 */
bool empty = stack.empty();
```

## 链表实现栈

使用链表实现栈时，可以将链表的头节点视为栈顶，尾节点视为栈底。对于入栈操作，只需将元素插入链表头部，这种节点插入方法被称为头插法。而对于出栈操作，只需将头节点从链表中删除即可

```CPP
/* 基于链表实现的栈 */
class LinkedListStack {
private:
    ListNode* stackTop; // 将头节点作为栈顶
    int stkSize;        // 栈的长度
public:
    LinkedListStack() {
        stackTop = nullptr;
        stkSize = 0;
    }
    ~LinkedListStack() {
        // 遍历链表删除节点，释放内存
        freeMemoryLinkedList(stackTop);
    }
    /* 获取栈的长度 */
    int size() {
        return stkSize;
    }
    /* 判断栈是否为空 */
    bool isEmpty() {
        return size() == 0;
    }
    /* 入栈 */
    void push(int num) {
        ListNode* node = new ListNode(num);
        node->next = stackTop;
        stackTop = node;
        stkSize++;
    }
    /* 出栈 */
    int pop() {
        int num = top();
        ListNode* tmp = stackTop;
        stackTop = stackTop->next;
        // 释放内存
        delete tmp;
        stkSize--;
        return num;
    }
    /* 访问栈顶元素 */
    int top() {
        if (isEmpty())
            throw out_of_range("栈为空");
        return stackTop->val;
    }
    /* 将 List 转化为 Array 并返回 */
    vector<int> toVector() {
        ListNode* node = stackTop;
        vector<int> res(size());
        for (int i = res.size() - 1; i >= 0; i--) {
            res[i] = node->val;
            node = node->next;
        }
        return res;
    }
};
```



## 数组实现栈

使用数组实现栈时，我们可以将数组的尾部作为栈顶

```CPP
/* 基于数组实现的栈 */
class ArrayStack {
private:
    vector<int> stack;
public:
    /* 获取栈的长度 */
    int size() {
        return stack.size();
    }
    /* 判断栈是否为空 */
    bool isEmpty() {
        return stack.size() == 0;
    }
    /* 入栈 */
    void push(int num) {
        stack.push_back(num);
    }
    /* 出栈 */
    int pop() {
        int num = top();
        stack.pop_back();
        return num;
    }
    /* 访问栈顶元素 */
    int top() {
        if (isEmpty())
            throw out_of_range("栈为空");
        return stack.back();
    }
    /* 返回 Vector */
    vector<int> toVector() {
        return stack;
    }
};
```

- 基于数组实现的栈在触发扩容时效率会降低，但由于扩容是低频操作，因此平均效率更高。
- 基于链表实现的栈可以提供更加稳定的效率表现
- 基于数组实现的栈可能造成一定的空间浪费，而链表节点需要额外存储指针，链表节点占用的空间相对较大，不能简单地确定哪种实现更加节省内存，需要针对具体情况进行分析

**栈典型应用**

- **浏览器中的后退与前进、软件中的撤销与反撤销**：每当我们打开新的网页，浏览器就会对上一个网页执行入栈，这样我们就可以通过后退操作回到上一个网页。后退操作实际上是在执行出栈。如果要同时支持后退和前进，那么需要两个栈来配合实现。
- **程序内存管理**：每次调用函数时，系统都会在栈顶添加一个栈帧，用于记录函数的上下文信息。在递归函数中，向下递推阶段会不断执行入栈操作，而向上回溯阶段则会不断执行出栈操作

# 队列

队列`queue`是一种遵循先入先出规则的线性数据结构

+ 队列头部称为队首，尾部称为队尾
+ 把元素加入队尾的操作称为入队，删除队首元素的操作称为出队

cpp内置的队列类：

```CPP
/* 初始化队列 */
queue<int> queue;
/* 元素入队 */
queue.push(1);
queue.push(3);
queue.push(2);
queue.push(5);
queue.push(4);
/* 访问队首元素 */
int front = queue.front();
/* 元素出队 */
queue.pop();
/* 获取队列的长度 */
int size = queue.size();
/* 判断队列是否为空 */
bool empty = queue.empty();
```

## 链表实现队列

可以将链表的头节点和尾节点分别视为队首和队尾，规定队尾仅可添加节点，队首仅可删除节点

```CPP
/* 基于链表实现的队列 */
class LinkedListQueue {
private:
    ListNode* front, * rear; // 头节点 front ，尾节点 rear
    int queSize;
public:
    LinkedListQueue() {
        front = nullptr;
        rear = nullptr;
        queSize = 0;
    }
    ~LinkedListQueue() {
        // 遍历链表删除节点，释放内存
        freeMemoryLinkedList(front);
    }
    /* 获取队列的长度 */
    int size() {
        return queSize;
    }
    /* 判断队列是否为空 */
    bool isEmpty() {
        return queSize == 0;
    }
    /* 入队 */
    void push(int num) {
        // 尾节点后添加 num
        ListNode* node = new ListNode(num);
        // 如果队列为空，则令头、尾节点都指向该节点
        if (front == nullptr) {
            front = node;
            rear = node;
        }
        // 如果队列不为空，则将该节点添加到尾节点后
        else {
            rear->next = node;
            rear = node;
        }
        queSize++;
    }
    /* 出队 */
    int pop() {
        int num = peek();
        // 删除头节点
        ListNode* tmp = front;
        front = front->next;
        // 释放内存
        delete tmp;
        queSize--;
        return num;
    }
    /* 访问队首元素 */
    int peek() {
        if (size() == 0)
            throw out_of_range("队列为空");
        return front->val;
    }
    /* 将链表转化为 Vector 并返回 */
    vector<int> toVector() {
        ListNode* node = front;
        vector<int> res(size());
        for (int i = 0; i < res.size(); i++) {
            res[i] = node->val;
            node = node->next;
        }
        return res;
    }
};
```

## 数组实现队列

在数组中删除首元素的时间复杂度为O(n)，这会导致出队操作效率较低。然而，我们可以采用以下巧妙方法来避免这个问题

我们可以使用一个变量 `front` 指向队首元素的索引，并维护一个变量 `size` 用于记录队列长度。定义 `rear = front + size` ，这个公式计算出的 `rear` 指向队尾元素之后的下一个位置。

基于此设计，**数组中包含元素的有效区间为 `[front, rear - 1]`**，各种操作的实现方法如图 5-6 所示。

- 入队操作：将输入元素赋值给 `rear` 索引处，并将 `size` 增加 1 。
- 出队操作：只需将 `front` 增加 1 ，并将 `size` 减少 1 

入队和出队操作都只需进行一次操作
在不断进行入队和出队的过程中，`front` 和 `rear` 都在向右移动，当它们到达数组尾部时就无法继续移动了。为了解决此问题，可以将数组视为首尾相接的“环形数组”。
对于环形数组，我们需要让 `front` 或 `rear` 在越过数组尾部时，直接回到数组头部继续遍历。这种周期性规律可以通过“取余操作”来实现，代码如下所示：

```CPP
/* 基于环形数组实现的队列 */
class ArrayQueue {
private:
    int* nums;       // 用于存储队列元素的数组
    int front;       // 队首指针，指向队首元素
    int queSize;     // 队列长度
    int queCapacity; // 队列容量
public:
    ArrayQueue(int capacity) {
        // 初始化数组
        nums = new int[capacity];
        queCapacity = capacity;
        front = queSize = 0;
    }
    ~ArrayQueue() {
        delete[] nums;
    }
    /* 获取队列的容量 */
    int capacity() {
        return queCapacity;
    }
    /* 获取队列的长度 */
    int size() {
        return queSize;
    }
    /* 判断队列是否为空 */
    bool isEmpty() {
        return size() == 0;
    }
    /* 入队 */
    void push(int num) {
        if (queSize == queCapacity) {
            cout << "队列已满" << endl;
            return;
        }
        // 计算队尾指针，指向队尾索引 + 1
        // 通过取余操作，实现 rear 越过数组尾部后回到头部
        int rear = (front + queSize) % queCapacity;
        // 将 num 添加至队尾
        nums[rear] = num;
        queSize++;
    }
    /* 出队 */
    int pop() {
        int num = peek();
        // 队首指针向后移动一位，若越过尾部则返回到数组头部
        front = (front + 1) % queCapacity;
        queSize--;
        return num;
    }
    /* 访问队首元素 */
    int peek() {
        if (isEmpty())
            throw out_of_range("队列为空");
        return nums[front];
    }
    /* 将数组转化为 Vector 并返回 */
    vector<int> toVector() {
        // 仅转换有效长度范围内的列表元素
        vector<int> arr(queSize);
        for (int i = 0, j = front; i < queSize; i++, j++) {
            arr[i] = nums[j % queCapacity];
        }
        return arr;
    }
};
```

**队列典型应用**

- **淘宝订单**。购物者下单后，订单将加入队列中，系统随后会根据顺序处理队列中的订单。在双十一期间，短时间内会产生海量订单，高并发成为工程师们需要重点攻克的问题。
- **各类待办事项**。任何需要实现“先来后到”功能的场景，例如打印机的任务队列、餐厅的出餐队列等，队列在这些场景中可以有效地维护处理顺序

## 双向队列

cpp内置双向队列：

```CPP
/* 初始化双向队列 */
deque<int> deque;
/* 元素入队 */
deque.push_back(2);   // 添加至队尾
deque.push_back(5);
deque.push_back(4);
deque.push_front(3);  // 添加至队首
deque.push_front(1);
/* 访问元素 */
int front = deque.front(); // 队首元素
int back = deque.back();   // 队尾元素
/* 元素出队 */
deque.pop_front();  // 队首元素出队
deque.pop_back();   // 队尾元素出队
/* 获取双向队列的长度 */
int size = deque.size();
/* 判断双向队列是否为空 */
bool empty = deque.empty();
```

对于双向队列而言，头部和尾部都可以执行入队和出队操作，采用“双向链表”作为双向队列的底层数据结构。

双向链表的头节点和尾节点视为双向队列的队首和队尾，同时实现在两端添加和删除节点的功能。

```CPP
/* 双向链表节点 */
struct DoublyListNode {
    int val;              // 节点值
    DoublyListNode* next; // 后继节点指针
    DoublyListNode* prev; // 前驱节点指针
    DoublyListNode(int val) : val(val), prev(nullptr), next(nullptr) { }
};
/* 基于双向链表实现的双向队列 */
class LinkedListDeque {
private:
    DoublyListNode* front, * rear; // 头节点 front ，尾节点 rear
    int queSize = 0;              // 双向队列的长度
public:
    /* 构造方法 */
    LinkedListDeque() : front(nullptr), rear(nullptr) {
    }
    /* 析构方法 */
    ~LinkedListDeque() {
        // 遍历链表删除节点，释放内存
        DoublyListNode* pre, * cur = front;
        while (cur != nullptr) {
            pre = cur;
            cur = cur->next;
            delete pre;
        }
    }
    /* 获取双向队列的长度 */
    int size() {
        return queSize;
    }
    /* 判断双向队列是否为空 */
    bool isEmpty() {
        return size() == 0;
    }
    /* 入队操作 */
    void push(int num, bool isFront) {
        DoublyListNode* node = new DoublyListNode(num);
        // 若链表为空，则令 front 和 rear 都指向 node
        if (isEmpty())
            front = rear = node;
        // 队首入队操作
        else if (isFront) {
            // 将 node 添加至链表头部
            front->prev = node;
            node->next = front;
            front = node; // 更新头节点
            // 队尾入队操作
        }
        else {
            // 将 node 添加至链表尾部
            rear->next = node;
            node->prev = rear;
            rear = node; // 更新尾节点
        }
        queSize++; // 更新队列长度
    }
    /* 队首入队 */
    void pushFirst(int num) {
        push(num, true);
    }
    /* 队尾入队 */
    void pushLast(int num) {
        push(num, false);
    }
    /* 出队操作 */
    int pop(bool isFront) {
        if (isEmpty())
            throw out_of_range("队列为空");
        int val;
        // 队首出队操作
        if (isFront) {
            val = front->val; // 暂存头节点值
            // 删除头节点
            DoublyListNode* fNext = front->next;
            if (fNext != nullptr) {
                fNext->prev = nullptr;
                front->next = nullptr;
                delete front;
            }
            front = fNext; // 更新头节点
            // 队尾出队操作
        }
        else {
            val = rear->val; // 暂存尾节点值
            // 删除尾节点
            DoublyListNode* rPrev = rear->prev;
            if (rPrev != nullptr) {
                rPrev->next = nullptr;
                rear->prev = nullptr;
                delete rear;
            }
            rear = rPrev; // 更新尾节点
        }
        queSize--; // 更新队列长度
        return val;
    }
    /* 队首出队 */
    int popFirst() {
        return pop(true);
    }
    /* 队尾出队 */
    int popLast() {
        return pop(false);
    }
    /* 访问队首元素 */
    int peekFirst() {
        if (isEmpty())
            throw out_of_range("双向队列为空");
        return front->val;
    }
    /* 访问队尾元素 */
    int peekLast() {
        if (isEmpty())
            throw out_of_range("双向队列为空");
        return rear->val;
    }
    /* 返回数组用于打印 */
    vector<int> toVector() {
        DoublyListNode* node = front;
        vector<int> res(size());
        for (int i = 0; i < res.size(); i++) {
            res[i] = node->val;
            node = node->next;
        }
        return res;
    }
};
```

- 栈是一种遵循先入后出原则的数据结构，可通过数组或链表来实现。
- 在时间效率方面，栈的数组实现具有较高的平均效率，但在扩容过程中，单次入栈操作的时间复杂度会劣化至O(n) 。相比之下，栈的链表实现具有更为稳定的效率表现。
- 在空间效率方面，栈的数组实现可能导致一定程度的空间浪费。但需要注意的是，链表节点所占用的内存空间比数组元素更大。
- 队列是一种遵循先入先出原则的数据结构，同样可以通过数组或链表来实现。在时间效率和空间效率的对比上，队列的结论与前述栈的结论相似。
- 双向队列是一种具有更高自由度的队列，它允许在两端进行元素的添加和删除操作

****

# 哈希表

hash table又称散列表，通过建立键和值之间的映射来实现高效的元素查询，向哈希表中输入一个 `key` ，可以在 O(1) 时间内获取对应的`value`

![哈希表的抽象表示](https://www.hello-algo.com/chapter_hashing/hash_map.assets/hash_table_lookup.png)

在哈希表中进行增删查改的时间复杂度都是 O(1) ，非常高效

![image-20240103181228487](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240103181228487.png)

unordered_map是C++标准库容器，实现了哈希表数据结构
```CPP
/* 初始化哈希表 */
unordered_map<int, string> map;

/* 添加操作 */
// 在哈希表中添加键值对 (key, value)
map[12836] = "小哈";
map[15937] = "小啰";
map[16750] = "小算";
map[13276] = "小法";
map[10583] = "小鸭";

/* 查询操作 */
// 向哈希表中输入键 key ，得到值 value
string name = map[15937];

/* 删除操作 */
// 在哈希表中删除键值对 (key, value)
map.erase(10583);
```

哈希表有三种常用的遍历方式：遍历键值对、遍历键和遍历值

```CPP
//遍历键值对key->value
for (auto kv: map) {
    cout << kv.first << " -> " << kv.second << endl;
}
//使用迭代器遍历key->value
for (auto iter = map.begin(); iter != map.end(); iter++) {
    cout << iter->first << "->" << iter->second << endl;
}
```

使用最简单的方法，仅用一个数组来实现哈希表，数组中的每个空位称为桶bucket，每个桶可存储一个键值对

通过哈希函数实现基于key定位对应的桶，作用是将一个较大的输入空间映射到一个较小的输出空间

+ 输入空间是所有key，输出空间是所有bucket(数组索引)
+ 输入一个key，通过哈希函数得到该key对应的键值对在数组的存储位置

哈希函数的计算过程分为以下两步

1. 通过某种哈希算法计算得到哈希值
2. 将哈希值对桶数量取模，从而获取该key对应的数组索引

然后就可以利用index在哈希表中访问对应的bucket，从而获取value

​									哈希函数工作原理

![哈希函数工作原理](https://www.hello-algo.com/chapter_hashing/hash_map.assets/hash_function.png)

```CPP
/* 键值对 */
struct Pair {
public:
    int key;
    string val;
    Pair(int key, string val) {
        this->key = key;
        this->val = val;
    }
};

/* 基于数组实现的哈希表 */
class ArrayHashMap {
private:
    vector<Pair*> buckets;

public:
    ArrayHashMap() {
        // 初始化数组，包含 100 个桶
        buckets = vector<Pair*>(100);
    }

    ~ArrayHashMap() {
        // 释放内存
        for (const auto& bucket : buckets) {
            delete bucket;
        }
        buckets.clear();
    }

    /* 哈希函数 */
    int hashFunc(int key) {
        int index = key % 100;
        return index;
    }

    /* 查询操作 */
    string get(int key) {
        int index = hashFunc(key);
        Pair* pair = buckets[index];
        if (pair == nullptr)
            return "";
        return pair->val;
    }

    /* 添加操作 */
    void put(int key, string val) {
        Pair* pair = new Pair(key, val);
        int index = hashFunc(key);
        buckets[index] = pair;
    }

    /* 删除操作 */
    void remove(int key) {
        int index = hashFunc(key);
        // 释放内存并置为 nullptr
        delete buckets[index];
        buckets[index] = nullptr;
    }

    /* 获取所有键值对 */
    vector<Pair*> pairSet() {
        vector<Pair*> pairSet;
        for (Pair* pair : buckets) {
            if (pair != nullptr) {
                pairSet.push_back(pair);
            }
        }
        return pairSet;
    }

    /* 获取所有键 */
    vector<int> keySet() {
        vector<int> keySet;
        for (Pair* pair : buckets) {
            if (pair != nullptr) {
                keySet.push_back(pair->key);
            }
        }
        return keySet;
    }

    /* 获取所有值 */
    vector<string> valueSet() {
        vector<string> valueSet;
        for (Pair* pair : buckets) {
            if (pair != nullptr) {
                valueSet.push_back(pair->val);
            }
        }
        return valueSet;
    }

    /* 打印哈希表 */
    void print() {
        for (Pair* kv : pairSet()) {
            cout << kv->key << " -> " << kv->val << endl;
        }
    }
};
```



## 哈希冲突

哈希函数的作用是将所有 `key` 构成的输入空间映射到数组所有`index`构成的输出空间，而输入空间往往远大于输出空间，因此一定存在多个输入对应相同输出的情况

上述示例中的哈希函数，当输入的 `key` 后两位相同时，哈希函数的输出结果也相同：

```CPP
12836 % 100 = 36
20336 % 100 = 36
```

两个学号指向了同一个姓名，这显然是不对的。将这种**多个输入对应同一输出的情况称为哈希冲突 hash collision**

![哈希冲突示例](https://www.hello-algo.com/chapter_hashing/hash_map.assets/hash_collision.png)

哈希表容量越大，多个 `key` 被分配到同一个桶中的概率就越低，冲突就越少。**因此可以通过扩容哈希表来减少哈希冲突**

![哈希表扩容](https://www.hello-algo.com/chapter_hashing/hash_map.assets/hash_table_reshash.png)

如果`key`是336就又冲突了，哈希表扩容需将所有键值对从原哈希表迁移至新哈希表，非常耗时，并且由于哈希表容量改变，需要通过哈希函数来重新计算所有键值对的存储位置，这进一步增加了扩容过程的计算开销。为此，编程语言通常会预留足够大的哈希表容量，防止频繁扩容

负载因子load factor是哈希表的一个重要概念，其定义为**哈希表的元素数量除以桶数量**，用于衡量哈希冲突的严重程度，也常作为哈希表扩容的触发条件



哈希表扩容需要进行大量的数据搬运与哈希值计算。为了提升效率，可以采用以下策略：

1. 改良哈希表数据结构，使哈希表可以在出现哈希冲突时正常工作
2. 仅在必要时，即当哈希冲突比较严重时，才执行扩容操作

**链式地址**

在原始哈希表中，每个桶仅能存储一个键值对。链式地址separate chaining将单个元素转换为链表，将键值对作为链表节点，将所有发生冲突的键值对都存储在同一链表中

![链式地址哈希表](https://www.hello-algo.com/chapter_hashing/hash_collision.assets/hash_table_chaining.png)

基于链式地址实现的哈希表的操作方法也随之改变

- **查询元素**：输入 `key` ，经过哈希函数得到桶索引，即可访问链表头节点，然后遍历链表并对比 `key` 以查找目标键值对
- **添加元素**：首先通过哈希函数访问链表头节点，然后将节点（键值对）添加到链表中
- **删除元素**：根据哈希函数的结果访问链表头部，接着遍历链表以查找目标节点并将其删除

链式地址存在以下局限性。

- **占用空间增大**：链表包含节点指针，它相比数组更加耗费内存空间
- **查询效率降低**：因为需要线性遍历链表来查找对应元素

```CPP
/* 链式地址哈希表 */
class HashMapChaining {
private:
    int size;                       // 键值对数量
    int capacity;                   // 哈希表容量
    double loadThres;               // 触发扩容的负载因子阈值
    int extendRatio;                // 扩容倍数
    vector<vector<Pair*>> buckets;  // 桶数组

public:
    /* 构造方法 */
    HashMapChaining() : size(0), capacity(4), loadThres(2.0 / 3.0), extendRatio(2) {
        buckets.resize(capacity);
    }

    /* 析构方法 */
    ~HashMapChaining() {
        for (auto& bucket : buckets) {
            for (Pair* pair : bucket) {
                // 释放内存
                delete pair;
            }
        }
    }

    /* 哈希函数 */
    int hashFunc(int key) {
        return key % capacity;
    }

    /* 负载因子 */
    double loadFactor() {
        return (double)size / (double)capacity;
    }

    /* 查询操作 */
    string get(int key) {
        int index = hashFunc(key);
        // 遍历桶，若找到 key ，则返回对应 val
        for (Pair* pair : buckets[index]) {
            if (pair->key == key) {
                return pair->val;
            }
        }
        // 若未找到 key ，则返回空字符串
        return "";
    }

    /* 添加操作 */
    void put(int key, string val) {
        // 当负载因子超过阈值时，执行扩容
        if (loadFactor() > loadThres) {
            extend();
        }
        int index = hashFunc(key);
        // 遍历桶，若遇到指定 key ，则更新对应 val 并返回
        for (Pair* pair : buckets[index]) {
            if (pair->key == key) {
                pair->val = val;
                return;
            }
        }
        // 若无该 key ，则将键值对添加至尾部
        buckets[index].push_back(new Pair(key, val));
        size++;
    }

    /* 删除操作 */
    void remove(int key) {
        int index = hashFunc(key);
        auto& bucket = buckets[index];
        // 遍历桶，从中删除键值对
        for (int i = 0; i < bucket.size(); i++) {
            if (bucket[i]->key == key) {
                Pair* tmp = bucket[i];
                bucket.erase(bucket.begin() + i); // 从中删除键值对
                delete tmp;                       // 释放内存
                size--;
                return;
            }
        }
    }

    /* 扩容哈希表 */
    void extend() {
        // 暂存原哈希表
        vector<vector<Pair*>> bucketsTmp = buckets;
        // 初始化扩容后的新哈希表
        capacity *= extendRatio;
        buckets.clear();
        buckets.resize(capacity);
        size = 0;
        // 将键值对从原哈希表搬运至新哈希表
        for (auto& bucket : bucketsTmp) {
            for (Pair* pair : bucket) {
                put(pair->key, pair->val);
                // 释放内存
                delete pair;
            }
        }
    }

    /* 打印哈希表 */
    void print() {
        for (auto& bucket : buckets) {
            cout << "[";
            for (Pair* pair : bucket) {
                cout << pair->key << " -> " << pair->val << ", ";
            }
            cout << "]\n";
        }
    }
};
```

当链表很长时，查询效率O(n)很差。此时可以将链表转换为AVL 树或红黑树，从而将查询操作的时间复杂度优化至 O(logn)

**开放寻址**

开放寻址 open addressing不引入额外的数据结构，而是通过多次探测来处理哈希冲突，探测方式主要包括线性探测、平方探测和多次哈希等

1. **线性探测**

   线性探测采用固定步长的线性搜索来进行探测，其操作方法与普通哈希表有所不同

   - **插入元素**：通过哈希函数计算桶索引，若发现桶内已有元素，则从冲突位置向后线性遍历(步长通常为1)，直至找到空桶，将元素插入其中
   - **查找元素**：若发现哈希冲突，则使用相同步长向后进行线性遍历，直到找到对应元素，返回 `value` 即可；如果遇到空桶，说明目标元素不在哈希表中，返回 `None`

   ![开放寻址（线性探测）哈希表的键值对分布](https://www.hello-algo.com/chapter_hashing/hash_collision.assets/hash_table_linear_probing.png)

此哈希函数中最后两位相同的 `key` 都会被映射到相同的桶。而通过线性探测，它们被依次存储在该桶以及之下的桶中

**线性探测容易产生聚焦现象**，具体来说，数组中连续被占用的位置越长，这些连续位置发生哈希冲突的可能性越大，从而进一步促使该位置的聚堆生长，形成恶性循环，最终导致增删查改操作效率劣化

**不能在开放寻址哈希表中直接删除元素**，因为删除元素会在数组内产生一个空桶None，而当查询元素时，线性探测到该空桶就会返回，因此在该空桶之下的元素都无法再被访问到，程序可能误判这些元素不存在

![在开放寻址中删除元素导致的查询问题](https://www.hello-algo.com/chapter_hashing/hash_collision.assets/hash_table_open_addressing_deletion.png)

可以采用懒删除 lazy deletion机制：它不直接从哈希表中移除元素，**而是利用一个常量 `TOMBSTONE` 来标记这个桶**。在该机制下，`None` 和 `TOMBSTONE` 都代表空桶，都可以放置键值对。但不同的是，线性探测到 `TOMBSTONE` 时应该继续遍历，因为其之下可能还存在键值对

然而，**懒删除可能会加速哈希表的性能退化**。这是因为每次删除操作都会产生一个删除标记，随着 `TOMBSTONE` 的增加，搜索时间也会增加，因为线性探测可能需要跳过多个 `TOMBSTONE` 才能找到目标元素

为此考虑在线性探测中记录遇到的首个 `TOMBSTONE` 的索引，并将搜索到的目标元素与该 `TOMBSTONE` 交换位置。这样做的好处是当每次查询或添加元素时，元素会被移动至距离理想位置（探测起始点）更近的桶，从而优化查询效率

一个包含懒删除的开放寻址（线性探测）哈希表。为了更加充分地使用哈希表的空间，将哈希表看作一个环形数组，当越过数组尾部时，回到头部继续遍历：

```cpp
/* 开放寻址哈希表 */
class HashMapOpenAddressing {
  private:
    int size;                             // 键值对数量
    int capacity = 4;                     // 哈希表容量
    const double loadThres = 2.0 / 3.0;     // 触发扩容的负载因子阈值
    const int extendRatio = 2;            // 扩容倍数
    vector<Pair *> buckets;               // 桶数组
    Pair *TOMBSTONE = new Pair(-1, "-1"); // 删除标记

  public:
    /* 构造方法 */
    HashMapOpenAddressing() : size(0), buckets(capacity, nullptr) {
    }

    /* 析构方法 */
    ~HashMapOpenAddressing() {
        for (Pair *pair : buckets) {
            if (pair != nullptr && pair != TOMBSTONE) {
                delete pair;
            }
        }
        delete TOMBSTONE;
    }

    /* 哈希函数 */
    int hashFunc(int key) {
        return key % capacity;
    }

    /* 负载因子 */
    double loadFactor() {
        return (double)size / capacity;
    }

    /* 搜索 key 对应的桶索引 */
    int findBucket(int key) {
        int index = hashFunc(key);
        int firstTombstone = -1;
        // 线性探测，当遇到空桶时跳出
        while (buckets[index] != nullptr) {
            // 若遇到 key ，返回对应的桶索引
            if (buckets[index]->key == key) {
                // 若之前遇到了删除标记，则将键值对移动至该索引处
                if (firstTombstone != -1) {
                    buckets[firstTombstone] = buckets[index];
                    buckets[index] = TOMBSTONE;
                    return firstTombstone; // 返回移动后的桶索引
                }
                return index; // 返回桶索引
            }
            // 记录遇到的首个删除标记
            if (firstTombstone == -1 && buckets[index] == TOMBSTONE) {
                firstTombstone = index;
            }
            // 计算桶索引，越过尾部则返回头部
            index = (index + 1) % capacity;
        }
        // 若 key 不存在，则返回添加点的索引
        return firstTombstone == -1 ? index : firstTombstone;
    }

    /* 查询操作 */
    string get(int key) {
        // 搜索 key 对应的桶索引
        int index = findBucket(key);
        // 若找到键值对，则返回对应 val
        if (buckets[index] != nullptr && buckets[index] != TOMBSTONE) {
            return buckets[index]->val;
        }
        // 若键值对不存在，则返回空字符串
        return "";
    }

    /* 添加操作 */
    void put(int key, string val) {
        // 当负载因子超过阈值时，执行扩容
        if (loadFactor() > loadThres) {
            extend();
        }
        // 搜索 key 对应的桶索引
        int index = findBucket(key);
        // 若找到键值对，则覆盖 val 并返回
        if (buckets[index] != nullptr && buckets[index] != TOMBSTONE) {
            buckets[index]->val = val;
            return;
        }
        // 若键值对不存在，则添加该键值对
        buckets[index] = new Pair(key, val);
        size++;
    }

    /* 删除操作 */
    void remove(int key) {
        // 搜索 key 对应的桶索引
        int index = findBucket(key);
        // 若找到键值对，则用删除标记覆盖它
        if (buckets[index] != nullptr && buckets[index] != TOMBSTONE) {
            delete buckets[index];
            buckets[index] = TOMBSTONE;
            size--;
        }
    }

    /* 扩容哈希表 */
    void extend() {
        // 暂存原哈希表
        vector<Pair *> bucketsTmp = buckets;
        // 初始化扩容后的新哈希表
        capacity *= extendRatio;
        buckets = vector<Pair *>(capacity, nullptr);
        size = 0;
        // 将键值对从原哈希表搬运至新哈希表
        for (Pair *pair : bucketsTmp) {
            if (pair != nullptr && pair != TOMBSTONE) {
                put(pair->key, pair->val);
                delete pair;
            }
        }
    }

    /* 打印哈希表 */
    void print() {
        for (Pair *pair : buckets) {
            if (pair == nullptr) {
                cout << "nullptr" << endl;
            } else if (pair == TOMBSTONE) {
                cout << "TOMBSTONE" << endl;
            } else {
                cout << pair->key << " -> " << pair->val << endl;
            }
        }
    }
};
```

2. **平方探测**

   平方探测与线性探测类似，都是开放寻址的常见策略之一。当发生冲突时，平方探测不是简单地跳过一个固定的步数，而是跳过探测次数的平方的步数，即 1,4,9,… 步

   平方探测主要具有以下优势。

   - 平方探测通过跳过探测次数平方的距离，试图缓解线性探测的聚集效应
   - 平方探测会跳过更大的距离来寻找空位置，有助于数据分布得更加均匀

   然而，平方探测并不是完美的

   - 仍然存在聚集现象，即某些位置比其他位置更容易被占用
   - 由于平方的增长，平方探测可能不会探测整个哈希表，这意味着即使哈希表中有空桶，平方探测也可能无法访问到它

3. **多次哈希**

   多次哈希方法使用多个哈希函数 f~1~(x)、 f~2~(x) 、f~3~(x)、… 进行探测。

   - **插入元素**：若哈希函数 f~1~(x)出现冲突，则尝试 f~2~(x) ，以此类推，直到找到空位后插入元素
   - **查找元素**：在相同的哈希函数顺序下进行查找，直到找到目标元素时返回；若遇到空位或已尝试所有哈希函数，说明哈希表中不存在该元素，则返回 `None` 

   与线性探测相比，多次哈希方法不易产生聚集，但多个哈希函数会带来额外的计算量

## 哈希算法

无论是开放寻址还是链式地址，它们只能保证哈希表可以在发生冲突时正常工作，而无法减少哈希冲突的发生

**键值对的分布情况由哈希函数决定**，**哈希算法 `hash()` 决定了输出值**，为了降低哈希冲突的发生概率，应当将注意力集中在哈希算法 `hash()` 的设计上

哈希算法应具备以下特点：

- **确定性**：对于相同的输入，哈希算法应始终产生相同的输出。这样才能确保哈希表是可靠的
- **效率高**：计算哈希值的过程应该足够快。计算开销越小，哈希表的实用性越高
- **均匀分布**：哈希算法应使得键值对均匀分布在哈希表中。分布越均匀，哈希冲突的概率就越低

哈希算法除了可以用于实现哈希表，还广泛应用于其他领域中：

- **密码存储**：为了保护用户密码的安全，系统通常不会直接存储用户的明文密码，而是存储密码的哈希值。当用户输入密码时，系统会对输入的密码计算哈希值，然后与存储的哈希值进行比较。如果两者匹配，那么密码就被视为正确
- **数据完整性检查**：数据发送方可以计算数据的哈希值并将其一同发送；接收方可以重新计算接收到的数据的哈希值，并与接收到的哈希值进行比较。如果两者匹配，那么数据就被视为完整

对于密码学的相关应用，为了防止从哈希值推导出原始密码等逆向工程，哈希算法需要具备更高等级的安全特性：

- **单向性**：无法通过哈希值反推出关于输入数据的任何信息
- **抗碰撞性**：应当极难找到两个不同的输入，使得它们的哈希值相同
- **雪崩效应**：输入的微小变化应当导致输出的显著且不可预测的变化

**均匀分布与抗碰撞性是两个独立的概念**，满足均匀分布不一定满足抗碰撞性。例如在随机输入 `key` 下，哈希函数 `key % 100` 可以产生均匀分布的输出。然而该哈希算法过于简单，所有后两位相等的 `key` 的输出都相同，因此我们可以很容易地从哈希值反推出可用的 `key` ，从而破解密码

- **加法哈希**：对输入的每个字符的 ASCII 码进行相加，将得到的总和作为哈希值
- **乘法哈希**：利用乘法的不相关性，每轮乘以一个常数，将各个字符的 ASCII 码累积到哈希值中
- **异或哈希**：将输入数据的每个元素通过异或操作累积到一个哈希值中
- **旋转哈希**：将每个字符的 ASCII 码累积到一个哈希值中，每次累积之前都会对哈希值进行旋转操作

```CPP
/* 加法哈希 */
int addHash(string key) {
    long long hash = 0;
    const int MODULUS = 1000000007;
    for (unsigned char c : key) {
        hash = (hash + (int)c) % MODULUS;
    }
    return (int)hash;
}

/* 乘法哈希 */
int mulHash(string key) {
    long long hash = 0;
    const int MODULUS = 1000000007;
    for (unsigned char c : key) {
        hash = (31 * hash + (int)c) % MODULUS;
    }
    return (int)hash;
}

/* 异或哈希 */
int xorHash(string key) {
    int hash = 0;
    const int MODULUS = 1000000007;
    for (unsigned char c : key) {
        hash ^= (int)c;
    }
    return hash & MODULUS;
}

/* 旋转哈希 */
int rotHash(string key) {
    long long hash = 0;
    const int MODULUS = 1000000007;
    for (unsigned char c : key) {
        hash = ((hash << 4) ^ (hash >> 28) ^ (int)c) % MODULUS;
    }
    return (int)hash;
}
```

每种哈希算法的最后一步都是对大质数1000000007取模，以确保哈希值在合适的范围内，不使用合数是因为，**使用大质数作为模数，可以最大化地保证哈希值的均匀分布**。因为质数不与其他数字存在公约数，可以减少因取模操作而产生的周期性模式，从而避免哈希冲突

```CPP
nt num = 3;
size_t hashNum = hash<int>()(num);
// 整数 3 的哈希值为 3

bool bol = true;
size_t hashBol = hash<bool>()(bol);
// 布尔量 1 的哈希值为 1

double dec = 3.14159;
size_t hashDec = hash<double>()(dec);
// 小数 3.14159 的哈希值为 4614256650576692846

string str = "Hello 算法";
size_t hashStr = hash<string>()(str);
// 字符串“Hello 算法”的哈希值为 15466937326284535026

// 在 C++ 中，内置 std:hash() 仅提供基本数据类型的哈希值计算
// 数组、对象的哈希值计算需要自行实现
```

**只有不可变对象才可作为哈希表的 `key`** 。假如我们将列表（动态数组）作为 `key` ，当列表的内容发生变化时，它的哈希值也随之改变，我们就无法在哈希表中查询到原先的 `value` 了

虽然自定义对象（比如链表节点）的成员变量是可变的，但它是可哈希的。**这是因为对象的哈希值通常是基于内存地址生成的**，即使对象的内容发生了变化，但它的内存地址不变，哈希值仍然是不变的

***

# 树

二叉树 binary tree是一种非线性数据结构，代表祖先与后代之间的派生关系，体现了一分为二的分治逻辑

与链表类似，二叉树的基本单元是节点，每个节点包含值、左子节点引用和右子节点引用

```CPP
/* 二叉树节点结构体 */
struct TreeNode {
    int val;          // 节点值
    TreeNode *left;   // 左子节点指针
    TreeNode *right;  // 右子节点指针
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

每个节点都有两个引用（指针），分别指向左子节点 left-child node和右子节点right-child node，该节点被称为这两个子节点的父节点parent node

当给定一个二叉树的节点时，将该节点的左子节点及其以下节点形成的树称为该节点的左子树 left subtree，同理可得右子树right subtree

在二叉树中，除叶节点外，其他所有节点都包含子节点和非空子树

![父节点、子节点、子树](https://www.hello-algo.com/chapter_tree/binary_tree.assets/binary_tree_definition.png)

**初始化二叉树**

```CPP
/* 初始化二叉树 */
// 初始化节点
TreeNode* n1 = new TreeNode(1);
TreeNode* n2 = new TreeNode(2);
TreeNode* n3 = new TreeNode(3);
TreeNode* n4 = new TreeNode(4);
TreeNode* n5 = new TreeNode(5);
// 构建节点之间的引用（指针）
n1->left = n2;
n1->right = n3;
n2->left = n4;
n2->right = n5;
```

与链表类似，在二叉树中插入与删除节点可以通过修改指针来实现

```CPP
/* 插入与删除节点 */
TreeNode* P = new TreeNode(0);
// 在 n1 -> n2 中间插入节点 P
n1->left = P;
P->left = n2;
// 删除节点 P
n1->left = n2;
```

插入节点可能会改变二叉树的原有逻辑结构，而删除节点通常意味着删除该节点及其所有子树。因此在二叉树中，插入与删除通常是由一套操作配合完成的，以实现有实际意义的操作

## 二叉树遍历

1. **层序遍历**

   层序遍历level-order traversal从顶部到底部逐层遍历二叉树，并在每一层按照从左到右的顺序访问节点

   层序遍历本质上属于广度优先遍历 breadth-first traversal，也称广度优先搜索breadth-first search, BFS，它体现了一种一圈一圈向外扩展的逐层遍历方式

![二叉树的层序遍历](https://www.hello-algo.com/chapter_tree/binary_tree_traversal.assets/binary_tree_bfs.png)

```CPP
/* 层序遍历 */
vector<int> levelOrder(TreeNode *root) {
    // 初始化队列，加入根节点
    queue<TreeNode *> queue;
    queue.push(root);
    // 初始化一个列表，用于保存遍历序列
    vector<int> vec;
    while (!queue.empty()) {
        TreeNode *node = queue.front();
        queue.pop();              // 队列出队
        vec.push_back(node->val); // 保存节点值
        if (node->left != nullptr)
            queue.push(node->left); // 左子节点入队
        if (node->right != nullptr)
            queue.push(node->right); // 右子节点入队
    }
    return vec;
}
```

前序、中序和后序遍历都属于深度优先遍历depth-first traversal，也称深度优先搜索 depth-first search, DFS，它体现了一种先走到尽头，再回溯继续的遍历方式

![二叉搜索树的前序、中序、后序遍历](https://www.hello-algo.com/chapter_tree/binary_tree_traversal.assets/binary_tree_dfs.png)

```CPP
/* 前序遍历 */
void preOrder(TreeNode *root) {
    if (root == nullptr)
        return;
    // 访问优先级：根节点 -> 左子树 -> 右子树
    vec.push_back(root->val);
    preOrder(root->left);
    preOrder(root->right);
}

/* 中序遍历 */
void inOrder(TreeNode *root) {
    if (root == nullptr)
        return;
    // 访问优先级：左子树 -> 根节点 -> 右子树
    inOrder(root->left);
    vec.push_back(root->val);
    inOrder(root->right);
}

/* 后序遍历 */
void postOrder(TreeNode *root) {
    if (root == nullptr)
        return;
    // 访问优先级：左子树 -> 右子树 -> 根节点
    postOrder(root->left);
    postOrder(root->right);
    vec.push_back(root->val);
}
```

## 二叉树数组表示

二叉树存储单位为node，node之间用pointer连接

给定一棵完美二叉树，将所有节点按照层序遍历的顺序存储在一个数组中，则每个节点都对应唯一的数组索引

![完美二叉树的数组表示](https://www.hello-algo.com/chapter_tree/array_representation_of_tree.assets/array_representation_binary_tree.png)

**映射公式的角色相当于链表中的指针**。给定数组中的任意一个节点，我们都可以通过映射公式来访问它的左（右）子节点



完美二叉树是一个特例，在二叉树的中间层通常存在许多 `None` 。由于层序遍历序列并不包含这些 `None` ，因此我们无法仅凭该序列来推测 `None` 的数量和分布位置。**这意味着存在多种二叉树结构都符合该层序遍历序列**

给定一棵非完美二叉树，上述数组表示方法已经失效

![层序遍历序列对应多种二叉树可能性](https://www.hello-algo.com/chapter_tree/array_representation_of_tree.assets/array_representation_without_empty.png)

**可以考虑在层序遍历序列中显式地写出所有 `None`**

```CPP
/* 二叉树的数组表示 */
//使用int最大值INT_MAX标记空位
vector<int> tree = {1, 2, 3, 4, INT_MAX, 6, 7, 8, 9, INT_MAX, INT_MAX, 12, INT_MAX, INT_MAX, 15};
```

![任意类型二叉树的数组表示](https://www.hello-algo.com/chapter_tree/array_representation_of_tree.assets/array_representation_with_empty.png)

**完全二叉树非常适合使用数组来表示**。因为`None` 只出现在最底层且靠右的位置，因此所有 `None` 一定出现在层序遍历序列的末尾

这意味着使用数组表示完全二叉树时，可以省略存储所有 `None`

![完全二叉树的数组表示](https://www.hello-algo.com/chapter_tree/array_representation_of_tree.assets/array_representation_complete_binary_tree.png)

**基于数组表示的二叉树**

```CPP
/* 数组表示下的二叉树类 */
class ArrayBinaryTree {
  public:
    /* 构造方法 */
    ArrayBinaryTree(vector<int> arr) {
        tree = arr;
    }

    /* 节点数量 */
    int size() {
        return tree.size();
    }

    /* 获取索引为 i 节点的值 */
    int val(int i) {
        // 若索引越界，则返回 INT_MAX ，代表空位
        if (i < 0 || i >= size())
            return INT_MAX;
        return tree[i];
    }

    /* 获取索引为 i 节点的左子节点的索引 */
    int left(int i) {
        return 2 * i + 1;
    }

    /* 获取索引为 i 节点的右子节点的索引 */
    int right(int i) {
        return 2 * i + 2;
    }

    /* 获取索引为 i 节点的父节点的索引 */
    int parent(int i) {
        return (i - 1) / 2;
    }

    /* 层序遍历 */
    vector<int> levelOrder() {
        vector<int> res;
        // 直接遍历数组
        for (int i = 0; i < size(); i++) {
            if (val(i) != INT_MAX)
                res.push_back(val(i));
        }
        return res;
    }

    /* 前序遍历 */
    vector<int> preOrder() {
        vector<int> res;
        dfs(0, "pre", res);
        return res;
    }

    /* 中序遍历 */
    vector<int> inOrder() {
        vector<int> res;
        dfs(0, "in", res);
        return res;
    }

    /* 后序遍历 */
    vector<int> postOrder() {
        vector<int> res;
        dfs(0, "post", res);
        return res;
    }

  private:
    vector<int> tree;

    /* 深度优先遍历 */
    void dfs(int i, string order, vector<int> &res) {
        // 若为空位，则返回
        if (val(i) == INT_MAX)
            return;
        // 前序遍历
        if (order == "pre")
            res.push_back(val(i));
        dfs(left(i), order, res);
        // 中序遍历
        if (order == "in")
            res.push_back(val(i));
        dfs(right(i), order, res);
        // 后序遍历
        if (order == "post")
            res.push_back(val(i));
    }
};
```

二叉树的数组表示主要有以下优点:

- 数组存储在连续的内存空间中，对缓存友好，访问与遍历速度较快
- 不需要存储指针，比较节省空间
- 允许随机访问节点

数组表示也存在一些局限性:

- 数组存储需要连续内存空间，因此不适合存储数据量过大的树
- 增删节点需要通过数组插入与删除操作实现，效率较低
- 当二叉树中存在大量 `None` 时，数组中包含的节点数据比重较低，空间利用率较低

## 二叉搜索树

二叉搜索树binary search tree满足以下条件：

1. 对于根节点，左子树中所有节点的值 < 根节点的值 < 右子树中所有节点的值
2. 任意节点的左、右子树也是二叉搜索树，即同样满足条件 

![二叉搜索树](https://www.hello-algo.com/chapter_tree/binary_search_tree.assets/binary_search_tree.png)

**查找节点**

- 若 `cur.val < num` ，说明目标节点在 `cur` 的右子树中，因此执行 `cur = cur.right` 
- 若 `cur.val > num` ，说明目标节点在 `cur` 的左子树中，因此执行 `cur = cur.left` 
- 若 `cur.val = num` ，说明找到目标节点，跳出循环并返回该节点

```CPP
/* 查找节点 */
TreeNode *search(int num) {
    TreeNode *cur = root;
    // 循环查找，越过叶节点后跳出
    while (cur != nullptr) {
        // 目标节点在 cur 的右子树中
        if (cur->val < num)
            cur = cur->right;
        // 目标节点在 cur 的左子树中
        else if (cur->val > num)
            cur = cur->left;
        // 找到目标节点，跳出循环
        else
            break;
    }
    // 返回目标节点
    return cur;
}
```

**插入节点**

1. **查找插入位置**：与查找操作相似，从根节点出发，根据当前节点值和 `num` 的大小关系循环向下搜索，直到越过叶节点（遍历至 `None` ）时跳出循环。
2. **在该位置插入节点**：初始化节点 `num` ，将该节点置于 `None` 的位置

![在二叉搜索树中插入节点](https://www.hello-algo.com/chapter_tree/binary_search_tree.assets/bst_insert.png)

在代码实现中，需要注意以下两点：

- 二叉搜索树不允许存在重复节点，若待插入节点在树中已存在，则不执行插入，直接返回
- 为了实现插入节点，需要借助节点 `pre` 保存上一轮循环的节点。这样在遍历至 `None` 时，可以获取到其父节点，从而完成节点插入操作

```CPP
/* 插入节点 */
void insert(int num) {
    // 若树为空，则初始化根节点
    if (root == nullptr) {
        root = new TreeNode(num);
        return;
    }
    TreeNode *cur = root, *pre = nullptr;
    // 循环查找，越过叶节点后跳出
    while (cur != nullptr) {
        // 找到重复节点，直接返回
        if (cur->val == num)
            return;
        pre = cur;
        // 插入位置在 cur 的右子树中
        if (cur->val < num)
            cur = cur->right;
        // 插入位置在 cur 的左子树中
        else
            cur = cur->left;
    }
    // 插入节点
    TreeNode *node = new TreeNode(num);
    if (pre->val < num)
        pre->right = node;
    else
        pre->left = node;
}
```

**删除节点**

```CPP
/* 删除节点 */
void remove(int num) {
    // 若树为空，直接提前返回
    if (root == nullptr)
        return;
    TreeNode *cur = root, *pre = nullptr;
    // 循环查找，越过叶节点后跳出
    while (cur != nullptr) {
        // 找到待删除节点，跳出循环
        if (cur->val == num)
            break;
        pre = cur;
        // 待删除节点在 cur 的右子树中
        if (cur->val < num)
            cur = cur->right;
        // 待删除节点在 cur 的左子树中
        else
            cur = cur->left;
    }
    // 若无待删除节点，则直接返回
    if (cur == nullptr)
        return;
    // 子节点数量 = 0 or 1
    if (cur->left == nullptr || cur->right == nullptr) {
        // 当子节点数量 = 0 / 1 时， child = nullptr / 该子节点
        TreeNode *child = cur->left != nullptr ? cur->left : cur->right;
        // 删除节点 cur
        if (cur != root) {
            if (pre->left == cur)
                pre->left = child;
            else
                pre->right = child;
        } else {
            // 若删除节点为根节点，则重新指定根节点
            root = child;
        }
        // 释放内存
        delete cur;
    }
    // 子节点数量 = 2
    else {
        // 获取中序遍历中 cur 的下一个节点
        TreeNode *tmp = cur->right;
        while (tmp->left != nullptr) {
            tmp = tmp->left;
        }
        int tmpVal = tmp->val;
        // 递归删除节点 tmp
        remove(tmp->val);
        // 用 tmp 覆盖 cur
        cur->val = tmpVal;
    }
}
```



## AVL树

AVL 树既是二叉搜索树，也是平衡二叉树，同时满足这两类二叉树的所有性质，因此也被称为平衡二叉搜索树

**节点高度**

AVL 树的相关操作需要获取节点高度

节点高度是指从该节点到它的最远叶节点的距离，即所经过的边的数量

叶节点高度为0，空节点高度为-1

```CPP
/* AVL 树节点类 */
struct TreeNode {
    int val{};          // 节点值
    int height = 0;     // 节点高度
    TreeNode *left{};   // 左子节点
    TreeNode *right{};  // 右子节点
    TreeNode() = default;
    explicit TreeNode(int x) : val(x){}
};
```

```CPP
/* 获取节点高度 */
int height(TreeNode *node) {
    // 空节点高度为 -1 ，叶节点高度为 0
    return node == nullptr ? -1 : node->height;
}

/* 更新节点高度 */
void updateHeight(TreeNode *node) {
    // 节点高度等于最高子树高度 + 1
    node->height = max(height(node->left), height(node->right)) + 1;
}
```



**节点平衡因子**

节点的平衡因子balance factor定义为节点左子树的高度减去右子树的高度，同时规定空节点的平衡因子为0

```CPP
/* 获取平衡因子 */
int balanceFactor(TreeNode *node) {
    // 空节点平衡因子为 0
    if (node == nullptr)
        return 0;
    // 节点平衡因子 = 左子树高度 - 右子树高度
    return height(node->left) - height(node->right);
}
```

**AVL树旋转**

AVL树特点在于旋转操作，它能够在不影响二叉树的中序遍历序列的前提下，使失衡节点重新恢复平衡。换句话说，旋转操作既能保持二叉搜索树的性质，也能使树重新变为平衡二叉树

```CPP
/* 右旋操作 */
TreeNode *rightRotate(TreeNode *node) {
    TreeNode *child = node->left;
    TreeNode *grandChild = child->right;
    // 以 child 为原点，将 node 向右旋转
    child->right = node;
    node->left = grandChild;
    // 更新节点高度
    updateHeight(node);
    updateHeight(child);
    // 返回旋转后子树的根节点
    return child;
}
```

![有 grand_child 的右旋操作](https://www.hello-algo.com/chapter_tree/avl_tree.assets/avltree_right_rotate_with_grandchild.png)

```CPP
/* 左旋操作 */
TreeNode *leftRotate(TreeNode *node) {
    TreeNode *child = node->right;
    TreeNode *grandChild = child->left;
    // 以 child 为原点，将 node 向左旋转
    child->left = node;
    node->right = grandChild;
    // 更新节点高度
    updateHeight(node);
    updateHeight(child);
    // 返回旋转后子树的根节点
    return child;
}
```

![有 grand_child 的左旋操作](https://www.hello-algo.com/chapter_tree/avl_tree.assets/avltree_left_rotate_with_grandchild.png)

**先左旋后右旋**

![先左旋后右旋](https://www.hello-algo.com/chapter_tree/avl_tree.assets/avltree_left_right_rotate.png)

**先右旋后左旋**

![先右旋后左旋](https://www.hello-algo.com/chapter_tree/avl_tree.assets/avltree_right_left_rotate.png)

```CPP
/* 执行旋转操作，使该子树重新恢复平衡 */
TreeNode *rotate(TreeNode *node) {
    // 获取节点 node 的平衡因子
    int _balanceFactor = balanceFactor(node);
    // 左偏树
    if (_balanceFactor > 1) {
        if (balanceFactor(node->left) >= 0) {
            // 右旋
            return rightRotate(node);
        } else {
            // 先左旋后右旋
            node->left = leftRotate(node->left);
            return rightRotate(node);
        }
    }
    // 右偏树
    if (_balanceFactor < -1) {
        if (balanceFactor(node->right) <= 0) {
            // 左旋
            return leftRotate(node);
        } else {
            // 先右旋后左旋
            node->right = rightRotate(node->right);
            return leftRotate(node);
        }
    }
    // 平衡树，无须旋转，直接返回
    return node;
}

/* 执行旋转操作，使该子树重新恢复平衡 */
TreeNode *rotate(TreeNode *node) {
    // 获取节点 node 的平衡因子
    int _balanceFactor = balanceFactor(node);
    // 左偏树
    if (_balanceFactor > 1) {
        if (balanceFactor(node->left) >= 0) {
            // 右旋
            return rightRotate(node);
        } else {
            // 先左旋后右旋
            node->left = leftRotate(node->left);
            return rightRotate(node);
        }
    }
    // 右偏树
    if (_balanceFactor < -1) {
        if (balanceFactor(node->right) <= 0) {
            // 左旋
            return leftRotate(node);
        } else {
            // 先右旋后左旋
            node->right = rightRotate(node->right);
            return leftRotate(node);
        }
    }
    // 平衡树，无须旋转，直接返回
    return node;
}
```

**插入节点**

