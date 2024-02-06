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

#### 函数
- 可以用auto代替函数的返回类型，让编译器自动推断出来
- 每个函数都有一个预定义的局部变量`__func__`,其中包含当前函数的名称，这个变量的一个用途是用于日志记录：**【注意：前后都是两个短横线】**
  ```c++
  int add(int n1, int n2) {
    cout << format("Entering function {}", __func__) << endl;
    return n1 + n2;
  }
  ```
