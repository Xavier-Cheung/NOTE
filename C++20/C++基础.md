### 一些小知识点
---


## C++标准库速成
- c++20支持**import**导入模块  
 
- main函数要么没有参数，要么具有两个参数
```c++
int main(int argc, char* argv[])
```
>argc给出了传递给程序的**实参数目**，argv**包含了这些实参**。注意：argv[0]可能是程序的名称也可能是空字符串，所以不应该使用它【***使用时索引从1开始***】
- 格式化字符串可以使用`std::format("字符串{}", 填入)`
- 名称空间：用来处理不同代码段之间的名称冲突问题，可指定定义名称的环境
>>为将某段代码加入命名空间，可以使用namespace块将其包含，例子：
  ```c++
  namesapce mycode{
    void foo() {
        std::cout << "nadondoand" << endl;
    }
  }
  ```
  >>可使用**using**指令预先避免指明地址空间，例子：
  ```c++
  import<iostream>
  using namespace mycode;
  int main()
  {
    foo();//这里调用的是mycode空间内的foo函数
  }
  ```
  >>也可以使用**using**指令应用命名空间内的特定项，例子：
  ```c++
  using std::cout;
  cout << "hello" << std::endl;
  ```
  >>名称空间可嵌套
  名称空间可设置别名

- 字面量：用于表达源代码中一个**固定值**的表示法
>C++14后可使用数字分隔符，例子：
```C++
123`456`789
//浮点数，八进制，十六进制都可用
```

- 变量初始化
```c++
  int a = 1;
  int a {1};//新方法，其他类型同理
```

- 数值极限
>推荐使用`include<limits>`中的类模板`std::numeric_limts`,例子：
```c++
  cout << numeric_limits<int>::max() << endl;//min(), lowest()
```

- 零初始化：用{}进行零初始化
```c++
int a {};//0
float f {};//0.0
int* ptr {};//nullptr
```
- 类型转换
```c++
  float f {3.14f};
  int a {(int) f};//旧方法C语言
  int b {int (f)};
  int c {static_cast<int> (f)};//新方法，推荐
```

- 浮点型数字
>+/-infinity 表示正/负无穷
NAN表示非数字
`std::isnan()`判断给定的浮点数是否为数字
`std::isinf()`判断给定的浮点数是否为无穷
---
#### 枚举类型
>新：
```c++
//声明
  enum class Piece
  {
    king = 1,
    Queen,
    Rook = 10,
    Pawn
  };
  //使用
  Piece one {Piece::King};
  Piece one = Piece::King;
```
>旧：
```c++
  enum color_set1 {RED, BLUE, WHITE, BLACK}; // 定义枚举类型color_set1
  enum week {Sun, Mon, Tue, Wed, Thu, Fri, Sat}; // 定义枚举类型week
  ```
  >枚举常量只能以标识符形式表示，而不能是整型、字符型等文字常量。例如，以下定义非法：
  ```c++
  enum letter_set {'a','d','F','s','T'}; //枚举常量不能是字符常量
  enum year_set{2000,2001,2002,2003,2004,2005}; //枚举常量不能是整型常量
  //可改为以下形式则定义合法：

  enum letter_set {a, d, F, s, T};
  enum year_set{y2000, y2001, y2002, y2003, y2004, y2005};
```
>枚举常量代表该枚举类型的变量可能取的值，编译系统为每个枚举常量指定一个整数值，默认状态下，这个整数就是所列举元素的序号，序号从0开始。 可以在定义枚举类型时为部分或全部枚举常量指定整数值，在指定值之前的枚举常量仍按默认方式取值，而指定值之后的枚举常量按依次加1的原则取值。 各枚举常量的值可以重复。例如：
```c++
  enum fruit_set {apple, orange, banana=1, peach, grape}
  //枚举常量apple=0,orange=1, banana=1,peach=2,grape=3。
  enum week {Sun=7, Mon=1, Tue, Wed, Thu, Fri, Sat};
  //枚举常量Sun,Mon,Tue,Wed,Thu,Fri,Sat的值分别为7、1、2、3、4、5、6。
```
>定义格式：定义枚举类型之后，就可以定义该枚举类型的变量，如：`color_set1 color1, color2;`
  亦可类型与变量同时定义（甚至类型名可省），格式如下：
  `enum {Sun,Mon,Tue,Wed,Thu,Fri,Sat} weekday1, weekday2;`

  >枚举变量的值只能取枚举常量表中所列的值，就是整型数的一个子集。
  枚举变量占用内存的大小与整型数相同。
  枚举变量只能参与赋值和关系运算以及输出操作，参与运算时用其本身的整数值。例如，设有定义：

  ```c++
  enum color_set1 {RED, BLUE, WHITE, BLACK} color1, color2;
  enum color_set2 { GREEN, RED, YELLOW, WHITE} color3, color4;
  ```
  >则允许的赋值操作如下：
  ```c++
  color3=RED;           //将枚举常量值赋给枚举变量
  color4=color3;        //相同类型的枚举变量赋值，color4的值为RED
  int  i=color3;        //将枚举变量赋给整型变量，i的值为1
  int  j=GREEN;         //将枚举常量赋给整型变量，j的值为0
  
  //允许的关系运算有：==、<、>、<=、>=、!=等，例如：
  //比较同类型枚举变量color3，color4是否相等
  if (color3==color4) cout<<"相等"；
  //输出的是变量color3与WHITE的比较结果，结果为1
  cout<< color3<WHITE;
  //枚举变量可以直接输出，输出的是变量的整数值。例如：

  cout<< color3;         //输出的是color3的整数值，即RED的整数值1
  ```
  >枚举值得基本类型可以这样改变：
  ```c++
  enum piece : long long
  {
    a,
    b
  };
  ```
  >不同的强类型枚举可以拥有相同名字的枚举值**必须使用枚举值得全名**【C++20可以使用`using enum`来避免使用全名】

- export 用来导出模块  
---  
#### if 语句的初始化器
- C++17后可以有 if 语句的初始化器**初始化部分只在<>块内有效**
```c++
if(<初始化部分>; <if条件语句>) {
  <body>
}
else if(<条件>) {
  <body>
}
else {
  <body>
}
```
- switcht 同上
```c++
switch(<初始化>;<>)
{
  ...
}
```
>`[[fallthrough]];`的使用
---
#### 三向比较运算符【太空飞船运算符】
>可用于确定两个值得大小顺序
返回枚举类型“enumeration-like”，定义在compare库中
>>如果是整数类型，则结果是所谓的强排序:【设第一个操作数为a，第二个为b】
`strong_ordering::less      //a < b` 
`strong_ordering::greater   //a > b`
`strong_ordering::equal     //a == b`
  
>>如果操作数是浮点型，则结果是一个偏序：
`partial_ordering::less     //a < b`
`partial_ordering::greater     //a < b`
`partial_ordering::equivalent     //a < b`
`partial_ordering::unordered     //至少有一个数字NAN`

>>还有一种弱排序，针对自己实现的类型进行三向比较
`weak_ordering::less`
`weak_ordering::greater`
`weak_ordering::equivalent`

>>>使用示例
```c++
using namespace string_ordering;
int i {11};
strong_ordering result {i <=> 0};
if(result == less) cout << "less" << endl;
if(result == greater) cout << "greater" << endl;
if(result == equal) cout << "equal" << endl;
```
---
#### 函数
- 可以用auto代替函数的返回类型，让编译器自动推断出来
- 每个函数都有一个预定义的局部变量`__func__`,其中包含当前函数的名称，这个变量的一个用途是用于日志记录：**【注意：前后都是两个短横线】**
  ```c++
  int add(int n1, int n2) {
    cout << format("Entering function {}", __func__) << endl;
    return n1 + n2;
  }
  ```
---
#### 属性
>C++11后使用`[[artribute]]`表示
几个函数上下文中的标准属性：
>>1. `[[nodiscard]]`
>>>可用于有一个返回值的函数，使调用时没对返回值进行任何处理时发出警告
***C++20开始可以在`[[nodiscard("")]]`中的括号中添加一个字符串来说明原因***
>>2. `[[maybe_unused]]`
>>>可用于禁止编译器在未使用某些内容时发出警告，例如未使用函数中的某参数时
>>3. `[[noreturn]]`
>>>意味着该函数永远不会将控制权返回给调用点，克避免这种警告
>>4. `[[deprecated]]`
>>>将某些内容标记为**已弃用**，不鼓励使用，使用后会发出警告，***也可以添加字符串说明弃用原因***
>>5. `[[likely]]`和`[[unlikely]]`
>>>帮助编译器优化代码，标记分支的可能性
---
#### std::array
>有固定大小，`array<int, 3> arr{5, 6, 7}`,支持CTAD
`array arr{5, 6, 7}`
---
#### std::vector
>动态的，`vector<int> arr {2, 3}`，同样支持CTAD
`vector arr {2, 3}`【必须初始化】
用push_back()方法添加元素
---
#### std::pair
>将两个；类型的值组合在一起，可通过**first**和**second**访问这些值
`pair<double, int> mypair {1.5, 6}`，支持CTAD
`pair mypair {1.5, 3}`
---
#### std::optional
>保留特定的值，或不包含任何值

- 结构化绑定：允许声明多个变量，这些变量可以使用数组，结构体，pair等中的元素进行初始化,**两边数量必须相同**
```c++
  array arr {1, 2, 3};
  auto [x, y, z] {arr};
```
---
#### 循环
>基于范围的 for 循环【可用于数组、列表、具有返回迭代器的begin(),end()方法的类型】
```c++
  array arr {1, 2, 3, 4};
  for(int i : arr) {cout << i << endl;}
```
>C++20 中的 基于范围的for循环的初始化器，同 if 和 switch
```c++
  for(<initializer>; <for-range-declaration> : <for-range-initializer>) { <body> } 
```
---
#### 初始化列表
>在<initializer_list>头文件中， 可轻松编写能够接受可变数量参数的函数
```c++
import<initializer_list>
using namespace std;

int makesum(initializer_list<int> values) {
  for(int total = 0; int value : values)
    total += value;
  return total;
}

int main()
{
  cout << makesum({1,2,3}) << endl;
  int a {makesum({1,2,3})};
  cout << "a = " << a << endl;
  return 0;
}
```
---
#### 统一初始化【鼓励使用】
```c++
  int a = 3;
  int a {3};
  int a (3);
  int a = {3};
```
>指派初始化器的使用，对某些默认值不需初始化可使用
```c++
Employee one {
  .firstname = 'J';
  .lastname = 'D';//用.加上数据成员的名称
}
```
---
#### 指针和动态内存
1. 栈(stack)和自由存储区(free store)
2. 使用指针
    >`int *myptr = nullptr;`
    `int *myptr = new int;`
    **使用完后要释放内存：**
    `delete myptr;`
    `myptr = nullptr;`
    **注意！一定要delete后设置指针为空，因为delete只是释放这块内存空间，并没有删除该指针，删除后，该指针变成 '野指针' ，之后程序还会调用已经释放的存储空间，导致野指针也指向要操作的内存空间**[C++释放内存](https://blog.csdn.net/qq_36570733/article/details/80043321)

3. 动态分配的数组
    ```c++
      int size {8};
      int *arr {new int[size]}
      arr[1] = 2;
      delete[] arr;
      arr = nullptr;
    ```

4. 空指针常量 **NULL** 和 **nullptr** 的区别，nullptr是真正的空指针，NULL是空常量，NULL==0。
---
#### const的用法
1. **const直接作用于其直接左侧的内容**
   `int const *p {new int[3]};//数组的值不能变`
   `int *const p {new int[3]};//指针的值不能变`
   `int const * const p {new int[3]};//数组和指针的值不能变`

2. const方法
   >可以将类的方法标记为const，使该方法不能改变数据成员的值

3. **constexpr** 关键字
   >区别 "常量" 和 "常量表达式"
    ```c++
    const int getsize() {return 3;}
    int main()
    {
      int arr[getsize();]//这将编译失败
      return 0;
    }

    constexpr const int getsize() {return 3;}
    int main()
    {
      int arr[getsize();]//这将可以通过编译
      return 0;
    }
    ```

4. **consteval** 关键字【C++20】
   >保证始终在编译期对函数进行求值

---
#### 引用
>C++中的引用 (reference) 是另一个变量的别名，对引用的所有修改都会更改其引用的变量的值

1. 引用变量
    >- **引用变量必须在创建时被初始化**：
    ```c++
    int x { 3 };
    int& xRef { x };
    ```
    >- **修改引用：引用一旦创建便无法更改，更改引用时只会改变引用指向的变量的值**
      
    >- **const 引用**

    >- **指针的引用** 和 **引用的指针**
    取一个引用的地址和他引用的变量的地址是一样的
    注意：不能声明 **对引用的引用** 和 **引用的指针** 比如：`int& & 和 int& *`

    >- **结构化绑定和引用** 的使用

2. 引用数据成员
   >类的数据成员可以是引用，但是引用的初始化必须在**构造函数**中进行

3. 引用作为函数参数
    ```c++
    void addOne (int i) {
      i++;//不会改变a
    }
    void addOne (int& i) {
      i++;//会改变a
    }

    int a { 1 };
    addOne(a);

    addOne(5);//引用传递会编译错误，因为引用时隐式const常量，不能传递字面量
    ```
    >从函数返回对象的推荐方法是通过值返回，而不是使用一个输出参数
  
4. const 引用传递
   >可以实现参数不产生任何副本，并且无法更改原始变量
   ```c++
   void printString(const string& s) {//用const修饰
    cout << s << endl;
   }
   int main()
   {
    string s {"hhh"};
    printString(s);
    printString("hhh");//用了const修饰的引用就可以传递字面量了
    return 0;
   }
   ```
  
5. 引用作为返回值
   >必须保证函数终止后返回的引用所指向的变量继续存在的情况