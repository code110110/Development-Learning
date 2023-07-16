#  C++基础

## Day 1

## 03.面向对象的三大特征（*）

封装，继承，多态

## Using 声明和编译指令（**）

Using让命名空间中的某个标识符可以直接使用

## 06struct类型加强（**）

1.定义变量时不需要使用struct

2.结构体内可以写函数

## 07更加严格的类型转换（*）

不能隐性，必须显示转换

## 08.三目运算符（*）

c语言的三目运算符返回右值

C++语言的三目运算符返回的是左值，是空间

## 09.C/C++的const（**）

## C++ 中的类型限定符

类型限定符提供了变量的额外信息，用于在定义变量或函数时改变它们的默认行为的关键字。

| 限定符   | 含义                                                         |
| :------- | ------------------------------------------------------------ |
| const    | **const** 定义常量，表示该变量的值不能被修改。。             |
| volatile | 修饰符 **volatile** 告诉该变量的值可能会被程序以外的因素改变，如硬件或其他线程。。 |
| restrict | 由 **restrict** 修饰的指针是唯一一种访问它所指向的对象的方式。只有 C99 增加了新的类型限定符 restrict。 |
| mutable  | 表示类中的成员变量可以在 const 成员函数中被修改。            |
| static   | 用于定义静态变量，表示该变量的作用域仅限于当前文件或当前函数内，不会被其他文件或函数访问。 |
| register | 用于定义寄存器变量，表示该变量被频繁使用，可以存储在CPU的寄存器中，以提高程序的运行效率 |

#### 1.C语言的const修饰的变量都有空间

#### 2.C语言的const修饰的全局变量都有外部链接属性

#### 3.C++的const修饰的变量可以有空间，也可以没有空间（发生在常量折叠，且对变量没有进行取地址操作）

#### 4.C++语言中的const修饰的全局变量具有内部链接属性

#### 5.加上extern就变为外部链接属性

#### 6.c++编译器不能优化的情况

​	（1）自定义的数据类型

​	（2）如果用变量给const修饰的局部变量赋值，那么编译器就不能优化



## 10.引用（**）

跟指针一样的功能

引用就是给空间取别名

```c++
void func(int &a)//int &a=a;
{
    a=200;
}
void test02()
{
    int a=10;
    func(a);
    cout<<"a="<<a<<endl;
}
```

注意事项：

（1）int &b=a;这里的&不是取地址操作符，是起引用的标记的作用。

（2）创建引用时，必须初始化。

（3）引用一但初始化，就不能改变它的指向。

```c++
int a=10;
int &pRef=a;//给a的空间取别名为pRef
int b=20;
&pRef=b;//赋值操作
```

（4）引用必须用一块合法的内存空间

引用的本质在c++内部实现是一个常指针

## 11.指针的引用（**）

指针的引用是给指针变量这块空间取别名

```c++
void test01()
    P{}
```



## 12.数组

### 声明数组

在 C++ 中要声明一个数组，需要指定元素的类型和元素的数量，如下所示：

```
type arrayName [ arraySize ];
```

这叫做一维数组。**arraySize** 必须是一个大于零的整数常量

### 初始化数组

在 C++ 中，您可以逐个初始化数组，也可以使用一个初始化语句，如下所示：

```
double balance[5] = {1000.0, 2.0, 3.4, 7.0, 50.0};
```

大括号 { } 之间的值的数目不能大于我们在数组声明时在方括号 [ ] 中指定的元素数目。

如果您省略掉了数组的大小，数组的大小则为初始化时元素的个数。

## 13.指针

## Day 2



## 03内联函数（*）

### 基本概念

在c++中，预定义宏的概念是用内联函数来实现的，内敛函数是一个真正的函数。内联函数具有普通函数的所有行为。唯一不同之处在于内联函数会在适当的地方像预定义宏一样展开，所以不需要函数调用的开销。因此应该不适用宏，使用内联函数。

​	在普通函数（非成员函数）函数前面加上inline关键字使之称为内联函数，但是必须注意函数体和声明结合在一起，否则编译器将它作为普通函数对待。

```c++
inline void func(int a);//此写法仅仅是声明函数，应用如下方式来做；
```

```c++
inline int func(int a){return ++;}
```

### 什么情况不会成为内联函数

- 1.存在过多的条件判断语句
- 2.函数体过大
- 3.对函数进行取址操作

### 内联函数的好处

- 1.有宏函数的效率，没有宏函数的缺点
- 2.类的成员函数默认加上inline

内联仅仅是给编译器一个建议，编译器不一定会接受这种建议，如果你没有将函数声明为内联函数，那么编译器也可能将此函数作为内联编译。一个好的编译器会内联小的，简单的函数。

## 04.函数的默认参数（**）

函数的默认参数就是给函数的形参赋值

```c++
int myFunc(int a,int b=0)//int b=0;是函数的默认参数
{}
int main()
{ system("pause")
        return EXIT_SUCCESS}
```

#### 函数默认参数的使用

当函数内常用到某个形参的值，但偶尔用其他值

增加函数的灵活性

注意：

函数的默认参数后面的参数必须都是默认参数

```c++
int myFunc2(int a,int b=0,int c=2,int d=3)
{return a+b+c+d}
```

函数的声明和实现不能同时有函数的默认参数

```c++
void myFunc3(int a,int b);//函数的声明以;结束
void myFunc3(int a,int b=0)
{}
```

## 05.函数的占位参数

函数的占位参数，占位参数在后面运算符重载时区分前++或后++

```c++
void func(int a,int=10)//占位参数也有默认值
```

占位参数和默认参数混合使用

```c++
void func(int=10 ,int a=20)//一个参数无论时什么，第一个参数有默认值，后面的参数必都有
{}
void test()
{
    func();
    func(10);
    func(10,30);
}
```

### 值传递

```c++
void swap(int a,int b)
{
    int t=a;
    a=b;
    b=t;
}

```

### 指针传递

```c++
void swap2(int *a,int *b)
{
    int t=*a;
    *a=*b;
    *b=tmp;
}
```

### 引用传递

```c++
void swap3(int &a,int $b)
{
    int t=a;
    a=b;
    b=t;
}
```

====2023/6/15/P32==============

### 一个对象必须要初始化成员变量

成员函数中隐藏了一个本类的对象

## 对象的构造和析构

### 初始化和清理

1. 当对象产生时，必须初始化成员变量，当对象销毁前，必须清理对象
2. 初始化用构造函数，清理用析构函数，这两个函数由编译器调用

### 构造函数和析构函数

##### 注意：

1. 构造函数和析构函数的权限必须是公有的
2. 构造函数可以重载
3. 构造函数没有返回值，不能用void，构造函数可以有参数，析构函数没有返回值，不能用void,没有参数。

```c++
class Maker
{
    public:
    //构造函数的作用是初始化成员变量，由编译器调用
    Maker()//与类名一致
    {
        a=10;
    }
    //析构函数，在对象销毁前，编译器调用析构函数
    ~Maker()
    {
        
    }
};
//析构函数的作用
class Maker2
{
    public:
    //有参构造
    Maker2(const char *name,int age)
    {
        //堆空间申请
        pName=(char*)malloc(strlen(name)+1);
        strcpy(pName,name);
        mAge=age; 
    }
    //申请完堆区空间要及时释放，不然会发生内存泄漏。
    //打印函数
    void printMaker2()
    {
        count<<"name:"<<pName<<"age:"<<mAge<<endl;
    }
    //析构函数
    ~Maker2()
    {
        //释放堆区空间
        if(pName!=NULL)
        {
            free(pNmae);
            pName=NULL;
        }
    }
    private:
    char *pName;
    int mAge;
};

class Maker3
{
    public:
    //构造函数可以重载
    Maker3()//无参构造函数
    {
        
    }
    Maker3(int a)//有参构造函数
    {
        
    }
    ~Maker3()
    {
        
    }
}

void test02()
{
    Maker2 m2("name",12);
    m2.printMaker2();
    
}
void test01()
{
    //实例化对象，内部有两个事件：1分配空间，2.调用构造函数进行初始化
    Maker m;
    int b=m.a;
    count<<b<<endl;
}
void test03()
{
    Maker3 m;//ERROR:当构造参数私有化时，无法实例化对象
    //有对象产生必然会调用构造函数，有对象销毁必然会调用析构函数
    //有多少个对象就好调用多少次构造函数，有多少对象销毁就行调用多少次析构函数
    Maker3 m2(10);
}
int main()
{
   //运行test02();
    test02();
    
}
```

=======2023/6/19:p52

编译器提供默认的构造函数和析构函数

## 拷贝构造函数

```c++
class Maker
{
    public:
    Maker()//无参构造函数
    {
        a=20
    }
    //拷贝构造函数
    //编译器提供了默认的拷贝构造函数
    Maker(const Maker &m)
    {
        a=m.a;
    }  
    private:
    int a;
}
void test01()
{
    Maker m1;
    //用已有的对象去初始化另一个对象
    Maker m2(m1);
}
```

```c++
class Maker2
{
    public Maker2(int Ma)
    {
        ma=Ma;
    }
    Maker2(const Maker3 &m)
    {
        
    }
}
```

### 匿名对象

匿名对象的生命周期在当前行

若匿名对象有名字来接，就不是匿名对象。

### 拷贝构造函数的调用时机

1.对象以值方式给函数参数

2.用一个已有对象去初始化另一个调用对象

3.函数的局部对象以值的方式从函数返回

```c++
Maker func2()
{
    //局部对象
    Maker m;
    return m;
}
void test()
{
    Maker m1=func2();
    
}
```

 注：VS的Debugh模式下，会调用拷贝构造，Release模式下不会调用拷贝构造，QT也不调用

### 构造函数的调用规则

如果已经提供了有参构造，那么编译器不会提供默认构造函数，但是会提供默认的拷贝构造。

如果提供了有参构造，那么编译器不会提供默认构造函数，但是会提供默认的拷贝构造

##  多个对象的拷贝和析构

-  如果类有成员对象，那么先调用成员对象的构造函数，再调用本身的构造函数
- 析构函数的调用顺序相反
- 成员对象的构造函数调用和定义顺序一样。

**注意：如果有成员对象，那么实例化对象时，必须保证成员对象的构造和析构能被调用。**

```c++
class Maker2
{
    public:
    //初始化列表
    //注意：初始化列表只能写在构造函数
    //如果多个对象需要指定调用某个构造函数，用，隔开
    Maker2（int a,int b,int c）：bmw（a）,bui(b,c)
    {
    }
    //注意2：如果使用了初始化列表，那么所有的构造函数都要写初始化列表
    Maker2(const Maker &m2)：bmw(40),bui(10,20)
    {
        
    }
}
```

### 初始化列表

**作用**：初始化列表是调用成员对象的指定构造函数

可以使用对象的构造函数传递数值给成员对象的变量、

## 对象的深拷贝，浅拷贝

1.默认的拷贝构造函数进行了简单的赋值操作（浅拷贝）

```c++
class Student
{
    public:
    Student(const char *name,int Age)
    {
        pName=(char*)malloc(strlen(name)+1);
        strcpy(pName,name);
        age=Age;
    }
    //深拷贝
    Student(const Student &stu)
    {
        //1.申请空间
        PName=(char*)malloc(strlen(stu.pName)+1);
        //2.拷贝数据
        strcpy(pName,stu.pName);
        age=stu.age;
    }
    ~Student()
    {
        if(pName!=NULL)
        {
            free(pName);
            pName=NULL;
        }
    }
    public:
    char *pName;
    int age;
}

void test()
{
    //调用有参构造
    Student s1("SS",18);
    //调用默认的拷贝构造，进行了简单的赋值操作（s2.pName=s1.pName）
    Student s2(s1);//ERROR
    //浅拷贝的问题，同一块空间会被释放两次
}
```

2.深拷贝解决了浅拷贝对同一地址释放两次的问题。

## explicit的作用（*）

explicit（显式的）的作用是"禁止单参数构造函数"被用于自动型别转换，其中比较典型的例子就是容器类型。在这种类型的构造函数中你可以将初始长度作为参数传递给构造函数。

禁止通过构造函数进行的隐藏转换。声明为explicit的构造函数不能在隐式转换中使用。

```c++
class Maker
{
    public:
    //explicit只能放在构造函数前面，构造函数只有一个参数或其他参数有默认值时
    explicit Maker(int n)//防止编译器优化Maker m=10;这种格式
}
```

## C++ 动态内存

C++ 程序中的内存分为两个部分：

- **栈：**在函数内部声明的所有变量都将占用栈内存。
- **堆：**这是程序中未使用的内存，在程序运行时可用于动态分配内存。

在 C++ 中，您可以使用特殊的运算符为给定类型的变量在运行时分配堆内的内存，这会返回所分配的空间地址。这种运算符即 **new** 运算符。

如果您不再需要动态分配的内存空间，可以使用 **delete** 运算符，删除之前由 new 运算符分配的内存。

### new 和 delete 运算符

下面是使用 new 运算符来为任意的数据类型动态分配内存的通用语法：

```c++
new data-type;
```

在这里，**data-type** 可以是包括数组在内的任意内置的数据类型，也可以是包括类或结构在内的用户自定义的任何数据类型。让我们先来看下内置的数据类型。例如，我们可以定义一个指向 double 类型的指针，然后请求内存，该内存在执行时被分配。我们可以按照下面的语句使用 **new** 运算符来完成这点：

```c++
double* pvalue  = NULL; // 初始化为 null 的指针
pvalue  = new double;   // 为变量请求内存
```

如果自由存储区已被用完，可能无法成功分配内存。所以建议检查 new 运算符是否返回 NULL 指针，并采取以下适当的操作：

```c++
double* pvalue  = NULL;
if( !(pvalue  = new double ))
{
   cout << "Error: out of memory." <<endl;
   exit(1);
 
}
```

**malloc()** 函数在 C 语言中就出现了，在 C++ 中仍然存在，但建议尽量不要使用 malloc() 函数。new 与 malloc() 函数相比，其主要的优点是，new 不只是分配了内存，它还创建了对象。

在任何时候，当您觉得某个已经动态分配内存的变量不再需要使用时，您可以使用 delete 操作符释放它所占用的内存，如下所示：

```c++
delete pvalue;        // 释放 pvalue 所指向的内存
```

下面的实例中使用了上面的概念，演示了如何使用 new 和 delete 运算符：

### 实例

```c++
#include <iostream>
using namespace std;
 
int main ()
{
   double* pvalue  = NULL; // 初始化为 null 的指针
   pvalue  = new double;   // 为变量请求内存
 
   *pvalue = 29494.99;     // 在分配的地址存储值
   cout << "Value of pvalue : " << *pvalue << endl;
 
   delete pvalue;         // 释放内存
 
   return 0;
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```
Value of pvalue : 29495
```

### 数组的动态内存分配

假设我们要为一个字符数组（一个有 20 个字符的字符串）分配内存，我们可以使用上面实例中的语法来为数组动态地分配内存，如下所示：

```c++
char* pvalue  = NULL;   // 初始化为 null 的指针
pvalue  = new char[20]; // 为变量请求内存
```

要删除我们刚才创建的数组，语句如下：

```c++
delete [] pvalue;        // 删除 pvalue 所指向的数组
```

下面是 new 操作符的通用语法，可以为多维数组分配内存，如下所示：

#### 一维数组

```c++
// 动态分配,数组长度为 m 
int *array=new int [m];  
//释放内存 
delete [] array;
```



#### 二维数组

```c++
int **array; 
// 假定数组第一维长度为 m， 第二维长度为 n 
// 动态分配空间 
array = new int *[m]; 
for( int i=0; i<m; i++ ) 
{    
    array[i] = new int [n]; 
} 
//释放 
for( int i=0; i<m; i++ ) 
{    
    delete [] array[i]; 
}
delete [] array;
```

二维数组实例测试：

#### 实例

```c++
#include <iostream>
using namespace std;
 
int main()
{
    int **p;   
    int i,j;   //p[4][8] 
    //开始分配4行8列的二维数据   
    p = new int *[4];
    for(i=0;i<4;i++){
        p[i]=new int [8];
    }
 
    for(i=0; i<4; i++){
        for(j=0; j<8; j++){
            p[i][j] = j*i;
        }
    }   
    //打印数据   
    for(i=0; i<4; i++){
        for(j=0; j<8; j++)     
        {   
            if(j==0) cout<<endl;   
            cout<<p[i][j]<<"\t";   
        }
    }   
    //开始释放申请的堆   
    for(i=0; i<4; i++){
        delete [] p[i];   
    }
    delete [] p;   
    return 0;
}
```

#### 三维数组

```c++
int ***array;
// 假定数组第一维为 m， 第二维为 n， 第三维为h
// 动态分配空间
array = new int **[m];
for( int i=0; i<m; i++ )
{
    array[i] = new int *[n];
    for( int j=0; j<n; j++ )
    {
        array[i][j] = new int [h];
    }
}
//释放
for( int i=0; i<m; i++ )
{
    for( int j=0; j<n; j++ )
    {
        delete[] array[i][j];
    }
    delete[] array[i];
}
delete[] array;
```

三维数组测试实例：

#### 实例

```c++
#include <iostream>
using namespace std;
 
int main()
{   
    int i,j,k;   // p[2][3][4]
    
    int ***p;
    p = new int **[2]; 
    for(i=0; i<2; i++) 
    { 
        p[i]=new int *[3]; 
        for(j=0; j<3; j++) 
            p[i][j]=new int[4]; 
    }
    
    //输出 p[i][j][k] 三维数据
    for(i=0; i<2; i++)   
    {
        for(j=0; j<3; j++)   
        { 
            for(k=0;k<4;k++)
            { 
                p[i][j][k]=i+j+k;
                cout<<p[i][j][k]<<" ";
            }
            cout<<endl;
        }
        cout<<endl;
    }
    
    // 释放内存
    for(i=0; i<2; i++) 
    {
        for(j=0; j<3; j++) 
        {   
            delete [] p[i][j];   
        }   
    }       
    for(i=0; i<2; i++)   
    {       
        delete [] p[i];   
    }   
    delete [] p;  
    return 0;
}
```

------

#### 对象的动态内存分配

对象与简单的数据类型没有什么不同。例如，请看下面的代码，我们将使用一个对象数组来理清这一概念：

#### 实例

```c++
#include <iostream>
using namespace std;
 
class Box
{
   public:
      Box() { 
         cout << "调用构造函数！" <<endl; 
      }
      ~Box() { 
         cout << "调用析构函数！" <<endl; 
      }
};
 
int main( )
{
   Box* myBoxArray = new Box[4];
 
   delete [] myBoxArray; // 删除数组
   return 0;
} 
```

如果要为一个包含四个 Box 对象的数组分配内存，构造函数将被调用 4 次，同样地，当删除这些对象时，析构函数也将被调用相同的次数（4次）。

当上面的代码被编译和执行时，它会产生下列结果：

```
调用构造函数！
调用构造函数！
调用构造函数！
调用构造函数！
调用析构函数！
调用析构函数！
调用析构函数！
调用析构函数！
```

## c++的堆空间的申请和释放（**）

1.申请和释放变量空间

```c++
class Maker
{
    public：
    Maker()
    {
      
    }
    Maker(int a)
    {
        
    }
    ~Maker()
    {
        
    }
};
void test()
{
    //用new方式申请堆区空间，会调用类的构造函数
    Maker *m=new Maker;
    //释放堆区空间，会调用类的析构函数
    delete m;
    m=NULL;
}
```

# ==========2023/6/20:P65======

```c++
//new创造基础类型的数组
void test01()
{
    int *PInt=new int[10];//申请基础数据类型的数组
    for (int i=0;i<10;i++)
    {
        pInt[i]]i+1;
    }
    char *pChar=new char[64];
    //如果new时有中括号，那么delete时也要有中括号
    delete[] pInt;
    delete[] pChar;
}

```

```c++
//delete void*可能会出错，不会调用对象的析构函数
void test03()
{
    void *m=new Maker;
    //如果用void*接上new的对象，那么delete时不会调用析构函数
    delete m;
    //在编译阶段，编译器确定好了函数的调用地址。
    //c++编译器不认识void*，不知道void指向哪个函数，所以不会调用析构函数
    //这种编译方式叫静态联编
}
```

注：**C和c++的申请释放堆区空间不能混用**

唯一区别在于，c++在申请和释放时会调用析构函数

## 静态成员（*）

```c++
class Maker
{
    public:
    //静态成员变量的生命周期是整个程序，作用域在类内
    static int a；
};
//2.静态成员变量要在类内声明，类外初始化
int Maker::a=100;
```

**3.静态成员变量属于类，不属于对象，是所有对象共享。**

**4.静态成员变量可以用类访问，也可以用对象访问。**

**5.静态成员函数只能访问静态成员变量**。

**6.静态成员也有权限，如果私有，类外也不能访问**

**7.const修饰的静态成员变量，最好在类内初始化(类外也可以)。**

```c++
class Maker4
{
    public:
    const static int a=20;
    const static int b;
};
//类外也可以初始化
const int Maker4::b=30;
```

8.普通成员函数可以访问

## c++的对象模型（**）

 空类的大小为1，这样编译器可以更好的从内存区分对象

类的成员函数不占用类的大小，静态成员变量不占用类的大小，静态成员函数不占用类的大小

普通成员变量占用类的大小

类的成员中，成员函数和成员变量是分开存储

## this指针（***）

this 是 [C++](http://c.biancheng.net/cplus/) 中的一个关键字，也是一个 const [指针](http://c.biancheng.net/c/80/)，它指向当前对象，通过它可以访问当前对象的所有成员。

所谓当前对象，是指正在使用的对象。例如对于`stu.show();`，stu 就是当前对象，this 就指向 stu。

this 只能用在类的内部，通过 this 可以访问类的所有成员，包括 private、protected、public 属性的

**注意，this 是一个指针，要用`->`来访问成员变量或成员函数。**

this 虽然用在类的内部，但是只有在对象被创建以后才会给 this 赋值，并且这个赋值的过程是编译器自动完成的，不需要用户干预，用户也不能显式地给 this 赋值。本例中，this 的值和 pstu 的值是相同的。

几点注意：

- this 是 const 指针，它的值是不能被修改的，一切企图修改该指针的操作，如赋值、递增、递减等都是不允许的。
- this 只能在成员函数内部使用，用在其他地方没有意义，也是非法的。
- 只有当对象被创建后 this 才有意义，因此不能在 static 成员函数中使用（后续会讲到 static 成员）。

### this 到底是什么

this 实际上是成员函数的一个形参，在调用成员函数时将对象的地址作为实参传递给 this。不过 this 这个形参是隐式的，它并不出现在代码中，而是在编译阶段由编译器默默地将它添加到参数列表中。

this 作为隐式形参，本质上是成员函数的局部变量，所以只能用在成员函数的内部，并且只有在通过对象调用成员函数时才给 this 赋值。

在《[C++函数编译原理和成员函数的实现](http://c.biancheng.net/view/vip_2220.html)》一节中讲到，成员函数最终被编译成与对象无关的普通函数，除了成员变量，会丢失所有信息，所以编译时要在成员函数中添加一个额外的参数，把当前对象的首地址传入，以此来关联成员函数和成员变量。这个额外的参数，实际上就是 this，它是成员函数和成员变量关联的桥梁。

每个对象都有一个隐藏的this指针，但不属于对象，是编译器添加的。

编译器会把this指针传入成员函数内

```c++
class Maker2
{
    public:
    //当形参名和成员变量名相同时，用this指针区分
    Maker2(int id)
    {
        this->id=id;
    }
    //2.返回对象的本身
    Maker2 &getMaker2()
    {
        return *this;//运算符重载时有用
    }  
}
```

```c++
public:
Maker(int id,int age)
{
    this->id=id;
    this->age=age;
}
//常函数，1.函数的()后面添加const，该函数是常函数
void printMaker()const
{
    //id=100;err//2.常函数不能修改普通成员变量
    //3.const修饰的是this指针指向的空间中的变量不能改变 
}
```



## 常函数和常对象（**）

### 常函数

```c++
class Maker
{
    public:
    Maker(int id,int age)
    {
        this->id=id;
        this->age=age;
    }
    //在函数后面加上const,构成常函数
    //常函数
    void printMaker()const
    {
        //id=100;//ERROR：常函数内不能修改普通成员变量
        //const修饰的是this指针指向的空间中的变量不能改变
        //const Maker*const this;这是常函数修饰的
        score=200;//mutable修饰的成员变量在常函数中可以修改
    }
  public:
    int id;
    int age;
    mutable int score;//mutbale修饰的成员变量
}
```

### 常对象

```c++
void test()
{ //在数据前面加上const，让对象称为常对象
   const Maker m(1,18);//常对象
    //m.id=100;ERRoR//常对象不能改变普通成员变量的值
    //m.func();//常对象不能调用普通成员函数
    m.printMaker();//常对象可以调用常函数
    m.score=500;//常对象可以调用mutable修饰的成员变量
    Maker m2(2,20);
    m2.printMaker();//普通对象可以调用常函数
}
```

## 友元（***）

类的主要特点之一是主句隐藏，即类的私有成员无法在类的外部（作用域之外）访问。但是，有时候需要在类的外部访问类的私有成员。需要用到友元，友元函数是一种特权函数，c++允许这个特权函数访问私有成员。

### 概念：

友元是赋予全局函数，

### 语法：

- friend关键字只出现在声明处
- 其他类，类成员函数，全局函数都可声明为友元
- 友元函数不是类的成员，不存在this指针
- 友元函数可以访问对象任意成员属性，包括私有属性

```c++
class BUilding
{
    //声明一个Building类的友元函数
    friend void GoodGay(Building &bd);
    //同样可以声明一个类的友元类
    friend class Goodfriend;
    public:
    Building()
    {
        K="1111";
        W="2222";
    }
    public:
    string K
    private:
    string W;
};
void GoodGay(BUilding &bd)
{
    //访问公有成员：
    bd.K;
    //访问私有成员
    bd.W;
}
//通过传入参数来访问类的私有成员
void test()
{
    BUilding bd;
    GoodF f;
    f.func(bd);
}
//通过类内指针来访问类的私有成员
class GoodF2
{
    public :
    GoodF2()//构造函数申请空间
    {
        pbu=new Building
    }
    void func()
    {
        //访问
        pbu->K;
        pbu->W;
    }
    //防止浅拷贝，需要定义一个拷贝构造函数
    GoodF2(const GoodF2 &f2)
    {
        //1.申请空间
        pbu=new BUilding;
        //2.拷贝数据
        pbu->K= 
    }
    //使用完后，要用析构函数释放空间
    ~GoodF2()
    {
        if(pbu!=NULL)
        {
            delete pbu;
        }
    }
    public:
    Buildind *pbu;
}
    
```

#   ====2023/6/21/P76====

- ## 单例模式（***）

1.  单例模式是一个类只能实例化一个对象
2. 实现单例模式的思路：

- ​	把无参构造函数和拷贝构造函数私有化
- ​	定义一个类内的静态成员指针
- ​	在类外初始化时，new一个对象
- ​	把指针的权限设置为私有，然后提供一个静态成员函数让外面获取这个指针

```c++
class Maker
{
  //1.把构造函数私有化
   private:
    Maker()
    {
    }
    pubilc:
	static Maker* getMaker()
	{
    	return pMaker;
	}
    private:
    //2.定义一个类内的静态成员指针
    static Maker *pMaker;   
};

//3.在类外初始化时，new一个对象
Maker *Maker::pMaker=new Maker;//这里可以new是因为有Maker::作用域，编译器这时把他当成在类内
void test()
{
    Maker *m=Maker::getMaker();
    Maker *m2=Maker::getMaker();
}
```

```c++
//需求，获取打印机使用的次数
class Printer
{
    //1.私有化无参构造和拷贝构造
    Printer()
    {
        mcount 0;
    }
    printer(const Printer &p)
    public:
   	static Printer *getPrinter()
    {
        return p;
    }
    void printPrinter(string name)
    {
        cout<<name<<endl;
        mcount++;
    }
    int getCount()
    {
        return mcount;
    }
    private :
    int mcount;//记录打印机的次数
    //2.定义静态成员指针
    static Printer *p;
};
//3.类外进行初始化，new对象
Printer *Printer::p=new Printer;

void test()
{
    //Sell
    Printer *p1=Printer::getPrinter();
    p1->printPrinter("Sell");
    //....
    Printer *pc=Printer::getPrinter();
    cout<<"使用次数"<<p4->getCount()<<endl;
}
```

## 数组类的设计

```c++
class MyArray
{
    public:
    	MyArray();
    	MyArray(const MyArray &arr);
    	MyArray(int capacity,int val=0);   
    	~MyArray();
    	//头插法
    	void PushFront(int val);
    	//尾插
    	void PushBack(int val);
    	//获取数组元素个数
    	int Size();
    	//获取数组容量
    	int Capacirt();
    	//指定位置插入元素
    	void Insert(int pos,int val);
    	//获取指定位置的值
    	void 
    
    private:
    	int *pArray;//指向堆区空间，存储数据
    	int mSize;//元素个数
    	int mCapacity;//容量
};
```

# C++ 重载运算符和重载函数

C++ 允许在同一作用域中的某个**函数**和**运算符**指定多个定义，分别称为**函数重载**和**运算符重载**。

重载声明是指一个与之前已经在该作用域内声明过的函数或方法具有相同名称的声明，但是它们的参数列表和定义（实现）不相同。

当您调用一个**重载函数**或**重载运算符**时，编译器通过把您所使用的参数类型与定义中的参数类型进行比较，决定选用最合适的定义。选择最合适的重载函数或重载运算符的过程，称为**重载决策**。

## 运算符重载

1. 运算符重载，就是对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型。（不能改变其基础类型的寓意）
2. 运算符重载的目的是让语法更加简洁。 
3. 运算符重载的本质是一种函数调用的方式（编译器调用）
4. 这个函数名统一为**operator**
5. 重载函数可以写成全局函数或重载函数。
6. 几乎c中所有的运算符都可以被重载，但是运算符重载的使用是有限制的。特别是不能使用C中当前没有意义的运算符（例如**求幂）不能改变运算符优先级，不能改变运算符的参数个数。
6. 重载函数如果写成全局的，那么双目运算符左边的是第一个参数，右边是第二个参数
6. 重载函数如果写成成员函数，那么双目运算符的左边是this，右边是第一个参数
6. 不能改变运算符优先级，不能改变运算符的参数个数

### ”+“运算符重载

```c++
class Maker
{
    public:
    Maker(int id,int age)
    {
        this->id=id;
        this->age=age;
    }
    //写成成员函数，只需要一个参数(加号的右边),坐标的参数由this指向
    Maker operator+(Maker &m2)
    {
        Maker temp(this->id+p2.id,this->age+p2.age);
    	return temp;
    }
    public:
    int id;
    int age;
}
//全局方式。由编译器调用
Maker operator+(Maker &p1,Maker &p2)//3.编译器检查参数是否对应，第一参数的加的左边，第二个是加的右边
{
    Maker temp(p1.id+p2.id,p1.age+p2.age);
    return temp;
}
void test01()
{
    Maker m1(1,20);
    Maker m2(2,20);
    Maker m3=m1+m2;//1.编译器看到两个对象相加，就回去找有没有operator+的函数。
}
```

### "<<"和">>"运算符重载

```c++
//1.形参和实参是一个对象
//2.不能改变类库中的代码
//3.ostream中把拷贝构造函数私有化了
//4.如果要和endl一起使用，那么必须返回ostream的对象
ostream& operator<<(ostream &out,Maker &m)
{
    cout<<m.id<<""<<m.name<<endl;
    return out;
}
```

### 赋值运算符重载（**）

1. 编译器默认给类提供了一个默认的赋值运算符重载函数
2. 默认的赋值运算符重载函数进行了简单的赋值操作。
3. 当类有成员指针时，然后在构造函数中申请堆区空间，在析构函数中释放堆区空间。

```c++
class Student
{
    public:
    Student(const char *name)
    {
        pName=new char[strlen(name)+1];
        strcpy(pName,name);
    }
    //防止浅拷贝
    Student(const Student &stu)
    {
        pName=new char[strlen(stu.pName)+1];
        strcpy(pName,stu.pName);
    }
    //重写赋值运算符重载函数
    void &operator=(const Student&stu)
    {
        //1.不能确定this->pName指向的空间是否能装下stu中的数据，所以先释放this->pName指向的空间
        if(this->pName !=NULL)
        {
            delete[] this->pName;
            this->pName=NULL;
        }
        //2.申请堆区空间，大小由stu决定
         this->pName=new char[strlen(stu.pName)+1];
        //3.拷贝数据
        strcpy(this->pName,stu.pName);
        //4.返回对象本身
        return *this; 
    }
    ~Student()
    {
        if (pName!=NULL)
        {
            delete[] pName;
            pName=NULL;
        }
    }
}
//赋值运算重载中要返回引用
void test()
{
    Student s1("a");
    Student s2("b");
    Student s3("c");
    
    s1=s2=s3;
}
```

### 前置++和后置++

```c++
//重载前置++
Maker &operator++()
{
    ++this->a;
    return *this;
}
//后置++
Maker operatpr++(int)//占位参数必须是int
{
    //后置++，先返回，后++
    Maker tmp(*this);//1.*this里面的值a=2
    ++this->a;//a=3
    r
}
```

## 智能指针类（**）

1. 智能指针类是管理另一个类的对象的释放

   ```c++
   class Maker
   {
       public:
       Maker()
       {
       }
       void printMaker()
       {
       }
       ~Maker()
       {}
   };
   class SmartPoint
   {
       public:
       SmartPoint(Maker *m)
       {   
           this->pMaker=m;
       }
       //重载指针运算符
       Maker *operator->
       ~SmartPoint()
       {
           if (this->pMaker !=NULL)
           {
               delete this->pMaker;
           }
       }
       private:
       Maker *pMaker;
   };
   void test()
   {
       Maker *p=new Maker;
       SmartPoint sm(p);//栈区，会调用析构函数
       //当test()函数结束时，会调用SmartPoint的析构函数，
       //在这析构函数中deletelMaker的对象，会调用Maker的析构函数
   }
       
   ```

   ```c++
   //一个类如果重载了函数调用符号，那么这个类实例化出的对象也叫仿函数
   //仿函数的作用：1.方便代码维护 2.方便有权限的调用函数。3.z
   class Maker
   {
       public:
       Maker()
       {
       }
       void printMaker()
       {
       }
       void operator()()
       {
           
       }
       public:
       string name;
   };
   void test()
   {
       Maker func;
       func();//func为对象
   }
   ```

   ## 重载函数调用符号（**）
   
   1. 类里有重载函数调用符号的类实例化的对象也叫仿函数
   2. 方便有权限的调用函数
   3. 作为算法的策略
   
   ## 其他重载
   
   ```c++
   class Maker
   {
       public:
       Maker()
       {
           a=0;
       }
       void SetA(int val)
       {
           a=val;
       }
       //没有返回值，也没有void
       operator bool()
       {
           if(a<=0)
           {
               return false;
           }
           return ture;
       }
       
   void test()
   {
       Maker func;
       func();//func为对象
   }
   ```
   
   ## C++ 继承
   
   ### 基类 & 派生类
   
   一个类可以派生自多个类，这意味着，它可以从多个基类继承数据和函数。定义一个派生类，我们使用一个类派生列表来指定基类。类派生列表以一个或多个基类命名，形式如下：
   
   ```c++
   class derived-class: access-specifier base-class
   ```
   
   其中，访问修饰符 access-specifier 是 **public、protected** 或 **private** 其中的一个，base-class 是之前定义过的某个类的名称。如果未使用访问修饰符 access-specifier，则默认为 private。
   
   ### 访问控制和继承
   
   派生类可以访问基类中所有的非私有成员。因此基类成员如果不想被派生类的成员函数访问，则应在基类中声明为 private。
   
   我们可以根据访问权限总结出不同的访问类型，如下所示：
   
   | 访问     | public | protected | private |
   | :------- | :----- | :-------- | :------ |
   | 同一个类 | yes    | yes       | yes     |
   | 派生类   | yes    | yes       | no      |
   | 外部的类 | yes    | no        | no      |
   
   一个派生类继承了所有的基类方法，但下列情况除外：
   
   - 基类的构造函数、析构函数和拷贝构造函数。
   - 基类的重载运算符。
   - 基类的友元函数。
   
   ### 继承类型
   
   当一个类派生自基类，该基类可以被继承为 **public、protected** 或 **private** 几种类型。继承类型是通过上面讲解的访问修饰符 access-specifier 来指定的。
   
   我们几乎不使用 **protected** 或 **private** 继承，通常使用 **public** 继承。当使用不同类型的继承时，遵循以下几个规则：
   
   - **公有继承（public）：**当一个类派生自**公有**基类时，基类的**公有**成员也是派生类的**公有**成员，基类的**保护**成员也是派生类的**保护**成员，基类的**私有**成员不能直接被派生类访问，但是可以通过调用基类的**公有**和**保护**成员来访问。
   - **保护继承（protected）：** 当一个类派生自**保护**基类时，基类的**公有**和**保护**成员将成为派生类的**保护**成员。
   - **私有继承（private）：**当一个类派生自**私有**基类时，基类的**公有**和**保护**成员将成为派生类的**私有**成员。
   
   注：当子类和父类有同名成员时，子类的同名成员会隐藏父类的同名成员
   
   当子类和父类有同名函数时，父类的所有函数重载都会被隐藏

### 继承中的构造和析构

### 继承中的静态成员特性

1. 静态成员可以被继承
2. 继承的静态成员变量一样会被同名的子类成员变量隐藏
3. 继承的静态成员函数中，当子类有和父类同名静态函数时，父类的所有重载静态函数都会被隐藏
4. 改变继承的静态函数的某个特征，返回值或者参数个数，将会隐藏基类重载的函数
5. 静态成员函数不能是虚函数
6. 从父类继承过来的静态成员变量是父类的静态成员变量

### 多继承

多继承即一个子类可以有多个父类，它继承了多个父类的特性。

多继承的问题是，当父类中有同名成员时，子类中会产生二义性问题

C++ 类可以从多个类继承成员，语法如下：

```c++
class <派生类名>:<继承方式1><基类名1>,<继承方式2><基类名2>,…
{
<派生类类体>
};
```

其中，访问修饰符继承方式是 **public、protected** 或 **private** 其中的一个，用来修饰每个基类，各个基类之间用逗号分隔，

### 菱形继承和虚继承

![img](https://ask.qcloudimg.com/http-save/7525075/nsym2r57kx.png?imageView2/2/w/1200)

**菱形继承的概念：**两个不同的派生类继承于同一个父类，又有一个子类多继承于这个两个派生类。

**菱形继承的问题**：菱形继承主要有数据冗余和二义性的问题。由于最底层的派生类继承了两个基类，同时这两个基类有继承的是一个基类，故而会造成最顶部基类的两次调用，会造成数据冗余及二义性问题。

**解决方法：**利用虚继承，关键字virtual，被继承的基类叫虚基类。

**虚拟继承**可以解决菱形继承的二义性和数据冗余的问题。如上面的继承关系，在Student和Teacher的继承Person时使用虚拟继承，即可解决问题。需要注意的是，虚拟继承不要在其他地方去使用。

```c++
class Person
{
public :
    string _name ; // 姓名
};
class Student : virtual public Person
{
protected :
    int _num ; //学号
};
class Teacher : virtual public Person
{
protected :
    int _id ; // 职工编号
};
class Assistant : public Student, public Teacher
{
protected :
    string _majorCourse ; // 主修课程
};
void Test ()
{

    Assistant a ;
    a._name = "peter";
}
```

**虚继承的原理**

1. 编译给类添加了一个指针，指针指向类似于表的组织，该表记录了该指针距离变量的偏移量
2. 当子类多继承两个父类，那么只有一份成员变量，然后有两个指针，只有一份成员变量，所以不会产生二义性

```c++
void test()
{
    Sheep s;
    s.mA;
    //1.&s;
    //2.(int*)&s;强制转为int*类型
    //3.*(int*)*(int*)&s;//指向表的首地址
    //5.(int *)*(int*)&s+1;//指向了表存储偏移量的地址
    //6.*((int *)*(int*)&s+1)//
}
```

## 动态联编和静态静联编

### 1.静态联编

静态联编的决策策略是指，在对C++代码进行编译时，就进行地址和内存分配。使用静态联编的常见的地方就是申明一个变量：当声明某一类型的变量之后，编译器在编译相应源代码的时候，根据其数据类型分配内存空间和地址。如下代码所示，即使静态联编：

```c++
int a; // 在编译时，即确定好该变量的地址，并分配四字节内存（int为四字节）空间；
double b;// 同上
float c; // 同上
```

### 2.动态联编

  动态联编的决策策略是指，在运行C++程序时，根据代码逻辑才给变量分配内存和地址。常用的动态联编是给指针new一个动态地址：在申明一个指针变量之后，若不是直接指向已存在的变量，那么就是通过new动态分配内存和地址进行初始化。如下代码所示：

```c++
int *p = new int;
*p = 10; // 如果没有上句代码后面的new int, 那么执行改代码就会出错
delete p; // 记得不再使用new分配的内存地址时，一定要用delete释放内存，否在将会造成内存泄漏
```

动态联编的优点很明显，就是程序员可以决定什么时候为指针分配内存地址，这样可以有效的利用内存空间。

优点，可以扩展功能，不修改前面的代码的基础上进行项目的扩充

### 3.虚函数

在运行阶段才确定调用哪个函数（晚绑定），在普通成员函数前面加virtual,该函数变为虚函数，告诉编译器要晚绑定。

### 4.类型转换问题

子类转换成父类（向上转换）：编译器认为指针的寻址范围缩小了，所以是安全的

父类转换成子类（向下转换）：编译器认为指针的寻址范围扩大了，不安全

## 继承中同名成员的处理方法

1.通过类名加作用域来访问，当子类和父类有同名成员时，子类的同名成员隐藏父类的同名成员

## C++ 多态

**多态**按字面的意思就是多种形态。当类之间存在层次结构，并且类之间是通过继承关联时，就会用到多态。C++ 多态意味着调用成员函数时，会根据调用函数的对象的类型来执行不同的函数。下面的实例中，基类 Shape 被派生为两个类，如下所示：

什么是多态：同一个操作作用于不同的对象，可以有不同的解释，会产生不同的效果

### 作用：

### 多态的构成条件

**C++的多态必须满足两个条件：**

1 必须通过基类的指针或者引用调用虚函数
2 被调用的函数是虚函数，且必须完成对基类虚函数的重写

（三个条件）

1.有继承，2.重写父类的虚函数，3.父类指针指向子类对象

```c++
class People
{
    public:
    	virtual void Mypro()
        {
            
        }
};
class A :public People
{
    public:
    //重写父类的虚函数
    	virtual void Mypro()
        {
            
        }
};
//同一个操作
void doLogin(People *pro)
{
    pro->Mypro();
}
void test()
{
    People *pro=NULL;
    pro=new A;
    doLogin(pro);//不同的对象
    delete pro;
    
    pro=new B;
    doLogin(pro);//不同的对象
    delete pro;
    
    pro=new C;
    doLogin(pro);//不同的对象
    delete pro;
}
```

### 多态有什么用

1. 可以解决项目中的紧耦合问题，提供程序的可拓展性
2. 应用程序不必为每一个实现子类的功能调用编写代码，只需要对抽象的父类进行处理

### 多态的实现原理

1. 编译器只要发现类中有虚函数，编译器就会创建一张表，表中存放类中所有虚函数的入口地址
2. 编译器会在类对象中安插一个虚函数表指针，虚函数表指针指向本类的虚函数表。
3. 虚函数表指针是从父类继承下来的，虚函数表指针指向父类的虚函数表。编译器为了初始化从父类继承的虚函数表指针，编译器在我们所有构造函数中添加了初始化虚函数指针的代码，让从父类继承过来的虚函数表指针指向子类自己的虚函数表
4. 当编译器发现子类重写了父类的虚函数，那么子类重写的函数就会覆盖掉虚函数表对应的父类的函数

[p118]

## 纯虚函数和抽象类

### 1.依赖倒置

1. **依赖于具体抽象（接口），不依赖于具体的实现，也就是针对接口编程。**

2. **实现高层业务和实现层、实现层和实现层之间的解耦合；**

依赖倒置原则定义：依赖于抽象(接口)，不要依赖具体的实现(类)，也就是针对接口编程。

依赖倒置原则的图解。

![这里写图片描述](https://img-blog.csdn.net/20171017110833455?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjEwNzg1NTc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

```c++
//抽象层
//有纯虚函数的类叫抽象类，不能实例化对象
class rule
{
    public：
        //纯虚函数
        virtual int getnum(int a,int b)=0;
    {
        return a+b;
    }
};
//实现层
class plus_rule:public rule
{
    public:
    	virtual int getumr(int a,int b)//重写父类的虚函数，依赖抽象层
        {
            return a+b;
        }
};
//业务层
int doLogin(rule *)
{
    int a=10;
    int b=20;
    int ret=cal->getnum(a,b);
    return ret;
}

void test()
{
    rule *r=NULL;
    r=new plus_rule;
    //抽象类不能实例化对象
    
}
```

### 2.开闭原则

对修改源码关闭，对拓展新功能开放

### 3.纯虚函数

### 4.抽象类

1. 有纯虚函数的类叫抽象类，不能实例化对象
2. 如果子类继承抽象类，子类必须实现抽象类的所有纯虚函数，不然子类也编程抽象类

## 接口类的定义

所谓的接口，即将内部实现细节封装起来，外部用户用过预留的接口可以使用接口的功能而不需要知晓内部具体细节。c++中，通过类实现面向对象的编程，而在基类中只给出纯虚函数的声明，然后在派生类中实现纯虚函数的具体定义的方式实现接口，不同派生类实现接口的方式也不尽相同。

```c++
//抽象类
class Father
{
    public:
    	virtual void fun1()=0;//接口的声明
    	virtual void fun2(int a)=0;
    	virtual void fun3(int a,int b)=0;  
};
//继承并实现
class Son:public Father
{
    public:
    //接口的实现
    	virtual void fun1(){
            
        }
    	virtual void fun2(int a){
            
        }
    	virtual void fun3(int a,int b){
            
        }  
};
```

## 模板方法模式

1.在抽象类中确定好函数的调用顺序

```c++
class Drink{
  public:
    virtual void funA()=0;
    virtual void funB()=0;
    virtual void funC()=0;
    virtual void funD()=0;
    
    //模板方法
    void fun(){
        //确定调用函数的顺序
        funA();
        funB();
        funC();
        funD();
    }
};
class Coffee:public Drink{
    virtual void funA(){
        
    }
    virtual void funB(){
        
    }
    virtual void funC(){
    
    }
    virtual void funD(){
        
    }
};
class Tea: public Drink{
    virtual void funA(){
        
    }
    virtual void funB(){
        
    }
    virtual void funC(){
    
    }
    virtual void funD(){
        
    }
}
void test(){
    Drink *dr=NULL;
    dr=new funA();//父类指针指向子类对象
    dr->fun();
    delete dr; 
}
```

## 虚析构函数和纯虚析构函数

### 1.虚函数：

#### 声明方式：

```cpp
virtual 返回值类型 成员函数名(形参表);
```

#### 作用：

虚函数用于实现多态性

### 2.虚析构函数

#### 声明方式：

```cpp
virtual ~类名(){
    
};
```

#### 作用

1. 将派生类的对象赋值给基类的指针变量的时候，指针变量的指针指向的指针只是指向的派生类中的基类部分，从而系统只会执行基类的析构函数，而不执行派生类的析构函数。
2. 虚构函数是为了解决基类指针指向派生类对象，并用基类指针释放子类对象

### 3.纯虚析构函数

有纯虚析构函数的类是抽象类，不能实例化对象。纯虚函数没有函数体，不能被调用，类似于java中的抽象方法；当我们考虑看到派生类中需要某个函数但是暂时又不能实现的时候，可以将其设置为虚函数，让具体功能在派生类中去实现。

注：**纯虚析构函数需要在类外实现**

#### 声明方式

```cpp
virtual 返回值类型 函数名(形参表)=0;
```

```c++
//在类外实现纯虚析构函数
Animal::~Animal(){
    
}
```

**[p123]**

## 重写重载重定义

1. 重载：（同一作用域的同名函数）函数名相同，函数的参数个数、参数类型或参数顺序三者中必须至少有一种不同。函数返回值的类型可以相同，也可以不相同。发生在一个类内部，不能跨作用域。

2. 重定义：也叫做隐藏，子类重新定义父类中有相同名称的非虚函数 ( 参数列表可以不同 ) ，指派生类的函数屏蔽了与其同名的基类函数。可以理解成发生在继承中的重载。
   1. 有继承
   2. 子类（派生类）重写父类（基类）的virtual函数
3. 重写：也叫做覆盖，一般发生在子类和父类继承关系之间。子类重新定义父类中有相同名称和参数的虚函数。(override)
   1. 有继承
   2. 子类（派生类）重写父类（基类）的virtual函数
   3. 函数返回值，函数的名字，函数参数，必须和基类中的虚函数一致

## 父类引用子类对象

### 动态多态

动态多态的实现是通过子类重写父类的[虚函数](https://so.csdn.net/so/search?q=虚函数&spm=1001.2101.3001.7020)实现的。

**动态多态需要满足的条件：**

- 有继承关系
- 子类重写父类中的虚函数

**动态多态的使用方法：**

- **父类指针或引用指向子类对象**

假设我们现在有如下的类，其中Animal为基类，Cat和Dog都是其派生类。

```c++
class Animal{
public:
	virtual void speak(){
		cout << "动物在说话" << endl;
	}
};
 
class Cat :public Animal{
public:
	void speak(){
		cout << "小猫在说话" << endl;
	}
};
 
class Dog :public Animal{
public:
	void speak(){
		cout << "小狗在说话" << endl;
	}
 
};
```

#### 1.父类指针指向子类对象

调用时，可以定义父类的指针，然后指向子类的对象，指向哪个对象，便执行其对象的虚函数实现。

```cpp
int main() {
	Dog B;
	Animal *A=&B;
	A->speak();
	
	A=new Cat;
	A->speak();
	
	return 0;
}
```

#### 2.父类引用指向子类对象

这种实现，我们需要额外定义一个“实现函数”，其参数是**基类的引用**（否则不能实现多态）。这样在调用时就实现了父类引用指向子类对象。

```cpp
void DoSpeak(Animal & X){ //
	X.speak();
}
 
int main() {
	
	Cat A;
	DoSpeak(A);
 
	Dog B;
	DoSpeak(B);
	
	Animal C;
	DoSpeak(C);
	
	return 0;
}
```

#### 为什么父类指针可以指向子类？

可以通俗的理解，子类可能含有一些父类没有的成员变量或者方法函数，但是子类肯定继承了父类所有的成员变量和方法函数。
所以用父类指针指向子类时，没有问题，因为父类有的，子类都有，不会出现非法访问问题。但是如果用子类指针指向父类的话，一旦访问子类特有的方法函数或者成员变量，就会出现非法。虽然父类指针可以指向子类，但是其访问范围还是仅仅局限于父类本身有的数据，那些子类的数据，父类指针是无法访问的。

## C++ 模板

c++提供了函数模板（function template）所谓的函数模板，实际上是建立一个通用函数，其函数类型和形参类型不具体指定，用一个虚拟的类型来代表。这个通用函数就成为函数模板。凡是函数体相同的函数都可以用这个模板代替，不必定义多个函数，只需要在模板中定义一次即可。在调用函数时系统会根据实参的类型来取代模板中的虚拟类型，从而实习不同函数的功能。

1. c++提供两种模板机制：函数模板和类模板
   1. 函数模板针对仅参数类型不同的函数；
   2. 类模板针对仅数据成员和成员函数类型不同的类。
2. 类属-类型参数化，又称参数模板

总结：

1. **模板把函数或类要处理的数据类型参数化，表现为参数的多态性，成为类属**
2. **模板用于表达逻辑结构相同，但具体数据元素类型不同的数据对象的通用行为**

### 一、函数模板通式

**1、函数模板的格式：**

```c++
template <class 形参名，class 形参名，......> 返回类型 函数名(参数列表)
{
    函数体
}
```

其中template和class是关见字，class可以用typename 关见字代替，在这里typename 和class没区别，<>括号中的参数叫模板形参，模板形参和函数形参很相像，模板形参不能为空。一但声明了模板函数就可以用模板函数的形参名声明类中的成员变量和成员函数，即可以在该函数中使用内置类型的地方都可以使用模板形参名。模板形参需要调用该模板函数时提供的模板实参来初始化模板形参，一旦编译器确定了实际的模板实参类型就称他实例化了函数模板的一个实例。比如swap的模板函数形式为:

```c++
//T代表泛型的数据类型，不是只能写T，
template <class T> //让编译器看到这句话后面紧跟着的函数里有T不要报错
void mySwap(T& a, T& b){
    T tmp=a;
    a=b;
    b=tmp;
}
//使用函数模板
void test()
{
    int a=10;
    int b=20;
    //编译器会根据实参来自动推导数据类型
    //编译器在函数模板被调用时，进行第二次编译
    mySwap(a,b);//数据类型要保持一致
    //2.显示指定类型
    mySwap<int>(a,b);//<>用参数列表告诉编译器只传int类
     
}
```

当调用这样的模板函数时类型T就会被被调用时的类型所代替，比如**swap(a,b)**其中**a**和**b**是**int** 型，这时模板函数swap中的形参**T**就会被**int** 所代替，模板函数就变为**swap(int &a, int &b)**。而当**swap(c,d)**其中**c**和**d**是**double**类型时，模板函数会被替换为**swap(double &a, double &b)**，这样就实现了函数的实现与类型无关的代码。

2、注意：对于函数模板而言不存在 **h(int,int)** 这样的调用，**不能在函数调用的参数中指定模板形参的类型，对函数模板的调用应使用实参推演来进行**，即只能进行 **h(2,3)** 这样的调用，或者**int a, b; h(a,b)**。

### 隐式转换

```c++
template <class T> 
T func(T a, T b){
   return a+b;
}
void test()
{
    int a=12;
    double b=20.1;
    //如果使用参数列表指定数据类型，那么实参中可以隐式转换，
    cout<<func<int>(10,20.1)<<endl;
}

```

### 二、类模板通式

1、类模板的格式为：

```c++
template<class  形参名，class 形参名，…>   class 类名
{ ... };
```

类模板和函数模板都是以template开始后接模板形参列表组成，模板形参不能为空，一但声明了类模板就可以用类模板的形参名声明类中的成员变量和成员函数，即可以在类中使用内置类型的地方都可以使用模板形参名来声明。比如

```c++
template<class T> class A{public: T a; T b; T hy(T c, T &d);};
```

在类A中声明了两个类型为T的成员变量a和b，还声明了一个返回类型为T带两个参数类型为T的函数hy。

2、类模板对象的创建：比如一个模板类**A**，则使用类模板创建对象的方法为**A<int> m;**在类**A**后面跟上一个**<>**尖括号并在里面填上相应的类型，这样的话类**A**中凡是用到模板形参的地方都会被**int** 所代替。当类模板有两个模板形参时创建对象的方法为**A<int, double> m;**类型之间用逗号隔开。

3、对于类模板，模板形参的类型必须在类名后的尖括号中明确指定。比如**A<2> m;**用这种方法把模板形参设置为**int**是错误的（**编译错误：error C2079: 'a' uses undefined class 'A<int>'**），**类模板形参不存在实参推演的问题。**也就是说不能把整型值**2**推演为**int** 型传递给模板形参。要把类模板形参调置为**int** 型必须这样指定**A<int> m**。

4、在类模板外部定义成员函数的方法为：

```c++
template<模板形参列表> 函数返回类型 类名<模板形参名>::函数名(参数列表){函数体}，
```

比如有两个模板形参T1，T2的类A中含有一个void h()函数，则定义该函数的语法为：

```c++
template<class T1,class T2> void A<T1,T2>::h(){}。
```

**注意：**当在类外面定义类的成员时template后面的模板形参应与要定义的类的模板形参一致。

5、再次提醒注意：模板的声明或定义只能在全局，命名空间或类范围内进行。即不能在局部范围，函数内进行，比如不能在**main**函数中声明或定义一个模板。

## 普通函数和模板函数的调用规则

```c++
//普通函数
void myPlus(int a,int b)
{
    return a+b;
}

template<class T>
void myPlus(T a,T b)
{
    return a+b;
}
//1.函数模板和普通函数可以重载
void test()
{
    int a=10;
    int b=20;
    //2.如果普通函数和函数模板都可以实现的功能，普通函数优先调用
    myPlus(a,b);
    //3.可以使用<>空参数列表强制调用函数模板
    myPlus<>(a,b);
    //4.函数模板之前可以进行重载
    
    //5.如果函数模板可以产生更好的匹配，那么优先使用函数模板
    char c1='a';
    char c2='b';
    myPlus(c1,c2);
}
```

## 函数模板机制结论：

- 编译器并不是把函数处理成能够处理任何类型的函数
- 函数通过具体类型产生不同的函数
- 编译器会对函数模板进行两次编译，在声明的地方模板代码本身进行编译，在调用的地方对参数替换后的代码进行编译。

## 模板的局限性及局限性解决方法

1.模板的局限性：编写的函数模板很可能无法处理某些类型，另一方面，有时通用化是有意义的，但C++语法不允许这样做。为了解决这种问题，可以提供模板的重载，为这些特定的类型提供具体化的模板。

```c++
class Maker
{
    public:
    Maker(string name,int age){
        this->age=age;
        this->
	}
}
```

## 模板的特化

为了解决这种问题，可以提供模板的重载，为这些特定的类型提供具体化的模板，称为模板的特化，模板特化有时也称之为模板的具体化。

```c++
#include<iostream>
#include <string>
using namespace std;

class Person{
public:
	Person(string name, int age){
		this->m_Name = name;
		this->m_Age = age;
	}
	string m_Name;
	int m_Age;
};

template<class T>
bool myCompare( T &a , T &b ){
	if ( a == b){
		return true;
	}
	return false;
}
// 通过模板特化自定义数据类型，解决上述问题
// 如果具体化能够优先匹配，那么就选择具体化
// 语法  template<> 返回值  函数名<具体类型>(参数) 

template<> 
bool myCompare<Person>(Person &a, Person &b){
	if ( a.m_Age  == b.m_Age){
		return true;
	}
	return false;
}

void test01(){
	int a = 10;
	int b = 20;
	int ret = myCompare(a, b);//普通模板
	cout << "ret = " << ret << endl;
	
	Person p1("Tom", 10);
	Person p2("Jerry", 10);

	int ret2 = myCompare(p1, p2);//特化模板
	cout << "ret2 = " << ret2 << endl;
}
int main(){
	test01();
	return EXIT_SUCCESS;
}

```

