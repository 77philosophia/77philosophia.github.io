---
title: C++_Notes
categories:
  - 专业笔记
date: 2025-01-17 17:20:02
tags:
---



## Lec2: types and structures

1.STL：standard template Library

访问stl中的元素要用std::（IDK为什么不用stl）

2.c++是一种静态类型语言：runtime之前变量、函数就被赋予了类型。

与之相对应的动态类型语言：runtime时候根据变量和函数的value确认类型

> [!NOTE]
>
> 编译型语言和解释型语言的主要区别：source code翻译成机器码的时机

3.struct：绑定各种不同类型的变量

```c++
Student s;
s.name = "Sarah";
s.state = "CA";
s.age = 21;
等价于
Student s = {"Sarah","CA",21};
```

4.std::pair 

```c++
std::pari<int,string> numSuffix = {1,"st"};
numSuffix.first
```

一般通过std::pair返回success状态+result

```c++
std::pair<bool, Student> lookupStudent(string name) {
Student blank;
if (notFound(name)) return std::make_pair(false, blank);
Student result = getStudentWithName(name);
return std::make_pair(true, result);
}
std::pair<bool, Student> output = lookupStudent(“Julie”)
```

5.auto:让编译器自主推导类型



## Lec3:Initialization&References

### 1. Uniform initialization

对所有类型都适用，在声明的时候立即初始化

```c++
std::vector<int> vec{1,3,5};
std::pair<int, string> numSuffix1{1,"st"};
Student s{"Sarah"
, "CA"
, 21};
// less common/nice for primitive types, but possible!
int x{5};
string f{"Sarah"};
```

vector初始化的时候要注意

```c++
std::vector<int> vec1(3,5);
// makes {5, 5, 5}, not {3, 5}!
// uses a std::initializer_list (more later)
std::vector<int> vec2{3,5};
// makes {3, 5}
```

> [!NOTE]
>
> 使用uniform initialization的方式去初始化任何你的non-primitive typed variables,但是不要用vec(n,k)

### 2. auto

编译器推断类型，用于类型名字明显而且冗长的时候用auto代替来让代码清晰。

### 3.Structured Binding

```c++
# before
auto p =
std::make_pair(“s”, 5);
string a = p.first;
int b = p.second;

#after
auto p =
std::make_pair(“s”, 5);
auto [a, b] = p;
// a is string, b is int
// auto [a, b] =
std::make_pair(...);
```

对structs结构也适用



### 4.引用refrence

### 5. 左值和右值

```c++
//l-values可以出现在=的左边和右边
//x是一个l-value
int x = 3;
int y = x;
//l-values有名字
//l-values不是暂时的

//r-value只出现在=的右边
// 3是一个r-value
// r-value没有名字，是暂时的
```

看一些错误示例

```c++
void shift(vector<std::pair<int, int>>& nums) {
for (auto& [num1, num2]: nums) {
num1++;
num2++;
}
}
shift({{1, 1}});
// {{1, 1}} is an rvalue, it can’t be referenced

//改正：
void shift(vector<pair<int, int>>& nums) {
for (auto& [num1, num2]: nums) {
num1++;
num2++;
}
}
auto my_nums = {{1, 1}};
shift(my_nums);
```

引用只能创建在变量上

```c++
int& thisWontWork = 5; // This doesn't work!
```



### 6. const和const引用

const可以修饰references或者非引用变量。

```c++
std::vector<int> vec{1, 2, 3};
const std::vector<int> c_vec{7, 8}; // a const variable
std::vector<int>& ref = vec; // a regular reference  
const std::vector<int>& c_ref = vec; // a const reference
vec.push_back(3); // OKAY
c_vec.push_back(3); // BAD - const
ref.push_back(3); // OKAY     
c_ref.push_back(3); // BAD - const
```

不能用non-const引用修饰一个const变量

```c++
const std::vector<int> c_vec{7, 8}; // a const variable
// BAD - can't declare non-const ref to const vector
std::vector<int>& bad_ref = c_vec;

// fixed
const std::vector<int>& bad_ref = c_vec;

// BAD - Can't declare a non-const reference as equal
// to a const reference!
std::vector<int>& ref = c_ref;
```

加上auto(简而言之：auto自动推断类型不会推断const，auto修饰const也要加上)

```c++
std::vector<int> vec{1, 2, 3};
const std::vector<int> c_vec{7, 8};
std::vector<int>& ref = vec;
const std::vector<int>& c_ref = vec;
auto copy = c_ref; // a non-const copy
const auto copy = c_ref; // a const copy
auto& a_ref = ref; // a non-const reference
const auto& c_aref = ref; // a const reference
```

记住：c++默认会制造拷贝在做变量赋值的时候，如果需要用引用需要加&

> [!TIP]
>
> 占用内存小的没有必要用引用，比如int,double
>
> 需要改变变量的可以用引用
>
> 不需要调整值，但是变量大的，可以用const reference

也可以返回引用

```c++
// Note that the parameter must be a non-const reference to return
// a non-const reference to one of its elements!
int& front(std::vector<int>& vec) {
// assuming vec.size() > 0
return vec[0];
CODE DEMO
}
int main() {
std::vector<int> numbers{1, 2, 3};
front(numbers) = 4; // vec = {4, 2, 3}
return 0;
}
```

返回const引用

```c++
const int& front(std::vector<int>& vec) {
// assuming vec.size() > 0
return vec[0];
}
```



### Lec7: Streams

Streams: input/output的抽象，把data和data的string表示进行转化。

![image-20250119002648265](C-Notes/image-20250119002648265.png)

output streams

类型是std::ostream

![image-20250119231100562](C-Notes/image-20250119231100562.png)

output file streams

![image-20250119231150353](C-Notes/image-20250119231150353.png)

> [!NOTE]
>
> Std:cout是一个全局的constant object，通过#include<iostream>引入
>
> 其他的output stream需要先初始化

Std::cin

类型是std::istream

```c++
int x;
std::cin >> x;
```

![image-20250119231451766](C-Notes/image-20250119231451766.png)

![image-20250119231638276](C-Notes/image-20250119231638276.png)

Std::istream可以看出一个character的sequence，在遇到空格、tab、换行符的时候就会将buffer的值赋给变量。

![image-20250120140203477](C-Notes/image-20250120140203477.png)

input streams输入类型错误的时候，继续读入的东西就无效了

![image-20250120140418147](C-Notes/image-20250120140418147.png)

Std::get line()

![image-20250120140526819](C-Notes/image-20250120140526819.png)

![image-20250120140621449](C-Notes/image-20250120140621449.png)

> [!NOTE]
>
> ">>"提取single输入转换为built-in类型；如果要读入整行，需要用std::getline(istream& stream, string& line);

注意std::getline(istream& stream, string& line)的参数都是传引用。

```c++
std::string line;
std::getline(cin, line); //line changed now!
//say the user entered “Hello World 42!”
std::cout << line << std::endl;
//should print out “Hello World 42!”
```

> [!NOTE]
>
> 不要弄混>>和getline
>
> ">>"读直到下一个空格字符并且不会读入该空格字符
>
> getline读直到遇到下一个分隔符（默认就是\n）并且会读入这个换行符

##### Input File Streams

- 类型是std::ifstream

- 接收字符串必须用“>>”（从file中接收字符串并且转换为built-in类型）

- ifstream必须先初始化

  ```c++
  std::ifstream in(“out.txt”);
  // in is now an ifstream that reads from out.txt
  string str;
  in >> str; // first word in out.txt goes into str
  ```



### Stringstreams

stream读写到string对象中；

目的：允许对string的输入输出按照stream来。

```c++
std::string input = "123";
std::stringstream stream(input);
int number;
stream >> number;
std::cout << number << std::endl; // Outputs "123" 
```

总结：

- streams将data和data的字符串形式进行相互转换
- 控制台cin/cout，文件i/ofstreams,字符串i/o stringstreams
- Send data to a stream用stream name << data
- 从stream提取data用stream_name >> data, 并且还会将数据类型转换为data的数据类型



## Containers

容器种类：

![image-20250121165106294](C-Notes/image-20250121165106294.png)

![image-20250121165136897](C-Notes/image-20250121165136897.png)

![image-20250121170648848](C-Notes/image-20250121170648848.png)

##### container adaptors 容器适配器



## Lec6: iterators and pointers



![image-20250122191923467](C-Notes/image-20250122191923467.png)



## Lec7:classes

class相关的略

### template

![image-20250123163820237](C-Notes/image-20250123163820237.png)

模版类：

![image-20250123164006679](C-Notes/image-20250123164006679.png)

```c++
//mypair.h
public:
First getFirst();
Second getSecond();
template<typename First, typename Second> class MyPair {
void setFirst(First f);
void setSecond(Second f);
private:
First first;
Second second;
};
```

实现一个模版类：

```c++
//mypair.cpp
#include “mypair.h”
template<typename First, typename Second> //必须要声明每个成员函数都是模版的
First MyPair::getFirst(){
return first;
}
template<typename Second, typename First>
Second MyPair::getSecond(){
return second;
}
```

> [!NOTE]
>
> Templates不会产生代码直到实例化，因此要将.cpp文件包含在.h文件里。

![image-20250123170609616](C-Notes/image-20250123170609616.png)

理解c++编译器对于非模版类做了什么

![image-20250123170737511](C-Notes/image-20250123170737511.png)

理解c++编译器对于模版类做了什么

![image-20250123170841268](C-Notes/image-20250123170841268.png)

修复：

![image-20250123171745108](C-Notes/image-20250123171745108.png)

![image-20250123171805421](C-Notes/image-20250123171805421.png)

## Lec8: templateclassesconst

模版的用途就是将不同类型要实现相同功能的逻辑抽象出来，

将一个特定类型的类改为模版类：

```c++
class MyPair {
  public:
  	int getFirst();
    int getSecond();
  
  	void setFirst(int f);
  	void setSecond(int f);
  private:
  	int first;
  	int second;
};
```

写一个模版类：

```c++
template<typename First，typename Second> class MyPair {
  public:
  	First getFirst();
  	Second getSecond();
  
  	void setFirst(First f);
  	void setSecond(Second f);
  private:
  	First first;
  	Second second;
};
```

类外实现：

a.要声明类的成员函数是模版函数

b.要注意模版类的命名空间不是MyPair而是MyPair<First, Second>

![image-20250126171712040](C-Notes/image-20250126171712040.png)

模版类的成员类型：

![image-20250126171841419](C-Notes/image-20250126171841419.png)

```c++
// vector.h
template<typename T> class vector {
  public:
  using iterator = T*
    
  iterator begin();
}

//vector.cpp
template<typename T>
iterator vector<T>::begin() {...}  // error，iterator是类里面的一个嵌套类型

//vector.cpp 修正
template<typename T>
typename vector<T>::iterator vector<T>::begin() {...}
```

> [!NOTE]
>
> 1.using type_name = type定义了一个嵌套的类型，就如同vector<T>::iterator 
>
> 2.使用类定义vector<T>之前要加typename

总结：

![image-20250126172439971](C-Notes/image-20250126172439971.png)

#### const

类里，const-interface是所有标记为const的成员函数; const ClassName只能使用const-interface

比如：const Student

```c++
//main.cpp
std::string stringify(const Student& s){
return s.getName() + " is " + std::to_string(s.getAge()) +
" years old." ;
}
// With our new const-interface, this will work :)
```

类StrVector的at可以标记为const吗

![image-20250126182231645](C-Notes/image-20250126182231645.png)

返回的是引用，依然可以修改。从语义上来说不能标记为const

需要新建立一个关于at的const版本的函数

![image-20250126182354489](C-Notes/image-20250126182354489.png)

> [!NOTE]
>
> 不要重复造轮子，而是用static_cast和const_cast重用相关的逻辑。

```c++
std::string& StrVector::at(size_t index) {
	if (index >= size()) {
		throw std::out_of_range("Index out of range in at.");
	}
	return operator[](index); // operator[] = return *(begin() + index)
}

const std::string& StrVector::at(size_t index) {
  return static_cast<const std::string&>(const_cast<StrVecotr*>(this)->at(index));
}
```

![image-20250126182750157](C-Notes/image-20250126182750157.png)

![image-20250126182800510](C-Notes/image-20250126182800510.png)

关于类的begin()和end()类似于at，也需要一个const版本的逻辑

解决方法：const_iterator

![image-20250126182947080](C-Notes/image-20250126182947080.png)

```c++
class StrVector {
public:
using iterator = std::string*;
using const_ iterator = const std::string*;
/*...*/
size_t size() const;
bool empty() const;
/*...*/
void push_back(const std::string& elem);
const std::string& at(size_t indx) const;
iterator begin();
iterator end();
const_iterator begin()const;
const_iterator end()const;
/*...*
```

![image-20250126183014764](C-Notes/image-20250126183014764.png)



## Lec9: template functions

类似模版类，模版函数直到使用才会编译。

```c++
template <typename T, typename U>
auto smarterMyMin(T a, U b){
  return a < b ? a : b;
}
```

代码没有生成直到实例化，这样运行更快（**生成的代码针对具体类型**，没有额外的运行时检查或类型转换。）

根据这个特点模版可以用在高效性方面。

#### template metaprogramming

代码只在编译过程中运行一次。

```c++
template<unsigned n>
struct Factorial {
  enum {value = n * Factorial<n-1>::value};
};

template<>
struct Factorial<0> {
  enum {value = 1};
}

std::cout << Factorial<10>::value << endl; // prints 3628800, but run during compile time!
```

#### aside: constexpr

另一个在编译期间产生code的工具：constexpr关键字

```c++
constexpr double fib(int n) { // function declared as constexpn
if (n == 1) return 1;
return fib(n-l) * n;
}
int main() {
const long long bigval = fib(20);
std: : cout << bigval << std::endl;
}
```

TMP是意外发现的，不是被发明的，使用场合也不多；可能用在加速矩阵数学运算/特殊设计/游戏图像方面。



## Lec10: functions_and_lambdas

#### predicate Functions

返回true或者false的函数

![image-20250127173135145](C-Notes/image-20250127173135145.png)

- 函数指针
  - Function pointers可以和其他指针一样被对待
  - 可以像变量一样传递
  - 可以像函数一样被call

有个问题，泛化性比较差

![image-20250128235600102](C-Notes/image-20250128235600102.png)

![image-20250128235614504](C-Notes/image-20250128235614504.png)

![image-20250128235627468](C-Notes/image-20250128235627468.png)

我们想传递更多信息，但是不允许传递两个以上的参数

我们需要可以传递更多信息但是不需要传递一个以上参数的方法

```c++
auto var = [capture-clause](auto praram) -> bool
{
  
}
```

#### lambdas

Inline,匿名函数

![image-20250128235902715](C-Notes/image-20250128235902715.png)

![image-20250128235916833](C-Notes/image-20250128235916833.png)

![image-20250128235932720](C-Notes/image-20250128235932720.png)

= 通过value捕获

&通过引用捕获

![image-20250129000033376](C-Notes/image-20250129000033376.png)

> [!NOTE]
>
> use lambda当你需要一个短的函数或者需要局部值；
>
> 如果需要更多逻辑或者重载，使用函数指针



#### functor

functor是实现了operator()的类；

他们可以create closures对于customized functions

- Closure:一个functor object的单一实例化。（sigle instantiation）
- lambdas是一个换肤（reskin）of functors.

```c++
class functor {
 public:
  	int operator() (int arg) const {
      return num + arg;
    }
 private:
    int num;
};

int num = 0;
auto lambda = [&num](int arg){num += arg;};

```

![image-20250203121340540](C-Notes/image-20250203121340540.png)



#### virtual functions

看一个例子：

```c++
class Animal {
// constructors and other methods go here!
void speak() {
std::cout << "I'm an animal!" << std::endl;
} // private information and the rest of the class goes here!
class Dog : Animal { // this syntax tells us we're a subclass of Animal!
// constructors and private information here!
void speak() {
std::cout << "I'm an animal!" << std::endl;
} // private information and the rest of the class goes here!
}
```

如果我们传递一个dog对象：

```c++
void func(Animal* animal) { animal->speak();
// can take in any animal and make it speak!
}
int main() {
Animal* myAnimal = new Animal;
Dog* myDog = new Dog;
func(myAnimal);
fun
```

![image-20250203182923379](C-Notes/image-20250203182923379.png)

> [!NOTE]
>
> 如果指针指向一个夫类对象，不知道要使用子类的方法；
>
> 同样的问题存在于将一个夫类的指针指向子类的对象；

解决方法：将夫类被overriden的方法声明为virtual

![image-20250203183111810](C-Notes/image-20250203183111810.png)



## Lec11 operators和operators overload

可以overload的operator:

![image-20250203212741087](C-Notes/image-20250203212741087.png)

不能重载的运算符：

![image-20250203212810612](C-Notes/image-20250203212810612.png)

![image-20250203212856249](C-Notes/image-20250203212856249.png)

![image-20250203212905125](C-Notes/image-20250203212905125.png)

有两种重载运算符的方法：

![image-20250203213204584](C-Notes/image-20250203213204584.png)

非成员函数的重载更被stl所接纳：

![image-20250203213525137](C-Notes/image-20250203213525137.png)

成员函数的重载可以访问this->和私有变量

对于非成员函数的重载需要通过friend访问

![image-20250203213844189](C-Notes/image-20250203213844189.png)

注意new和delete的非成员函数重载，由于不针对特定类型的输入，会影响全局。

![image-20250203214025479](C-Notes/image-20250203214025479.png)



## Lec12 特殊的成员函数

六种特殊的成员函数：

- 默认构造函数 default constructor
- 析构函数 destructor
- 拷贝构造函数 copy constructor
- 拷贝赋值运算符copy assignment operator
- 移动构造函数move constructor
- 移动赋值运算符move assignment operator

```c++
class Widget {
 public:
  Widget(); // default constructor
  Widget(const Widget& w); //copy constructor 创建了一个新的对象
  Widget& operator = (const Widget& w); //拷贝赋值运算符 copy assignment operator 将一个已经存在的对象赋值给另一个
  ～Widget(); // destructor
  Widget(Widget&& rhs); //move constructor
  Widget& operator = (Widget&& rhs);
}
```

为什么需要override

![image-20250203224111121](C-Notes/image-20250203224111121.png)

### default & delete

想要禁止拷贝

![image-20250203224234915](C-Notes/image-20250203224234915.png)

> [!IMPORTANT]
>
> 应用： std::unique_ptr 只想要一份实例的拷贝
>
> 实现了MoveConstructible和MoveAssignable，但是拷贝构造和拷贝赋值没有实现

### =default

保持默认的构造函数的实现（用户定义构造函数编译器不会默认生成，如果要保留需要=default）

原则上：

1. 默认构造函数可以用的时候不要新定义
2. 如果不得不定义destructor, copy constructor或者copy assignment operator，这三个都应该被重定义



### move

总是拷贝开销会很大；

![image-20250203225320261](C-Notes/image-20250203225320261.png)



## Lect14 C++里的move语义

![image-20250203225430293](C-Notes/image-20250203225430293.png)

![image-20250203225438211](C-Notes/image-20250203225438211.png)

![image-20250203225520653](C-Notes/image-20250203225520653.png)

![image-20250203225547927](C-Notes/image-20250203225547927.png)

![image-20250203231122204](C-Notes/image-20250203231122204.png)

![image-20250203233445459](C-Notes/image-20250203233445459.png)

![image-20250203233620968](C-Notes/image-20250203233620968.png)

![image-20250203233632186](C-Notes/image-20250203233632186.png)

![image-20250203233809478](C-Notes/image-20250203233809478.png)

![image-20250203233858206](C-Notes/image-20250203233858206.png)

![image-20250203233912300](C-Notes/image-20250203233912300.png)

![image-20250203234104709](C-Notes/image-20250203234104709.png)

![image-20250203234159789](C-Notes/image-20250203234159789.png)

![image-20250203234259860](C-Notes/image-20250203234259860.png)

![image-20250203234308018](C-Notes/image-20250203234308018.png)

![image-20250203234428066](C-Notes/image-20250203234428066.png)

![image-20250203234444525](C-Notes/image-20250203234444525.png)

![image-20250203234501070](C-Notes/image-20250203234501070.png)

### heck &&

区分拷贝赋值和移动赋值

![image-20250203234545870](C-Notes/image-20250203234545870.png)

![image-20250203234750833](C-Notes/image-20250203234750833.png)

![image-20250203234805928](C-Notes/image-20250203234805928.png)

![image-20250203234828322](C-Notes/image-20250203234828322.png)

![image-20250203234916234](C-Notes/image-20250203234916234.png)

使用std::move

![image-20250203234931990](C-Notes/image-20250203234931990.png)

![image-20250203234943875](C-Notes/image-20250203234943875.png)

![image-20250203235220492](C-Notes/image-20250203235220492.png)

SMFs使用规则：

![image-20250203235250464](C-Notes/image-20250203235250464.png)

![image-20250203235338280](C-Notes/image-20250203235338280.png)

![image-20250203235348312](C-Notes/image-20250203235348312.png)



## Lec14: Type Safety and std::optional

类型检查需要检查数组是否为空和是否越界，否则会返回未定义的行为。

![image-20250203235855513](C-Notes/image-20250203235855513.png)

通过std::pair返回返回状态和返回值

![image-20250204000001279](C-Notes/image-20250204000001279.png)

![image-20250204000011320](C-Notes/image-20250204000011320.png)

用std::optinal<T>返回false时候的值。

![image-20250204000101966](C-Notes/image-20250204000101966.png)

![image-20250204000140494](C-Notes/image-20250204000140494.png)

![image-20250204000150587](C-Notes/image-20250204000150587.png)

![image-20250204000301227](C-Notes/image-20250204000301227.png)

![image-20250204000549867](C-Notes/image-20250204000549867.png)

![image-20250204000606798](C-Notes/image-20250204000606798.png)
