# C++ STL 学习

## 介绍

### 1.1 STL（Standard Template Library）基本概念

C++ 标准模板库的核心包括以下三个组件（广义上）：

| 组件                | 描述                                                         |
| :------------------ | :----------------------------------------------------------- |
| 容器（Containers）  | 容器是用来管理某一类对象的集合。C++ 提供了各种不同类型的容器，比如 deque、list、vector、map 等。 |
| 算法（Algorithms）  | 算法作用于容器。它们提供了执行各种操作的方式，包括对容器内容执行初始化、排序、搜索和转换等操作。 |
| 迭代器（iterators） | 迭代器用于遍历对象集合的元素。这些集合可能是容器，也可能是容器的子集。 |

这三个组件都带有丰富的预定义函数，帮助我们通过简单的方式处理复杂的任务。

容器和算法之间通过迭代器进行无缝衔接

**STL包含什么？**

**容器类模板**

容器用于存储**序列化的**数据，如：向量、队列、栈、集合等。 

**算法（函数）模板**

算法用于对容器中的数据元素进行一些常用**操作**，如：排序、统计等。

**迭代器类模板**

- 迭代器实现了**抽象的指针**功能，它们指向容器中的数据元素，用于对容器中的数据元素进行遍历和访问。
- **迭代器是容器和算法之间的桥梁**：传给算法的不是容器，而是指向容器中元素的迭代器，算法通过迭代器实现对容器中数据元素的访问。这样使得算法与容器保持独立，从而提高算法的通用性。

<img src="https://3445412966-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F4jcp9JrFSUV0Enu5fcXK%2Fuploads%2F4yIi4UTdNW9C2vdGDA0D%2F%E6%88%AA%E5%B1%8F2022-02-08%2020.57.44.png?alt=media&token=39377522-2a40-4233-8abd-296ba9c79229" alt="img" style="zoom: 25%;" />

​												STL的基本逻辑

**基于STL编程的例子**

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <numeric>
using namespace std;

void display(int x) { cout << ' ' << x; return; }
int main()
{
    vector<int> v; //创建容器对象v，元素类型为int
    int x;
    cin >> x;
    while (x > 0) //生成容器v中的元素
    {
        v.push_back(x); //往容器v中增加一个元素
        cin >> x;
    }
    //利用算法max_element计算并输出容器v中的最大元素
    cout << "Max = " << *max_element(v.begin(),v.end()) << endl;
    //利用算法accumulate计算并输出容器v中所有元素的和
    cout << "Sum = " << accumulate(v.begin(),v.end(),0) << endl;
    //利用算法sort对容器v中的元素进行排序
    sort(v.begin(),v.end()); 
    //利用算法for_each输出排序结果
    cout << "Sorted result is:\n";
    for_each(v.begin(),v.end(),display);
    cout << '\n';
    return 0;
}
```

### 1.2 STL 六大组件简介

STL提供了六大组件，彼此之间可以组合使用，分别是容器，算法，迭代器，仿函数，适配器，空间配置器。

- 容器（Container）：STL容器为各种数据结构，如vector、stack、queue、map、set等，用来存放数据，从实现角度来看，STL容器是一种class template。

- 算法（Algorithm）：STL的算法多数定义在**<algorithm>**头文件中，其中包括了各种常用的算法，如sort、find、copy、reverse等，从实现角度来看，STL算法是一种function template。
- 迭代器（Iterator）：STL迭代器扮演了容器与算法之间的胶合剂，共有五种类型，从实现角度来看，迭代器是一种将opetator*、opetator->、operator++等指针相关操作予以重载的class template。所有STL容器都附带有自己专属的迭代器，只有容器的设计者才知道如何遍历自己的元素。
- 仿函数（Functor）：行为类似函数，可作为算法的某种策略，从实现角度来看，仿函数是一种重载了operator()的class或者class template。
- 适配器（Adaptor）：一种用来修饰容器或仿函数或迭代器接口的东西。
- 空间配置器（Allocator）：负责空间的配置与管理。从实现角度来看，配置器是一个实现了动态空间配置、空间管理、空间释放的class template。

六大组件的交互关系，容器通过空间配置器取得数据存储空间，算法通过迭代器存储容器中的内容，仿函数可以协助算法完成不同的策略的变化，适配器可以修饰仿函数。

### 1.3 优点

- STL是c++的一部分，内建在编译器中。
- STL的一个重要特点就是数据结构和算法的分离
- 程序员不需要思考STL具体的实现过程，是要能熟练运用
- STL具有高可用性，高性能，高移植性，跨平台等优点

## 2. STL三大组件

三大组件的关系：容器存储数据，并且提供迭代器，算法使用迭代器来操作容器中的元素

### 2.1 STL容器

常用的数据结构如，数组(array)、链表（list）、栈（stack）、队列(queue)、映射表(map)、集合(set)等，根据数据在容器中的排列特性，这些数据分为**序列式容器**和**关联式容器**两种。

- 序列式容器就是容器元素在容器中的位置是由元素进入容器的时间和位置来决定。

  如：Vector容器、Deque容器、List容器、Stack容器、Queue容器。

- 关联式容器就是指容器已经有了一定规则，容器元素在在容器中的位置由规则来决定。

  Set/multiset 容器、Map/mutimap容器

### （1）vector容器

vector又称变长数组，定义在<vector>头文件中，vector容器是动态空间，随着元素的加入，它的内部机制会自动扩充空间以容纳新的元素。因此vector的运用对于内存的合理利用与运用的灵活性有很大的帮助。

**数据结构：**连续存储空间

**迭代器：**随机迭代器

**vector容器动态增长原理：**

1. 当存储空间不够时，会开辟另一块大的空间，然后把数据拷贝进去，最后再销毁原来的空间
2. 申请的空间会比用户需求大一点
3. 重新分配空间会使原来的迭代器失效



**常用api：**

**注意：**

- **vector的定义方式：**

```c++
//构造
vector<T> v;//采用模板实现类实现，默认构造函数
vector(v.begin(), v.end();//将v[begin(),end()]区间中的元素拷贝给本身。
vector(n.elem);//构造函数将n个elem拷贝给本身。
vector(const vector &vec);//拷贝构造函数
vector<int> v;  // 定义一个vector，其中的元素为int类型
vector<int> v[N];  // 定义一个vector数组，其中有N个vector
vector<int> v(len);  // 定义一个长度为len的vector
vector<int> v(len, x);  // 定义一个长度为len的vector，初始化每个元素为x
//常用赋值操作     
vector<int> v2(v1);  // 用v1给v2赋值，v1的类型为vector
vector<int> v2(v1.begin(), v1.begin() + 3);  // 将v1中第0~2三个元素赋值给v2

```

- **vector的常用内置函数：**

```c++
// vector中的常用内置函数
vector<int> v = { 1, 2, 3 };  // 初始化vector，v:{1, 2, 3}
vector<int>::iterator it = v.begin();  // 定义vector的迭代器，指向begin()

v.push_back(4);  // 在vector的尾部插入元素4，v:{1, 2, 3, 4}
v.pop_back();  // 删除vector的最后一个元素，v:{1, 2, 3}
// 注意使用lower_bound()与upper_bound()函数时vector必须是有序的，upper_bound()在<algorithm>中
lower_bound(v.begin(), v.end(), 2);  // 返回第一个大于等于2的元素的迭代器v.begin() + 1，若不存在则返回v.end()
upper_bound(v.begin(), v.end(), 2);  // 返回第一个大于2的元素的迭代器v.begin() + 2，若不存在则返回v.end()
v.size();  // 返回vector中元素的个数
v.empty();  // 返回vector是否为空，若为空则返回true否则返回false
v.front();  // 返回vector中的第一个元素
v.back();  // 返回vector中的最后一个元素
v.begin();  // 返回vector第一个元素的迭代器
v.end();  // 返回vector最后一个元素后一个位置的迭代器
v.clear();  // 清空vector
v.erase(v.begin());  // 删除迭代器it所指向的元素，即删除第一个元素
v.erase(v.begin(), v.begin() + 2);  // 删除区间[v.begin(), v.begin() + 2)的所有元素
v.insert(v.begin(), 1);  // 在迭代器it所指向的位置前插入元素1，返回插入元素的迭代器

// 根据下标进行遍历
for (int i = 0; i < v.size(); i++)
    cout << v[i] << ' ';
// 使用迭代器遍历
for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
    cout << *it << ' ';
// for_each遍历(C++11)
for (auto x : v)
    cout << x << ' ';3.2 stack
```

#### (2) stack的定义方式：

stack又称栈，是一种后进先出（Last In First Out，LIFO）的数据结构，定义在<stack>头文件中，stack容器允许新增元素、移除元素、取得栈顶元素，但是除了最顶端以外，没有任何方法可以存取stack的其它元素，换言之，stack不允许有遍历行为。

- **stack的定义方式：**

- ```c++
  stack<int> stk;  // 定义一个stack，其中元素的类型为int
  stack<int> stk[N];  // 定义一个stack数组，其中有N个stack
  ```

- **stack的常用内置函数：**

  ```c++
  // stack中的常用内置函数
  stack<int> stk;
  stk.push(x);  // 在stack中插入元素x
  stk.pop();  // 弹出stack的栈顶元素
  stk.top();  // 返回stack的栈顶元素
  stk.size();  // 返回stack中元素的个数
  stk.empty();  // 返回stack是否为空，若为空则返回true否则返回false
  ```


#### (3) string 容器

string又称字符串，定**义在<string>头文件中**。C风格的字符串（以空字符结尾的字符数组）太过复杂难于掌握，因此C++标准库定义了一种string类。string和vector<char>在数据结构、内存管理等方面都是相同的。但是，vector<char>只是单纯的一个“char元素的容器”，而string不仅是一个“char元素的容器”，它还扩展了一些针对字符串的操作，例如string可以使用c_str()函数转换为C风格的字符串，vector中并未对输入输出流操作符进行重载，因此无法直接对vector<char>进行cin或者cout这样的操作，但是string可以，且vector<char>并不能直接实现字符串的拼接，但是string可以，string中重载了+, +=运算符。

**String和c风格字符串对比：**

- Char*是一个指针，String是一个类

  string封装了char* ，管理这个字符串，是一个char*型的容器。

- String封装了很多实用的成员方法

  find、copy、delete、replace、insert

- 不考虑内存释放和越界

  string管理char*分配的内存，每一次string的复制，取值都由string类负责维护，不必担心复制越界和取值越界。

**常用api：**构造、基本赋值、存取字符、拼接、查找和替换、比较、子串、插入和删除、string和const char*的转换

- 注意：

- ##### (1) **string的定义方式：**

  ```c++
  string str;  // 定义一个空的字符串
  string str[N];  // 定义一个string数组，其中有N个string
  string str(5, 'a');  // 使用5个字符'a'初始化
  string str("abc");  // 使用字符串初始化
  /*
  * 构造函数
  * string();  // 定义一个空的字符串
  string(conststring& str);  // 使用一个string对象初始化另一个string对象
  string(constchar* s);  // 使用字符串s初始化
  string(int n, char c);  // 使用n个字符c字符串初始化
  */
  ```

- ##### **(2) 基本赋值操作**

  ```c++
  string&operator=(constchar * s);//char*类型字符串，赋值给当前的字符串
  string&operator=(conststring&s);//把字符串s赋给当前的字符串==string& assign=(conststring&s);
  string&operator=(char c);//字符赋值给当前的字符串
  string& assign=(constchar *s);//字符串s赋值给当前的字符串
  string& assign=(constchar *s，int n);//字符串s的前n个字符赋值给当前的字符串
  string& assign=(int n, char c)//用n个字符c赋值给当前字符串
  string& assign=(conststring&s,int start,int n);//将s从start开始n个字符赋值给字符串
  ```

  **例：**

  ```c++
  void test02()
  {
  	string s1;
  	s1 = "hello";
  	cout << s1 << endl;
  
  	string s2;
  	s2.assign("world");
  	cout << s2 << endl;
  
  }
  ```

- ##### **(3) 存取字符操作**

  ```c++
  char&operator[](int n);//通过[]方式取字符
  char& at(int n);//通过at方式获取字符
  ```

  - **[  ] 和at的区别：**

    **[]**访问元素时，越界不抛出异常，直接挂，

    **a**t越界，会抛出异常

    ```c++
    try
    	{
    		//cout << s[100] << endl;
    		cout << s.at(100) << endl;
    	}
    	catch (out_of_range& ex)
    	{
    		cout << ex.what() << endl;
    	}
    ```

    输出：

    ```powershell
    invalid string position(4)
    ```

- **(4) string拼接操作**

- ```c++
  tring& operator+=(const string& str);//重载+=操作符 
  string& operator+=(const char* str);//重载+=操作符 
  string& operator+=(const char c);//重载+=操作符 
  string& append(const char *s);//把字符串s连接到当前字符串结尾 
  string& append(const char *s, int n);//把字符串s的前n个字符连接到当前字符串结尾 
  string& append(const string &s);//同operator+=() 
  string& append(const string &s, int pos, int n);//把字符串s中从pos开始的n个字符连接到 当前字符串结尾 
  string& append(int n, char c);//在当前字符串结尾添加n个字符c
  ```

- **(5) string查找和替换**

- ```c++
  int find(const string& str, int pos = 0) const; //查找str第一次出现位置,从pos开始查找 
  int find(const char* s, int pos = 0) const; //查找s第一次出现位置,从pos开始查找 
  int find(const char* s, int pos, int n) const; //从pos位置查找s的前n个字符第一次位置 
  int find(const char c, int pos = 0) const; //查找字符c第一次出现位置 
  int rfind(const string& str, int pos = npos) const;//查找str最后一次位置,从pos开始查找
  int rfind(const char* s, int pos = npos) const;//查找s最后一次出现位置,从pos开始查找 
  int rfind(const char* s, int pos, int n) const;//从pos查找s的前n个字符最后一次位置 
  int rfind(const char c, int pos = 0) const; //查找字符c最后一次出现位置 
  string& replace(int pos, int n, const string& str); //替换从pos开始n个字符为字符串 
  str string& replace(int pos, int n, const char* s); //替换从pos开始的n个字符为字符串s
  ```

- **(6) string比较操作**

  ```c++
  *
  compare函数在>时返回 1，<时返回 -1，==时返回 0。 
  比较区分大小写，比较时参考字典顺序，排越前面的越小。 
  大写的A比小写的a小。 
  */
  int compare(const string &s) const;//与字符串s比较 
  int compare(const char *s) const;//与字符串s比较
  ```

- **(7) string子串**

- ```c++
  string substr(int pos = 0, int n = npos) const;//返回由pos开始的n个字符组成的字符串 
  ```

- **(8) string插入和删除操作**

- ```c++
  string& insert(int pos, const char* s); //插入字符串 
  string& insert(int pos, const string& str); //插入字符串 
  string& insert(int pos, int n, char c);//在指定位置插入n个字符c 
  string& erase(int pos, int n = npos);//删除从Pos开始的n个字符
  ```

- **(9) string和c-style字符串转换**

- ```c++
  //string 转 char* 
  string str = "itcast"; 
  const char* cstr = str.c_str(); 
  //char* 转 string char* s = "itcast"; 
  string str(s);
  ```

  在c++中存在一个从const char到*string*的隐式类型转换，却不存在从一个*string*对象到*C_string*的自动类 型转换。对于*string*类型的字符串，可以通过*c_str()*函数返回*string*对象对应的*C_string.* 通常，程序员在整个程序中应坚持使用*string*类对象，直到必须将内容转化为*char*时才将其转换为C_string.

  提示: 为了修改string字符串的内容，下标操作符[]和at都会返回字符的引用。但当字符串的内存被重新分配之后，可能发生错误.

- ##### **(10) string的常用内置函数：**

  ```c++
  // string中的常用内置函数
  string str("abcabc");
  str.push_back('d');  // 在string尾部插入字符，"abcabcd"
  str.pop_back();  // 删除string尾部的字符，"abcabc"
  str.length();  // 返回string中字符的个数
  str.size();  // 作用与length()相同
  str.empty();  // 返回string是否为空，若为空返回true否则返回false
  str.substr(1);  // 返回string中从下标为1开始至末尾的子串，"bcabc"
  str.substr(0, 2);  // 返回string中从下标为0开始长度为2的子串，"ab"
  str.insert(1, 2, 'x');  // 在下标为1的字符前插入2个字符'x'，"axxbcabc"
  str.insert(1, "yy");  // 在下标为1的字符前插入字符串"yy"，"ayyxxbcabc"
  str.erase(1, 4);  // 删除从位置1开始的4个字符，"abcabc"
  str.find('b');  // 返回字符'b'在string中第一次出现的位置，返回1，若不存在则返回-1
  str.find('b', 2);  // 返回从位置2开始字符'b'在string中第一次出现的位置，返回4
  str.find("bc");  // 同上，返回字符串第一次出现的位置，返回1，若不存在则返回-1
  str.find("bc", 2);  // 返回4
  str.rfind('b');  // 反向查找，原理同上，返回4，若不存在则返回-1
  str.rfind('b', 3);  // 返回1
  str.rfind("bc");  // 返回4，若不存在则返回-1
  str.rfind("bc", 3);  // 返回1
  stoi(str);  // 返回str的整数形式
  to_string(value);  // 返回value的字符串形式，value为整型、浮点型等
  str[0];  // 用下标访问string中的字符
  cout << (str == str) << endl;  // string可比较大小，按字典序
  ```

-  **(11) string中erase()与remove()的用法**

  ```c++
  string str1, str2, str3, str4, str5;
  str1 = str2 = str3 = str4 = str5 = "I love AcWing! It's very funny!";
  str1.erase(15);  // 删除[15,end())的所有元素，"I love AcWing!"
  str2.erase(6, 11);  // 从第6个元素(包括)开始往后删除11个元素，"I love's very funny!"
  str3.erase(str3.begin() + 2);  // 删除迭代器所指的元素，"I ove AcWing! It's very funny!"
  str4.erase(str4.begin() + 7, str4.end() - 11);  // 删除[str4.begin()+7,str4.end()-11)的所有元素，"I love very funny!"
  str5.erase(remove(str5.begin(), str5.end(), 'n'), str5.end());  // 删除[str5.begin(),str5.end())中所有字符'n'，"I love AcWig! It's very fuy!"
  ```


#### (4) `priority_queue`：优先队列

queue又称队列，是一种先进先出（First In First Out，FIFO）的数据结构，定义在<queue>头文件中，queue容器允许从一端（称为队尾）新增元素（入队），从另一端（称为队头）移除元素（出队）。

priority_queue又称优先队列，同样定义在<queue>头文件中，与queue不同的地方在于我们可以自定义其中数据的优先级，优先级高的排在队列前面，优先出队。priority_queue具有queue的所有特性，包括基本操作，只是在这基础上添加了内部的一个排序，它的本质是用堆实现的，因此可分为小根堆与大根堆，小根堆中较小的元素排在前面，大根堆中较大的元素排在前面。（创建priority_queue时默认是大根堆！）

- **queue/priority_queue的定义方式：**

```c++
queue<int> que;  // 定义一个queue，其中元素的类型为int
queue<int> que[N];  // 定义一个queue数组，其中有N个queue
priority_queue<int> bigHeap;  // 定义一个大根堆
priority_queue<int, vector<int>, greater<int> > smallHeap;  // 定义一个小根堆
```

- **queue/priority_queue的常用内置函数：**

```c++
// queue/priority_queue中的常用内置函数
queue<int> que;
priority_queue<int> bigHeap;
que.push(x);  // 在queue的队尾插入元素x
que.pop();  // 出队queue的队头元素
que.front();  // 返回queue的队头元素
que.back();  // 返回queue的队尾元素
que.size();  // 返回queue中元素的个数
que.empty();  // 返回queue是否为空，若为空则返回true否则返回false
bigHeap.top();  // 返回priority_queue的队头元素
```

#### (5) deque：双端队列

deque又称双端队列，定义在<deque>头文件中，vector容器是单向开口的连续内存空间，deque则是一种双向开口的连续线性空间。所谓的双向开口，意思是可以在头尾两端分别做元素的插入和删除操作，当然，vector也可以在头尾两端插入元素，但是在其头部进行插入操作效率很低。deque和vector最大的差异一是在于deque允许使用常数项时间在头部进行元素的插入和删除操作，二是在于deque没有容量的概念，因为它是动态的以分段连续空间组合而成，随时可以增加一段新的空间并链接起来。

- **deque的定义方式：**

```
deque<int> deq;  // 定义一个deque，其中的元素为int类型
deque<int> deq[N];  // 定义一个deque数组，其中有N个deque
deque<int> deq(len);  // 定义一个长度为len的deque
deque<int> deq(len, x);  // 定义一个长度为len的deque，初始化每个元素为x
deque<int> deq2(deq1);  // 用deq1给v2赋值，deq2的类型为deque
deque<int> deq2(deq1.begin(), deq1.begin() + 3);  // 将deq1中第0~2三个元素赋值给deq2
```

- **deque的常用内置函数：**

  ```c++
  //deque中的常用内置函数
  deque<int> deq = { 1, 2, 3 };  // 初始化vector，v:{1, 2, 3}
  deque<int>::iterator it = deq.begin();  // 定义vector的迭代器，指向begin()
  deq.push_back(4);  // 在deque的尾部插入元素4，v:{1, 2, 3, 4}
  deq.pop_back();  // 删除deque的尾部元素，v:{1, 2, 3}
  deq.push_front(4);  // 在deque的头部插入元素4，v:{4, 1, 2, 3}
  deq.pop_front();  // 删除deque的头部元素，v:{1, 2, 3}
  deq.size();  // 返回deque中元素的个数
  deq.empty();  // 返回deque是否为空，若为空则返回true否则返回false
  deq.front();  // 返回deque中的第一个元素
  deq.back();  // 返回deque中的最后一个元素
  deq.begin();  // 返回deque第一个元素的迭代器
  deq.end();  // 返回deque最后一个元素后一个位置的迭代器
  deq.clear();  // 清空deque
  deq.erase(deq.begin());  // 删除迭代器it所指向的元素，即删除第一个元素
  deq.erase(deq.begin(), deq.begin() + 2);  // 删除区间[v.begin(), v.begin() + 2)的所有元素
  deq.insert(deq.begin(), 1);  // 在迭代器it所指向的位置前插入元素1，返回插入元素的迭代器
  
  // 根据下标进行遍历
  for (int i = 0; i < deq.size(); i++)
      cout << deq[i] << ' ';
  // 使用迭代器遍历
  for (deque<int>::iterator it = deq.begin(); it != deq.end(); it++)
      cout << *it << ' ';
  // for_each遍历(C++11)
  for (auto x : deq)
      cout << x << ' ';
  ```

#### (6) map/multimap：地图

map/multimap又称映射，定义在<map>头文件中，map和multimap的底层实现机制都是红黑树。map的功能是能够将任意类型的元素映射到另一个任意类型的元素上，并且所有的元素都会根据元素的键值自动排序。map所有的元素都是pair，同时拥有键值和实值（即(key, value)对），key被视为键值，value被视为实值，map不允许两个元素有相同的键值。multimap和map的操作类似，唯一区别是multimap的键值允许重复。

- **map/multimap的定义方式**：

```c++
map<string, int> mp;  // 定义一个将string映射成int的map
map<string, int> mp[N];  // 定义一个map数组，其中有N个map
multimap<string, int> mulmp;  // 定义一个将string映射成int的multimap
multimap<string, int> mulmp[N];  // 定义一个multimap数组，其中有N个multimap
```

- **map/multimap的常用内置函数：**

```c++
// map/multimap中的常用内置函数
map<string, int> mp;
mp["abc"] = 3;  // 将"abc"映射到3
mp["ab"]++;  // 将"ab"所映射的整数++
mp.insert(make_pair("cd", 2));  // 插入元素
mp.insert({ "ef", 5 });  // 同上
mp.size();  // 返回map中元素的个数
mp.empty();  // 返回map是否为空，若为空返回true否则返回false
mp.clear();  // 清空map
mp.erase("ef");  // 清除元素{"ef", 5}
mp["abc"];  // 返回"abc"映射的值
mp.begin();  // 返回map第一个元素的迭代器
mp.end();  // 返回map最后一个元素后一个位置的迭代器
mp.find("ab");  // 返回第一个键值为"ab"的迭代器，若不存在则返回mp.end()
mp.find({ "abc", 3 });  // 返回元素{"abc", 3}的迭代器，若不存在则返回mp.end()
mp.count("abc");  // 返回第一个键值为"abc"的元素数量1，由于map元素不能重复因此count返回值只有0或1
mp.count({ "abc", 2 });  // 返回第一个键值为"abc"的元素数量1，注意和find不一样，count只判断第一个键值
mp.lower_bound("abc");  // 返回第一个键值大于等于"abc"的元素的迭代器，{"abc", 3}
mp.upper_bound("abc");  // 返回第一个键值大于"abc"的元素的迭代器，{"cd", 2}

// 使用迭代器遍历
for (map<string, int>::iterator it = mp.begin(); it != mp.end(); it++)
    cout << (*it).first << ' ' << (*it).second << endl;
// for_each遍历(C++11)
for (auto x : mp)
    cout << x.first << ' ' << x.second << endl;
// 扩展推断范围的for_each遍历(C++17)
for (auto &[k, v] : mp)
    cout << k << ' ' << v << endl;
```

#### (7) set/multiset：集合

set/multiset又称集合，定义在<set>头文件中。set的特性是所有元素都会根据元素的键值自动被排序，set的元素不像map那样可以同时拥有键值和实值，set的元素既是键值又是实值，set不允许两个元素有相同的键值，因此总结来说就是set中的元素是有序且不重复的。multiset的特性和用法和set完全相同，唯一的区别在于multiset允许有重复元素，set和multiset的底层实现都是红黑树。

- **set/multiset的定义方式：**

```c++
set<int> st;  // 定义一个set，其中的元素类型为int
set<int> st[N];  // 定义一个set数组，其中有N个set
multiset<int> mulst;  // 定义一个multiset
multiset<int> mulst[N];  // 定义一个multiset数组，其中有N个multiset
```

- **set/multiset的常用内置函数：**

```c++
// set/multiset中的常用内置函数
set<int> st;
st.insert(5);  // 插入元素5
st.insert(6);  // 同上
st.insert(7);  // 同上
st.size();  // 返回set中元素的个数
st.empty();  // 返回set是否为空，若为空返回true否则返回false
st.erase(6);  // 清除元素6
st.begin();  // 返回set第一个元素的迭代器
st.end();  // 返回set最后一个元素后一个位置的迭代器
st.clear();  // 清空set
st.find(5);  // 返回元素5的迭代器，若不存在则返回st.end()
st.count(5);  // 返回元素5的个数1，由于set元素不会重复，因此count返回值只有0或1
st.lower_bound(5);  // 返回第一个键值大于等于5的元素的迭代器，返回元素5的迭代器
st.upper_bound(5);  // 返回第一个键值大于5的元素的迭代器，返回元素7的迭代器

// 使用迭代器遍历
for (set<int>::iterator it = st.begin(); it != st.end(); it++)
    cout << (*it) << ' ';
// for_each遍历(C++11)
for (auto x : st)
    cout << x << ' ';
```

#### (8) unordered_map/unordered_set

unordered_map/unordered_set分别定义在<unordered_map>与<unordered_set>头文件中，内部采用的是hash表结构，拥有快速检索的功能。与map/set相比最大的区别在于unordered_map/unordered_set中的元素是无序的，增删改查的时间复杂度为O(1)（map/set增删改查的时间复杂度为O(logn)，但是不支持lower_bound()/upper_bound()函数。

- **unordered_map/unordered_set的定义方式：**

- ```c++
  unordered_set<int> st;  // 定义一个unordered_set，其中的元素类型为int
  unordered_set<int> st[N];  // 定义一个unordered_set数组，其中有N个unordered_set
  unordered_map<int, int> mp;  // 定义一个unordered_map
  unordered_map<int, int> mp[N];  // 定义一个unordered_map数组，其中有N个unordered_map
  ```

- **unordered_map/unordered_set的常用内置函数：**

```c++
// unordered_map/unordered_set中的常用内置函数
unordered_set<int> st;
unordered_map<int, int> mp;
st.insert(5);  // 插入元素5
st.insert(6);  // 同上
st.insert(7);  // 同上
st.size();  // 返回unordered_set中元素的个数
st.empty();  // 返回unordered_set是否为空，若为空返回true否则返回false
st.erase(6);  // 清除元素6
st.find(5);  // 返回元素5的迭代器，若不存在则返回st.end()
st.count(5);  // 返回元素5的个数，由于unordered_set元素不会重复，因此count返回值只有0或1
st.begin();  // 返回unordered_set第一个元素的迭代器
st.end();  // 返回unordered_set最后一个元素后一个位置的迭代器
st.clear();  // 清空unordered_set
mp.insert(make_pair(1, 2));  // 插入元素{1, 2}
mp.insert({ 3, 4 });  // 同上
mp.size();  // 返回unordered_map中元素的个数
mp.empty();  // 返回unordered_map是否为空，若为空返回true否则返回false
mp.erase(3);  // 清除元素{3, 4}
mp.find(1);  // 返回第一个键值为1的迭代器，若不存在则返回mp.end()
mp.count(1);  // 返回第一个键值为1的元素数量，由于unordered_map元素不能重复因此count返回值只有0或1
mp.begin();  // 返回unordered_map第一个元素的迭代器
mp.end();  // 返回unordered_map最后一个元素后一个位置的迭代器
mp.clear();  // 清空unordered_map

// 使用迭代器遍历
for (unordered_set<int>::iterator it = st.begin(); it != st.end(); it++)
    cout << (*it) << ' ';
// for_each遍历(C++11)
for (auto x : st)
    cout << x << ' ';

// 使用迭代器遍历
for (unordered_map<int, int>::iterator it = mp.begin(); it != mp.end(); it++)
    cout << (*it).first << ' ' << (*it).second << endl;
// for_each遍历(C++11)
for (auto x : mp)
    cout << x.first << ' ' << x.second << endl;
// 扩展推断范围的for_each遍历(C++17)
for (auto &[k, v] : mp)
    cout << k << ' ' << v << endl;
```

### （9）list容器

#### （9.1）list容器基本概念

链表是一种物理存储单元上非连续，非顺序的存储结构，数据元素的逻辑顺序是通过链表中的指针链接次序实现的。链表由一系列结点组成（链表中的每个元素称为结点），结点可以在运行时动态生成。每个结点包括两个部分：

- 一是存储数据元素的数据域
- 二是存储下一结点地址的指针域

相较于vector的连续线性空间，list较为复杂，好处是每次插入或者删除一个元素，就是配置或者释放一个元素的空间。list对于空间的运用非常精准。

list容器又称**双向链表容器**，即该容器的底层是以双向链表的形式实现的。这意味着，list 容器中的元素可以分散存储在内存空间里，而不是必须存储在一整块连续的内存空间中。

<font color=red>List有一个重要特性，掺入删除操作都不会造成原有list迭代器的失效。</font>

图 1 展示了 list 双向链表容器是如何存储元素的。

![](http://c.biancheng.net/uploads/allimg/180912/2-1P912134314345.jpg)

可以看到，list 容器中各个元素的前后顺序是靠[指针](http://c.biancheng.net/c/80/)来维系的，每个元素都配备了 2 个指针，分别指向它的前一个元素和后一个元素。其中第一个元素的前向指针总为 null，因为它前面没有元素；同样，尾部元素的后向指针也总为 null。

基于这样的存储结构，list 容器具有一些其它容器（array、vector 和 deque）所不具备的优势，即它可以在序列已知的任何位置快速插入或删除元素（时间复杂度为`O(1)`）。并且在 list 容器中移动元素，也比其它容器的效率高。

使用 list 容器的缺点是，它不能像 array 和 vector 那样，通过位置直接访问元素。举个例子，如果要访问 list 容器中的第 6 个元素，它不支持`容器对象名[6]`这种语法格式，正确的做法是从容器中第一个元素或最后一个元素开始遍历容器，直到找到该位置。

> 实际场景中，如何需要对序列进行大量添加或删除元素的操作，而直接访问元素的需求却很少，这种情况建议使用 list 容器存储序列。

list 容器以模板类 list<T>（T 为存储元素的类型）的形式在`<list>`头文件中，并位于 std 命名空间中。因此，在使用该容器之前，代码中需要包含下面两行代码：

```c++
#include <list>
using namespace std;
```

#### list容器的创建

根据不同的使用场景，有以下 5 种创建 list 容器的方式供选择。

**(1) 创建一个没有任何元素的空 list 容器：**

```c++
std::list<int> values;
```

和空 array 容器不同，空的 list 容器在创建之后仍可以添加元素，因此创建 list 容器的方式很常用。

**(2) 创建一个包含 n 个元素的 list 容器：**

```c++
std::list<int> values(10);
```

通过此方式创建 values 容器，其中包含 10 个元素，每个元素的值都为相应类型的默认值（int类型的默认值为 0）。

**(3) 创建一个包含 n 个元素的 list 容器，并为每个元素指定初始值。例如：**

```c++
std::list<int> values(10, 5);
```

如此就创建了一个包含 10 个元素并且值都为 5 个 values 容器。

**(4) 在已有 list 容器的情况下，通过拷贝该容器可以创建新的 list 容器。例如：**

```c++
std::list<int> value1(10);
std::list<int> value2(value1);
```

注意，采用此方式，必须保证新旧容器存储的元素类型一致。

**(5) 通过拷贝其他类型容器（或者普通数组）中指定区域内的元素，可以创建新的 list 容器。例如：**

```c++
//拷贝普通数组，创建list容器
int a[] = { 1,2,3,4,5 };
std::list<int> values(a, a+5);

//拷贝其它类型的容器，创建 list 容器
std::array<int, 5>arr{ 11,12,13,14,15 };
std::list<int>values(arr.begin()+2, arr.end());//拷贝arr容器中的{13,14,15}
```

#### list容器可用的成员函数

表 2 中罗列出了 list 模板类提供的所有成员函数以及各自的功能。

| 成员函数        | 功能                                                         |
| --------------- | ------------------------------------------------------------ |
| begin()         | 返回指向容器中第一个元素的双向迭代器。                       |
| end()           | 返回指向容器中最后一个元素所在位置的下一个位置的双向迭代器。 |
| rbegin()        | 返回指向最后一个元素的反向双向迭代器。                       |
| rend()          | 返回指向第一个元素所在位置前一个位置的反向双向迭代器。       |
| cbegin()        | 和 begin() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| cend()          | 和 end() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| crbegin()       | 和 rbegin() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| crend()         | 和 rend() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| empty()         | 判断容器中是否有元素，若无元素，则返回 true；反之，返回 false。 |
| size()          | 返回当前容器实际包含的元素个数。                             |
| max_size()      | 返回容器所能包含元素个数的最大值。这通常是一个很大的值，一般是 232-1，所以我们很少会用到这个函数。 |
| front()         | 返回第一个元素的引用。                                       |
| back()          | 返回最后一个元素的引用。                                     |
| assign()        | 用新元素替换容器中原有内容。                                 |
| emplace_front() | 在容器头部生成一个元素。该函数和 push_front() 的功能相同，但效率更高。 |
| push_front()    | 在容器头部插入一个元素。                                     |
| pop_front()     | 删除容器头部的一个元素。                                     |
| emplace_back()  | 在容器尾部直接生成一个元素。该函数和 push_back() 的功能相同，但效率更高。 |
| push_back()     | 在容器尾部插入一个元素。                                     |
| pop_back()      | 删除容器尾部的一个元素。                                     |
| emplace()       | 在容器中的指定位置插入元素。该函数和 insert() 功能相同，但效率更高。 |
| insert()        | 在容器中的指定位置插入元素。                                 |
| erase()         | 删除容器中一个或某区域内的元素。                             |
| swap()          | 交换两个容器中的元素，必须保证这两个容器中存储的元素类型是相同的。 |
| resize()        | 调整容器的大小。                                             |
| clear()         | 删除容器存储的所有元素。                                     |
| splice()        | 将一个 list 容器中的元素插入到另一个容器的指定位置。         |
| remove(val)     | 删除容器中所有等于 val 的元素。                              |
| remove_if()     | 删除容器中满足条件的元素。                                   |
| unique()        | 删除容器中相邻的重复元素，只保留一个。                       |
| merge()         | 合并两个事先已排好序的 list 容器，并且合并之后的 list 容器依然是有序的。 |
| sort()          | 通过更改容器中元素的位置，将它们进行排序。                   |
| reverse()       | 反转容器中元素的顺序。                                       |

 
除此之外，[C++](http://c.biancheng.net/cplus/) 11 标准库还新增加了 begin() 和 end() 这 2 个函数，和 list 容器包含的 begin() 和 end() 成员函数不同，标准库提供的这 2 个函数的操作对象，既可以是容器，还可以是普通数组。当操作对象是容器时，它和容器包含的 begin() 和 end() 成员函数的功能完全相同；如果操作对象是普通数组，则 begin() 函数返回的是指向数组第一个元素的指针，同样 end() 返回指向数组中最后一个元素之后一个位置的指针（注意不是最后一个元素）。

list 容器还有一个`std::swap(x , y)`非成员函数（其中 x 和 y 是存储相同类型元素的 list 容器），它和 swap() 成员函数的功能完全相同，仅使用语法上有差异。

如下代码演示了表 2 中部分成员函数的用法：

```c++
#include <iostream>
#include <list>
using namespace std;

int main()
{
    //创建空的 list 容器
    std::list<double> values;
    //向容器中添加元素
    values.push_back(3.1);
    values.push_back(2.2);
    values.push_back(2.9);
    cout << "values size：" << values.size() << endl;
    //对容器中的元素进行排序
    values.sort();
    //使用迭代器输出list容器中的元素
    for (std::list<double>::iterator it = values.begin(); it != values.end(); ++it) {
        std::cout << *it << " ";
    }
    return 0;
}
```

运行结果为：

```
values size：3
2.2 2.9 3.1
```

> 表 2 中这些成员函数的具体用法，后续学习用到时会具体讲解，感兴趣的读者，也可以通过查阅[ STL手册](http://www.cplusplus.com/reference/stl/)做详细了解。

**电梯案例**：获取电梯人数以及姓名，并打印出来

- 思路：

  - 1.抽象人员

  - 2.抽象电梯（list）

  - 3.进电梯的人（queue）

  - 4.把电梯喝出电梯的人拷贝一份放入vector中，待打印

- 流程：
  - 1. 电梯上升（1-7）
  - 2. 创建人员
  - 3. 判断电梯条件，然后进电梯
  - 4. 判断出电梯条件，然后出电梯
  - 5. 打印出电梯喝进电梯人员的人数和姓名



### 2.2 STL算法

算法分为**质变算法**和**非质变算法**

- 质变算法：运行过程中会更改区间内元素的内容
- 非质变算法：不会改变

C++标准库定义了一组泛型算法，之所以称为泛型指的是它们可以操作在多种容器上，不但可以作用于标准库类型，还可以用在内置数组类型甚至其它类型的序列上。泛型算法定义在<algorithm>头文件中，标准库还定义了一组泛化的算术算法（Generalized Numeric Algorithm），定义在<numeric>头文件中。使用方法如下：

```c++
#include <iostream>
#include <algorithm>
#include <numeric>
using namespace std;

int main()
{
    // 使用STL容器时将数组指针改为迭代器即可

    int a[5] = { 1, 2, 3, 4, 5 };
    int b[5] = { 0 };

    // 排序算法
    sort(a, a + 5);  // 将区间[0, 5)内元素按字典序从小到大排序
    sort(a, a + 5, greater<int>());  // 将区间[0, 5)内元素按字典序从大到小排序
    reverse(a, a + 5);  // 将区间[0, 5)内元素翻转
    nth_element(a, a + 3, a + 5);  // 将区间[0, 5)中第a + 3个数归位，即将第3大的元素放到正确的位置上，该元素前后的元素不一定有序

    // 查找与统计算法
    find(a, a + 5, 3);  // 在区间[0, 5)内查找等于3的元素，返回迭代器，若不存在则返回end()
    binary_search(a, a + 5, 2);  // 二分查找区间[0, 5)内是否存在元素2，若存在返回true否则返回false
    count(a, a + 5, 3);  // 返回区间[0, 5)内元素3的个数

    // 可变序列算法
    copy(a, a + 2, a + 3);  // 将区间[0, 2)的元素复制到以a+3开始的区间，即[3, 5)
    replace(a, a + 5, 3, 4);  // 将区间[0, 5)内等于3的元素替换为4
    fill(a, a + 5, 1);  // 将1写入区间[0, 5)中(初始化数组函数)
    unique(a, a + 5);  // 将相邻元素间的重复元素全部移动至末端，返回去重之后数组最后一个元素之后的地址
    remove(a, a + 5, 3);  // 将区间[0, 5)中的元素3移至末端，返回新数组最后一个元素之后的地址

    // 排列算法
    next_permutation(a, a + 5);  // 产生下一个排列{ 1, 2, 3, 5, 4 }
    prev_permutation(a, a + 5);  // 产生上一个排列{ 1, 2, 3, 4, 5 }

    // 前缀和算法
    partial_sum(a, a + 5, b);  // 计算数组a在区间[0, 5)内的前缀和并将结果保存至数组b中，b = { 1, 3, 6, 10, 15 }

    // 差分算法(感谢willem248同学的补充)
    adjacent_difference(a, a + 5, b);  // 计算数组a区间[0, 5)内的差分并将结果保存至数组b中，b = { 1, 1, 1, 1, 1 }
    adjacent_difference(a, a + 5, b, plus<int>());  // 计算相邻两元素的和，b = { 1, 3, 5, 7, 9 }
    adjacent_difference(a, a + 5, b, multiplies<int>());  // 计算相邻两元素的乘积，b = { 1, 2, 6, 12, 20 }

    return 0;
}
```

### 2.3 迭代器

STL库支持的迭代器类型是所有编程语言中最全面的[[2\]](https://zhuanlan.zhihu.com/p/352606819#ref_2)，共有五种：

| **输入迭代器**     | **提供对数据的只读访问**           | **支持++、==、！=**            |
| ------------------ | ---------------------------------- | ------------------------------ |
| **输出迭代器**     | **对数据的只写访问**               | **支持++**                     |
| **前向迭代器**     | **读写操作，并能向前推进迭代器**   | **支持++、==、！=**            |
| **双向迭代器**     | 提供读写操作，并能向前向后操作     | 支持++、--                     |
| **随机访问迭代器** | 提供读写操作、在数据中可以随机移动 | 支持++、--、[n]、-n、<=、>、>= |

1.  **InputIterator**：输入迭代器。支持对容器元素的逐个遍历，以及对元素的读取（input)；
2. **OutputIterator**：输出迭代器。支持对容器元素的逐个遍历，以及对元素的写入（output)。
3. **ForwardIterator**：前向迭代器。向前逐个遍历元素。可以对元素读取；
4. **BidirectionalIterator**：双向迭代器。支持向前向后逐个遍历元素，可以对元素读取。
5. **RandomAccessIterator**：随机访问迭代器。支持O(1)时间复杂度对元素的随机位置访问，支持对元素的读取。

输出迭代器可以修改元素，这可能会导致内部结构的调整，进而导致原有的迭代器失效！可能的情况有：

1. 结构和元素顺序变更：比如对map，set，priority_queue插入元素；
2. 内存变化：比如对vector插入元素，可能导致重新申请内存并拷贝！

从上面的描述我们可以很容易的知道他们之间的关系：

![img](https://pic4.zhimg.com/80/v2-85338ebc8dc1eae5d42a66156a9ffd5f_720w.webp)

实际上在标准库上的实现如下（GNU):

```c++
//  Marking input iterators.
  struct input_iterator_tag { };

  ///  Marking output iterators.
  struct output_iterator_tag { };

  /// Forward iterators support a superset of input iterator operations.
  struct forward_iterator_tag : public input_iterator_tag { };

  /// Bidirectional iterators support a superset of forward iterator
  /// operations.
  struct bidirectional_iterator_tag : public forward_iterator_tag { };

  /// Random-access iterators support a superset of bidirectional
  /// iterator operations.
  struct random_access_iterator_tag : public bidirectional_iterator_tag { };
```

#### 迭代器类型成员

一个迭代器的类型成员包含如下：

```c++
 template<typename _Category, typename _Tp, typename _Distance = ptrdiff_t,
           typename _Pointer = _Tp*, typename _Reference = _Tp&>
    struct iterator
    {
      /// One of the @link iterator_tags tag types@endlink.
      typedef _Category  iterator_category;
      /// The type "pointed to" by the iterator.
      typedef _Tp        value_type;
      /// Distance between iterators is represented as this type.
      typedef _Distance  difference_type;
      /// This type represents a pointer-to-value_type.
      typedef _Pointer   pointer;
      /// This type represents a reference-to-value_type.
      typedef _Reference reference;
    };
```

所有容器的迭代器实现必须都包含如上的五个类型成员！这些成员会作为各个算法的类型参数或者用于iterator traits。

由于各个不同的容器其内部实现不同，且元素的数据结构也不同，所以stl并没有一个统一的iterator类实现。即各个不同容器在内部实现其自己的迭代器，同时对迭代器的各个类型成员重定义，以符合struct iterator的定义规范。

***该类最初本想所有的迭代器实现类均继承之，但是从一开始就没有使用过！仅仅成为了可选的实现。所以在C++17中将该类废弃了***。

#### iterator traits

迭代器类型萃取可以提取迭代器的各个类型。

```c++
template<typename _Iterator, typename = __void_t<>>
    struct __iterator_traits { };

  template<typename _Iterator>
    struct __iterator_traits<_Iterator,
			     __void_t<typename _Iterator::iterator_category,
				      typename _Iterator::value_type,
				      typename _Iterator::difference_type,
				      typename _Iterator::pointer,
				      typename _Iterator::reference>>
    {
      typedef typename _Iterator::iterator_category iterator_category;
      typedef typename _Iterator::value_type        value_type;
      typedef typename _Iterator::difference_type   difference_type;
      typedef typename _Iterator::pointer           pointer;
      typedef typename _Iterator::reference         reference;
    };

  template<typename _Iterator>
    struct iterator_traits
    : public __iterator_traits<_Iterator> { };
```

#### 迭代器的基本实现

迭代器在c++实际实现为指针。即使用指针访问对应索引的对象。为了更加直观简便的使用迭代器，各个迭代器均对该指针重载了"->"和"*"解引用操作符。而不需要向某些编程语言一样需要执行如下操作：

```
itr->value();
itr->next();
```

而c++中则：

```cpp
itr = container.begin();
itr++;
itr--;
*itr;
itr->...
```

以std::list的迭代器实现为例：

```cpp
  template<typename _Tp>
    struct _List_iterator
    {
      typedef _List_iterator<_Tp>		_Self;
      typedef _List_node<_Tp>			_Node;

      typedef ptrdiff_t				difference_type;
      typedef std::bidirectional_iterator_tag	iterator_category;
      typedef _Tp				value_type;
      typedef _Tp*				pointer;
      typedef _Tp&				reference;

      _List_iterator() _GLIBCXX_NOEXCEPT
      : _M_node() { }

      explicit
      _List_iterator(__detail::_List_node_base* __x) _GLIBCXX_NOEXCEPT
      : _M_node(__x) { }

      _Self
      _M_const_cast() const _GLIBCXX_NOEXCEPT
      { return *this; }

      // Must downcast from _List_node_base to _List_node to get to value.
      reference
      operator*() const _GLIBCXX_NOEXCEPT
      { return *static_cast<_Node*>(_M_node)->_M_valptr(); }

      pointer
      operator->() const _GLIBCXX_NOEXCEPT
      { return static_cast<_Node*>(_M_node)->_M_valptr(); }

      _Self&
      operator++() _GLIBCXX_NOEXCEPT
      {
	_M_node = _M_node->_M_next;
	return *this;
      }

      _Self
      operator++(int) _GLIBCXX_NOEXCEPT
      {
	_Self __tmp = *this;
	_M_node = _M_node->_M_next;
	return __tmp;
      }

      _Self&
      operator--() _GLIBCXX_NOEXCEPT
      {
	_M_node = _M_node->_M_prev;
	return *this;
      }

      _Self
      operator--(int) _GLIBCXX_NOEXCEPT
      {
	_Self __tmp = *this;
	_M_node = _M_node->_M_prev;
	return __tmp;
      }

      friend bool
      operator==(const _Self& __x, const _Self& __y) _GLIBCXX_NOEXCEPT
      { return __x._M_node == __y._M_node; }

      friend bool
      operator!=(const _Self& __x, const _Self& __y) _GLIBCXX_NOEXCEPT
      { return __x._M_node != __y._M_node; }

      // The only member points to the %list element.
      __detail::_List_node_base* _M_node;
    };
```

#### 具体使用

迭代器的常见使用方式有三种。

##### **常规遍历方式**

```cpp
std::vector<int> vec;
...
for (std::vector<int>::iterator itr = vec.begin(); itr != vec.end(); ++itr)
{
    // 需要对itr执行解引用！
    ...
}
```

这是最为常用的方式！

##### **c++11遍历方式**

```cpp
std::vector<int> vec;
...
for(int item : vec)
{
    // item已经执行了解引用
    ...
}
```

##### **c++11工具函数**

c++11增加了两个工具函数begin()/end()，支持对原生数组的迭代器访问，同时也支持对已有容器的访问。

```cpp
int eles[10] = {...};
for (auto itr = begin(eles); itr != end(eles); ++itr)
{
    ...
}
```

##### 容器迭代器的获取

各个容器对象提供了一系列的获取迭代器的方法。

1. begin()/end()：获取的迭代器的类型是T::iterator，可通过该迭起修改对应元素。即可变。
2. cbegin()/end()：获取的迭代器的类型是T:const_iterator，仅能读取对应元素，不可修改之。即常量访问。
3. rbegin()/rend()：获取的迭代器的类型是T::reverse_iterator。反向访问元素，即从最后一个元素向第一个元素遍历。可修改对应元素，即可变。
4. crbegin()/crend()：获取的迭代器的类型是T::const_reverse_iterator。反向访问元素，即从最后一个元素向第一个元素遍历。不可修改对应元素，即常量访问。
5. 从c++11开始，提供了工具函数begin()/end()。

获取容器迭代器是算法的第一步。获取后即完成了于容器的解耦。

#### 算法的使用

标准库对容器操作提供了丰富的算法，提高开发人员的开发效率。这些算法全是模板函数，传入迭代器。迭代器的类型在类型参数中显示标明，但是实际的类型检查在运行期间才能判断出来。

下面是几个标准库中提供的函数原型：

```cpp
template< class InputIt, class T >
typename iterator_traits<InputIt>::difference_type
    count( InputIt first, InputIt last, const T &value );

template< class BidirIt1, class BidirIt2 >
BidirIt2 copy_backward( BidirIt1 first, BidirIt1 last, BidirIt2 d_last );

template< class ForwardIt, class UnaryPredicate >
ForwardIt partition( ForwardIt first, ForwardIt last, UnaryPredicate p );
```

下面是一个具体的三大组件联合使用的快速排序的实例：

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>
#include <forward_list>
 
template <class ForwardIt>
 void quicksort(ForwardIt first, ForwardIt last)
 {
    if(first == last) return;
    auto pivot = *std::next(first, std::distance(first,last)/2);
    ForwardIt middle1 = std::partition(first, last, 
                         [pivot](const auto& em){ return em < pivot; });
    ForwardIt middle2 = std::partition(middle1, last, 
                         [pivot](const auto& em){ return !(pivot < em); });
    quicksort(first, middle1);
    quicksort(middle2, last);
 }
 
int main()
{
    std::vector<int> v = {0,1,2,3,4,5,6,7,8,9};
    std::cout << "Original vector:\n    ";
    for(int elem : v) std::cout << elem << ' ';
 
    auto it = std::partition(v.begin(), v.end(), [](int i){return i % 2 == 0;});
 
    std::cout << "\nPartitioned vector:\n    ";
    std::copy(std::begin(v), it, std::ostream_iterator<int>(std::cout, " "));
    std::cout << " * " " ";
    std::copy(it, std::end(v), std::ostream_iterator<int>(std::cout, " "));
 
    std::forward_list<int> fl = {1, 30, -4, 3, 5, -4, 1, 6, -8, 2, -5, 64, 1, 92};
    std::cout << "\nUnsorted list:\n    ";
    for(int n : fl) std::cout << n << ' ';
    std::cout << '\n';  
 
    quicksort(std::begin(fl), std::end(fl));
    std::cout << "Sorted using quicksort:\n    ";
    for(int fi : fl) std::cout << fi << ' ';
    std::cout << '\n';
}
```

#### 容器对迭代器的支持

并不是所有的stl容器均支持迭代器！

##### 支持迭代器的容器

1. array:
2. vector:
3. deque:双向队列
4. forward_list:单向（前向）列表
5. list:双向链表
6. set:
7. map:

##### 不支持迭代器的容器

不支持迭代器的均是容器适配器，其提供对其他容器的统一接口的访问。

1. stack：栈
2. queue：
3. priority_queue：优先队列

##### 迭代器失效问题

在对容器元素访问的时候，迭代器可能会失效，即迭代器指向的元素已经失效（内存不可访问）。这个是哪怕老手也很容易忽视的问题，并且一旦出问题，非常难以排查定位！

1. ***顺序型容器\***：在修改元素时迭代器不会失效！而删除和插入时可能导致失效。（可能是因为内存可能重新申请）。
2. ***关联型容器\***：在修改/删除/插入时均可能导致迭代器失效！

**[2023/8/10/P165]**















# 参考：

> 1. [C-STL常用容器API总结](https://vernlium.github.io/2019/12/29/C-STL%E5%B8%B8%E7%94%A8%E5%AE%B9%E5%99%A8API%E6%80%BB%E7%BB%93/)