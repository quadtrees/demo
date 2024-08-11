### 范围解析运算符 （Scope Resolution Operator）

#### 1. 全局作用域解析

在函数或变量名前使用 `::` 可以访问全局作用域中的变量或函数，特别是当局部作用域中存在同名变量或函数时。

```c++
int value = 10; // 全局变量

void Function()
{
    int value = 20; // 局部变量
    std::cout << "Local value: " << value << std::endl; // 输出局部变量
    std::cout << "Global value: " << ::value << std::endl; // 输出全局变量
}

int main()
{
    Function();
    return 0;
}
```

#### 2. 类成员函数定义

在类外定义成员函数时，需要使用 `::` 运算符来指定函数所属的类。

```c++
class MyClass
{
public:
    void MyFunction();
};

// 使用::来定义MyFunction
void MyClass::MyFunction()
{
    std::cout << "This is a member function of MyClass." << std::endl;
}

int main()
{
    MyClass obj;
    obj.MyFunction();
    return 0;
}
```

#### 3. 命名空间成员访问

在使用命名空间中的成员时，`::` 运算符用于指定命名空间。例如，标准库命名空间 `std` 中的成员。

```c++
#include <iostream>

int main()
{
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```

#### 4. 类的静态成员访问

在访问类的静态成员时，`::` 运算符用于指定类名。

```c++
class MyClass
{
public:
    static int staticValue;
};

// 定义静态成员变量
int MyClass::staticValue = 100;

int main()
{
    std::cout << "Static Value: " << MyClass::staticValue << std::endl;
    return 0;
}
```

#### 5. 嵌套类成员访问

在访问嵌套类的成员时，也需要使用 `::` 运算符来指定外部类和嵌套类。

```c++
class MyClass
{
public:
    static int staticValue;
};

// 定义静态成员变量
int MyClass::staticValue = 100;

int main()
{
    std::cout << "Static Value: " << MyClass::staticValue << std::endl;
    return 0;
}
```

## 常量函数

常量函数在 C++ 中是指那些不会修改对象状态（即不会修改类的成员变量）的成员函数。常量函数通过在函数声明后加上 `const` 关键字来定义。常量函数可以被常量对象调用，同时也可以被非常量对象调用。理解常量函数的作用和使用场景对于确保代码的正确性和安全性非常重要。

### 常量函数的定义

```c++
返回类型 函数名(参数列表) const;
```

```c++
class MyClass {
public:
    int GetValue() const; // 这是一个常量函数
    void SetValue(int value);

private:
    int value;
};
```

### 常量函数的作用

**保证不修改状态**：常量函数保证在其调用期间不会修改对象的状态。这对于确保对象的一致性非常重要。

**增强接口的可读性**：常量函数使得接口更加清晰，调用者可以确定哪些函数不会修改对象的状态。

**允许常量对象调用**：常量对象只能调用常量函数，这对于确保对象在某些上下文中不会被修改非常重要。

### 常量对象调用常量函数

```c++
const MyClass obj;
obj.GetValue(); // 可以调用常量函数
obj.SetValue(10); // 错误：不能调用非常量函数
```

### 示例

```c++
class MyClass {
public:
    MyClass(int val) : value(val) {}

// 常量函数，不会修改对象状态
int GetValue() const {
    return value;
}

// 非常量函数，可以修改对象状态
void SetValue(int val) {
    value = val;
}

private:
    int value;
};

int main() {
    const MyClass constObj(10);
    MyClass obj(20);

// 调用常量函数
int val1 = constObj.GetValue();
int val2 = obj.GetValue();

// 调用非常量函数
obj.SetValue(30);

// 错误：不能调用非常量函数
// constObj.SetValue(40);

return 0;

}
```



## 引用（Reference）

C++中的引用（Reference）是一个对已经存在的变量的别名。引用在函数参数传递、返回值优化、避免数据拷贝等方面有广泛的应用。理解引用的工作机制对于编写高效和安全的C++代码非常重要。

#### 基本概念

##### 引用声明

```c++
type &referenceName = variableName;
```

```c++
int a = 10;
int &ref = a; // ref 是 a 的引用，ref 和 a 绑定在一起，ref 就是 a 的另一个名字
```

##### 特性

1.引用必须在声明时初始化：

```c++
int &ref; // 错误，引用必须初始化
int a = 10;
int &ref = a; // 正确
```

2.引用一旦初始化后，就不能再改变引用对象：

```c++
int a = 10;
int b = 20;
int &ref = a;
ref = b; // 这是将 a 的值赋给 b，而不是让 ref 引用 b
```

3.引用不能为null，必须引用一个合法的对象

#### 应用场景

##### 1.作为函数参数

引用作为函数参数可以避免拷贝数据，提高效率，特别是对于大对象或复杂数据结构。

```c++
int &ref; // 错误，引用必须初始化
int a = 10;
int &ref = a; // 正确
```

##### 2.作为函数返回值

引用作为函数返回值可以返回对象本身，而不是对象的拷贝，这样可以提高效率。

```c++
int& getElement(int arr[], int index) {
    return arr[index];
}

int main() {
    int arr[5] = {1, 2, 3, 4, 5};
    getElement(arr, 1) = 10; // 修改 arr[1] 为 10
    return 0;
}
```

##### 3.常量引用

常量引用允许你在函数参数中传递只读引用，从而保证参数在函数中不会被修改。

这里const int &val 和 int const &val完全等价

```c++
void printValue(const int &val) {
    std::cout << val << std::endl;
}

int main() {
    int a = 5;
    printValue(a);
    return 0;
}
```

#### 引用与指针的区别

语法：引用的使用语法更接近于变量本身，而指针需要使用`*`和`&`操作符。

初始化：引用在声明时必须初始化，而指针可以在任何时候被赋值。

不可更改：引用一旦绑定到某个变量，就不能再改变指向，而指针可以随时指向其他对象。

安全性：引用不能为null，必须引用一个合法的对象，而指针可以为null，容易产生悬空指针或野指针。

#### 左值引用和右值引用（C++11及以上）

C++11引入了右值引用（Rvalue Reference），用于实现移动语义和完美转发。

##### 左值引用（Lvalue Reference）

用于引用变量或对象的别名：

```c++
int a = 5;
int &lval = a; // lval 是 a 的左值引用
```

##### 右值引用（Rvalue Reference）

用于引用临时对象：

```c++
int &&rval = 5; // rval 是一个右值引用，引用临时变量5
```

##### 右值引用的主要用途

移动语义：优化资源管理，避免不必要的深拷贝。

完美转发：在模板中保持参数类型。

```c++
class MyClass {
public:
    MyClass() : data(new int[100]) { std::cout << "Constructor" << std::endl; }
    ~MyClass() { delete[] data; }
    

// 移动构造函数
MyClass(MyClass&& other) noexcept : data(other.data) {
    other.data = nullptr;
    std::cout << "Move Constructor" << std::endl;
}

private:
    int* data;
};

MyClass createObject() {
    MyClass obj;
    return obj; // 返回临时对象，将触发移动构造函数
}

int main() {
    MyClass a = createObject(); // 使用移动语义
    return 0;
}
```

## 反射

C++中的反射（reflection）指的是在运行时检查和修改程序结构的能力，例如类的成员、方法、属性等。在其他高级语言（如Java、C#）中，反射是内置的功能。然而，C++没有内置的反射机制，但可以通过一些工具和框架来实现类似的功能。以下是详细的解释和一些实现方法。

#### 反射的基本概念

反射允许程序在运行时获得关于自身的信息，并且可以在运行时操作这些信息。具体来说，反射可以让程序：

- 枚举和查询类及其成员（属性、方法）。
- 动态调用方法。
- 动态访问和修改属性的值。

#### 实现反射的几种方法

##### 1. 使用宏和模板

在C++中，可以通过宏和模板来实现基本的反射功能。这种方法比较原始，但可以实现一些简单的反射操作。

```c++
#include <iostream>
#include <string>
#include <map>
#include <functional>

class Reflectable {
public:
    virtual ~Reflectable() = default;

void SetProperty(const std::string& name, int value) {
    properties[name] = value;
}

int GetProperty(const std::string& name) const {
    auto it = properties.find(name);
    if (it != properties.end()) {
        return it->second;
    }
    return 0; // or throw an exception
}

private:
    std::map<std::string, int> properties;
};

#define REFLECTABLE(...) \
    public: \
        static const std::vector<std::string>& GetProperties() { \
            static const std::vector<std::string> properties = { __VA_ARGS__ }; \
            return properties; \
        }

class MyClass : public Reflectable {
    REFLECTABLE("property1", "property2")

public:
    int property1;
    int property2;
};

int main() {
    MyClass obj;
    obj.SetProperty("property1", 42);

std::cout << "property1: " << obj.GetProperty("property1") << std::endl;

return 0;

}
```

##### 2. 使用元编程和RTTI

C++的运行时类型信息（RTTI）可以用于实现反射的一部分功能，例如通过`typeid`获取类型信息。

```c++
#include <iostream>
#include <typeinfo>

class Base {
public:
    virtual ~Base() = default;
};

class Derived : public Base {
};

int main() {
    Base* b = new Derived();
    std::cout << "Type: " << typeid(*b).name() << std::endl;
    delete b;
    return 0;
}
```

##### 3. 使用第三方库

有些第三方库专门用于C++反射，例如RTTR（Run Time Type Reflection）。

```c++
#include <rttr/registration>
#include <iostream>

using namespace rttr;

class MyClass {
public:
    int property1;
    double property2;

void MyMethod() {
    std::cout << "MyMethod called" << std::endl;
}

};

RTTR_REGISTRATION {
    registration::class_<MyClass>("MyClass")
        .property("property1", &MyClass::property1)
        .property("property2", &MyClass::property2)
        .method("MyMethod", &MyClass::MyMethod);
}

int main() {
    type t = type::get<MyClass>();
    for (auto& prop : t.get_properties()) {
        std::cout << "Property: " << prop.get_name() << std::endl;
    }
    for (auto& method : t.get_methods()) {
        std::cout << "Method: " << method.get_name() << std::endl;
    }
    return 0;
}
```

##### 4. Unreal Engine中的反射

Unreal Engine通过宏和自定义工具（如Unreal Header Tool）实现了强大的反射系统，使得在引擎中可以方便地进行反射操作。

```c++
UCLASS()
class MYGAME_API AMyActor : public AActor {
    GENERATED_BODY()

public:
    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Properties")
    int32 MyProperty;

UFUNCTION(BlueprintCallable, Category = "Functions")
void MyFunction();

};
```

在这个例子中，Unreal Engine使用`UCLASS`、`UPROPERTY`和`UFUNCTION`宏来实现类、属性和函数的反射，使得这些元素可以在引擎中被编辑器和蓝图访问和修改。

#### 总结

C++没有内置的反射机制，但可以通过宏、模板、RTTI以及第三方库来实现反射功能。每种方法都有其优缺点，具体选择哪种方法取决于项目的需求和开发环境。如果你需要在Unreal Engine中使用反射，那么引擎本身提供的宏和工具是最方便和强大的选择。

### 命名空间（Namespace）

#### 创建命名空间

命名空间的定义使用 `namespace` 关键字，后跟命名空间的名称和一个包含代码的块。下面是一个简单的示例：

```c++
namespace MyNamespace {
    int myVariable = 42;

void myFunction() {
    std::cout << "Hello from MyNamespace!" << std::endl;
}

class MyClass {
public:
    void display() {
        std::cout << "MyClass in MyNamespace" << std::endl;
    }
};

}
```

#### 使用命名空间

要访问命名空间中的成员，可以使用范围解析运算符 `::`。以下是如何使用 `MyNamespace` 中定义的成员：

```c++
#include <iostream>

// 定义命名空间
namespace MyNamespace {
    int myVariable = 42;

void myFunction() {
    std::cout << "Hello from MyNamespace!" << std::endl;
}

class MyClass {
public:
    void display() {
        std::cout << "MyClass in MyNamespace" << std::endl;
    }
};

}

int main() {
    // 访问命名空间中的变量
    std::cout << "myVariable: " << MyNamespace::myVariable << std::endl;

// 调用命名空间中的函数
MyNamespace::myFunction();

// 使用命名空间中的类
MyNamespace::MyClass myObject;
myObject.display();

return 0;

}
```

#### 使用 `using` 关键字（简化访问）

你可以使用 `using` 关键字将命名空间中的成员引入当前作用域，从而简化访问：

```c++
#include <iostream>

// 定义命名空间
namespace MyNamespace {
    int myVariable = 42;

void myFunction() {
    std::cout << "Hello from MyNamespace!" << std::endl;
}

class MyClass {
public:
    void display() {
        std::cout << "MyClass in MyNamespace" << std::endl;
    }
};

}

using namespace MyNamespace;

int main() {
    // 现在可以直接访问命名空间中的成员
    std::cout << "myVariable: " << myVariable << std::endl;
    myFunction();

MyClass myObject;
myObject.display();

return 0;

}
```

#### 嵌套命名空间

命名空间可以嵌套，即一个命名空间中可以定义另一个命名空间：

```c++
namespace OuterNamespace {
    int outerVariable = 100;

namespace InnerNamespace {
    int innerVariable = 200;

​    void innerFunction() {
​        std::cout << "Hello from InnerNamespace!" << std::endl;
​    }
}

}
```

要访问嵌套命名空间中的成员，可以使用层级的范围解析运算符：

```c++
#include <iostream>

namespace OuterNamespace {
    int outerVariable = 100;

namespace InnerNamespace {
    int innerVariable = 200;

​    void innerFunction() {
​        std::cout << "Hello from InnerNamespace!" << std::endl;
​    }
}

}

int main() {
    std::cout << "outerVariable: " << OuterNamespace::outerVariable << std::endl;
    std::cout << "innerVariable: " << OuterNamespace::InnerNamespace::innerVariable << std::endl;

OuterNamespace::InnerNamespace::innerFunction();

return 0;

}
```

#### 命名空间的别名

你可以使用 `namespace` 关键字创建命名空间的别名，以简化对长命名空间名称的引用：

```c++
namespace LongNamespaceName {
    void function() {
        std::cout << "Function in LongNamespaceName" << std::endl;
    }
}

namespace LNN = LongNamespaceName;

int main() {
    LNN::function();
    return 0;
}
```

### 命名空间 std

C++ 标准库中的所有组件都位于命名空间 `std` 中。命名空间 `std` 是标准库提供的一个命名空间，用于避免名字冲突，特别是在大型项目中。`std` 是 `standard` 的缩写，几乎所有的标准库类、函数和对象都在这个命名空间中定义。

#### 常见的 `std` 命名空间中的组件

##### 1. 输入输出流

- `std::cin`：标准输入流
- `std::cout`：标准输出流
- `std::cerr`：标准错误输出流
- `std::clog`：标准日志流

```c++
#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl;
    int a;
    std::cin >> a;
    std::cerr << "This is an error message" << std::endl;
    std::clog << "This is a log message" << std::endl;
    return 0;
}
```

##### 2. 容器

- `std::vector`：动态数组
- `std::list`：双向链表
- `std::deque`：双端队列
- `std::set`：集合
- `std::map`：映射（键值对）
- `std::unordered_set`：哈希集合
- `std::unordered_map`：哈希映射

```c++
#include <vector>
#include <list>
#include <set>
#include <map>

int main() {
    std::vector<int> vec = {1, 2, 3, 4};
    std::list<int> lst = {1, 2, 3, 4};
    std::set<int> s = {1, 2, 3, 4};
    std::map<int, std::string> m = {{1, "one"}, {2, "two"}};
    return 0;
}
```

##### 3. 算法

- ##### `std::sort`：排序

- `std::find`：查找

- `std::copy`：复制

- `std::transform`：变换

```c++
#include <algorithm>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> vec = {4, 2, 5, 1, 3};
    std::sort(vec.begin(), vec.end());
    for (int n : vec) {
        std::cout << n << " ";
    }
    return 0;
}
```

##### 4. 智能指针

- `std::unique_ptr`：独占所有权的智能指针
- `std::shared_ptr`：共享所有权的智能指针
- `std::weak_ptr`：弱引用，不控制对象生命周期

```c++
#include <memory>
#include <iostream>

int main() {
    std::unique_ptr<int> uptr = std::make_unique<int>(10);
    std::shared_ptr<int> sptr = std::make_shared<int>(20);
    std::weak_ptr<int> wptr = sptr;
    std::cout << *uptr << " " << *sptr << std::endl;
    return 0;
}
```

##### 5. 字符串和流

- `std::string`：字符串类
- `std::stringstream`：字符串流类

```c++
#include <string>
#include <sstream>
#include <iostream>

int main() {
    std::string str = "Hello, World!";
    std::stringstream ss;
    ss << str << " 2024";
    std::cout << ss.str() << std::endl;
    return 0;
}
```

##### 6. 时间和日期

- `std::chrono`：时间库
- `std::this_thread::sleep_for`：线程睡眠

```c++
#include <iostream>
#include <chrono>
#include <thread>

int main() {
    using namespace std::chrono_literals;
    std::cout << "Hello, World!" << std::endl;
    std::this_thread::sleep_for(2s);
    std::cout << "After 2 seconds" << std::endl;
    return 0;
}
```

#### 如何使用 `std` 命名空间

通常，您需要在使用 C++ 标准库中的组件时显式地指定 `std` 命名空间。这可以通过以下几种方式实现：

**使用 `std::` 前缀**：直接使用 `std::` 前缀来访问命名空间中的成员。

```c++
std::cout << "Hello, World!" << std::endl;
```

**使用 `using` 声明**：将命名空间中的特定成员引入到当前作用域。

```c++
using std::cout;
using std::endl;
cout << "Hello, World!" << endl;
```

**使用 `using namespace` 声明**：将整个命名空间引入到当前作用域（不推荐在全局范围内使用，以避免命名冲突）。

```c++
using namespace std;
cout << "Hello, World!" << endl;
```

#### 示例

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
#include <memory>

int main() {
    std::vector<int> vec = {5, 3, 1, 4, 2};
    std::sort(vec.begin(), vec.end());

std::cout << "Sorted vector: ";
for (int n : vec) {
    std::cout << n << " ";
}
std::cout << std::endl;

std::unique_ptr<std::string> uptr = std::make_unique<std::string>("Hello, World!");
std::cout << *uptr << std::endl;

std::shared_ptr<int> sptr = std::make_shared<int>(42);
std::cout << *sptr << std::endl;

return 0;

}
```



## 类

### 类的定义

```c++
#include <iostream>
using namespace std;

class MyClass {
public:
    MyClass(int x) : value(x) {} // 构造函数
    void display() const {
        cout << "Value: " << value << endl;
    }

private:
    int value;
};
```

### 类的实例化

```c++
int main() {
    // 实例化一个对象 obj1，构造函数初始化 value 为 10
    MyClass obj1(10);
    obj1.display(); // 输出: Value: 10

// 实例化另一个对象 obj2，构造函数初始化 value 为 20
MyClass obj2(20);
obj2.display(); // 输出: Value: 20

// 实例化更多对象并调用其成员函数
MyClass obj3(30);
obj3.display(); // 输出: Value: 30

return 0;

}
```

### 成员修饰符

##### public

访问权限：可以被类的外部代码访问，也可以被类的内部代码访问

继承：在继承时，基类的 `public` 成员在派生类中保持 `public` 访问级别

```c++
class MyClass {
public:
    int publicVar;  // 公有成员变量
    void publicFunc() { // 公有成员函数
        // 可以被类的外部代码访问
    }
};

MyClass obj;
obj.publicVar = 10;  // 允许访问
obj.publicFunc();    // 允许访问
```



##### private

访问权限：只能被类的内部代码访问，不能被类的外部代码访问。

继承：在继承时，基类的 `private` 成员在派生类中不可访问。

```c++
class MyClass {
private:
    int privateVar;  // 私有成员变量
    void privateFunc() { // 私有成员函数
        // 只能被类的内部代码访问
    }
};

MyClass obj;
// obj.privateVar = 10;  // 不允许访问，编译错误
// obj.privateFunc();    // 不允许访问，编译错误
```

```c++
#include <iostream>

class BankAccount {
public:
    BankAccount(double initialBalance) : balance(initialBalance) {}

void Deposit(double amount) {
    if (amount > 0) {
        balance += amount;
    }
}

void Withdraw(double amount) {
    if (amount > 0 && amount <= balance) {
        balance -= amount;
    }
}

double GetBalance() const {
    return balance;
}

private:
    double balance;

void LogTransaction(const std::string& transactionType, double amount) {
    // 私有方法，用于记录交易日志，不对外暴露
    std::cout << transactionType << ": " << amount << ", New Balance: " << balance << std::endl;
}

void UpdateBalance(double amount) {
    // 私有方法，用于更新余额，只能在类内部调用
    balance += amount;
}

};

int main() {
    BankAccount account(100.0);
    account.Deposit(50.0);
    account.Withdraw(30.0);

std::cout << "Final Balance: " << account.GetBalance() << std::endl;

// account.LogTransaction("Deposit", 50.0); // 错误：无法访问私有方法
// account.UpdateBalance(20.0); // 错误：无法访问私有方法

return 0;

}
```

封装：`LogTransaction` 和 `UpdateBalance` 方法是私有的，外部代码无法直接调用它们。

数据完整性与安全性：通过私有化 `UpdateBalance` 方法，确保只有类的内部逻辑可以修改余额，防止外部代码错误地修改余额。

控制访问：只有 `Deposit` 和 `Withdraw` 方法是公开的，控制了外部代码对类的访问方式。

提高可维护性：如果 `LogTransaction` 方法的实现需要改变，由于它是私有的，外部代码不会受到影响。

实现细节的隐藏：外部代码不需要知道 `LogTransaction` 和 `UpdateBalance` 的存在，只需要使用公开的接口。

##### protected

访问权限：只能被类的内部代码和派生类的代码访问，不能被类的外部代码访问。

继承：在继承时，基类的 `protected` 成员在派生类中保持 `protected` 访问级别。

```c++
class MyClass {
protected:
    int protectedVar;  // 受保护成员变量
    void protectedFunc() { // 受保护成员函数
        // 只能被类的内部代码和派生类的代码访问
    }
};

class DerivedClass : public MyClass {
    void someFunc() {
        protectedVar = 20;  // 允许访问
        protectedFunc();    // 允许访问
    }
};

MyClass obj;
// obj.protectedVar = 10;  // 不允许访问，编译错误
// obj.protectedFunc();    // 不允许访问，编译错误
```

### 继承&多态

#### 继承（Inheritance）

继承是指一个类（子类）从另一个类（父类）获取属性和方法的机制。通过继承，子类可以复用父类的代码，并且可以在子类中添加新的功能或重写父类的方法。

##### 继承的类型

1. **单继承**：一个子类继承一个父类。
2. **多重继承**：一个子类可以继承多个父类（C++ 支持多重继承，但其他一些语言如 Java 不支持）。
3. **多层继承**：一个类继承另一个类，该类又继承自另一个类，形成继承链。

```c++
#include <iostream>

// 基类
class Animal {
public:
    void eat() {
        std::cout << "I can eat!" << std::endl;
    }
};

// 派生类
class Dog : public Animal {
public:
    void bark() {
        std::cout << "I can bark! Woof woof!!" << std::endl;
    }
};

int main() {
    Dog myDog;
    myDog.eat(); // 调用基类的函数
    myDog.bark(); // 调用派生类的函数
    return 0;
}
```



#### 多态（Polymorphism）

多态允许一个接口（如函数、方法）有不同的实现。多态的主要目的是在运行时通过基类的指针或引用来调用派生类的重写方法。C++ 中的多态通过虚函数（virtual functions）实现。

##### 多态的类型

1. ##### **编译时多态（静态多态）**：通过函数重载和运算符重载实现。
2. **运行时多态（动态多态）**：通过继承和虚函数实现。

```c++
#include <iostream>

// 基类
class Animal {
public:
    virtual void makeSound() {
        std::cout << "Some generic animal sound" << std::endl;
    }
};

// 派生类
class Dog : public Animal {
public:
    void makeSound() override { // 重写基类的虚函数
        std::cout << "Woof woof!!" << std::endl;
    }
};

class Cat : public Animal {
public:
    void makeSound() override { // 重写基类的虚函数
        std::cout << "Meow meow!!" << std::endl;
    }
};

void printSound(Animal* animal) {
    animal->makeSound(); // 调用虚函数
}

int main() {
    Dog myDog;
    Cat myCat;

printSound(&myDog); // 输出 "Woof woof!!"
printSound(&myCat); // 输出 "Meow meow!!"

return 0;

}

### 
```

##### 虚析构函数

当基类包含虚函数时，应该也将其析构函数声明为虚函数，以确保正确地析构派生类对象。

```c++
class Animal {
public:
    virtual ~Animal() { // 虚析构函数
        std::cout << "Animal destroyed" << std::endl;
    }
};

class Dog : public Animal {
public:
    ~Dog() override {
        std::cout << "Dog destroyed" << std::endl;
    }
};
```

##### 总结

继承允许创建一个类层次结构，使得子类能够继承父类的属性和方法，从而实现代码重用和扩展。

多态允许通过基类的指针或引用来调用派生类的方法，从而实现接口的统一和灵活的代码结构。

虚函数和虚析构函数是实现运行时多态的关键。

### 类模板

#### 基本语法

```c++
template <typename T>
class MyClass {
public:
    MyClass(T value);
    void setValue(T value);
    T getValue() const;

private:
    T m_value;
};

template <typename T>
MyClass<T>::MyClass(T value) : m_value(value) {}

template <typename T>
void MyClass<T>::setValue(T value) {
    m_value = value;
}

template <typename T>
T MyClass<T>::getValue() const {
    return m_value;
}
```

#### 使用类模板

```c++
#include <iostream>
using namespace std;

int main() {
    MyClass<int> intObj(42); // 使用 int 类型实例化
    cout << "Integer value: " << intObj.getValue() << endl;

MyClass<double> doubleObj(3.14); // 使用 double 类型实例化
cout << "Double value: " << doubleObj.getValue() << endl;

MyClass<string> stringObj("Hello, World!"); // 使用 string 类型实例化
cout << "String value: " << stringObj.getValue() << endl;

return 0;

}
```

#### 模板高级语法

```c++
template <typename T, typename Container = std::vector<T>>
class MyContainer {
public:
    void add(const T& item) {
        m_container.push_back(item);
    }
    T get(int index) const {
        return m_container[index];
    }
private:
    Container m_container;
};
```



### 构造函数

#### 基本语法

```c++
class ClassName {
public:
    // 构造函数声明
    ClassName();                    // 默认构造函数
    ClassName(int param);           // 带参数的构造函数
    ClassName(int param1, int param2); // 多个参数的构造函数
    // ...其他成员函数和变量

private:
    int memberVariable1;
    int memberVariable2;
};
```

#### 构造函数的定义

```c++
#include <iostream>
using namespace std;

class MyClass {
public:
    // 默认构造函数
    MyClass() {
        cout << "Default constructor called" << endl;
        memberVariable1 = 0;
        memberVariable2 = 0;
    }

// 带一个参数的构造函数
MyClass(int param) {
    cout << "Parameterized constructor called with param: " << param << endl;
    memberVariable1 = param;
    memberVariable2 = 0;
}

// 带两个参数的构造函数
MyClass(int param1, int param2) {
    cout << "Parameterized constructor called with params: " << param1 << ", " << param2 << endl;
    memberVariable1 = param1;
    memberVariable2 = param2;
}

// 显示成员变量值的函数
void display() const {
    cout << "memberVariable1: " << memberVariable1 << ", memberVariable2: " << memberVariable2 << endl;
}

private:
    int memberVariable1;
    int memberVariable2;
};
```

#### 使用构造函数

```c++
int main() {
    MyClass obj1; // 调用默认构造函数
    obj1.display();

MyClass obj2(10); // 调用带一个参数的构造函数
obj2.display();

MyClass obj3(20, 30); // 调用带两个参数的构造函数
obj3.display();

return 0;

}
```

#### 初始化列表

```c++
class MyClass {
public:
    // 使用初始化列表的构造函数
    MyClass(int param1, int param2) : memberVariable1(param1), memberVariable2(param2) {
        cout << "Constructor with initialization list called" << endl;
    }

void display() const {
    cout << "memberVariable1: " << memberVariable1 << ", memberVariable2: " << memberVariable2 << endl;
}

private:
    int memberVariable1;
    int memberVariable2;
};
```

#### 拷贝构造函数

```c++
class MyClass {
public:
    // 使用初始化列表的构造函数
    MyClass(int param1, int param2) : memberVariable1(param1), memberVariable2(param2) {
        cout << "Constructor with initialization list called" << endl;
    }

void display() const {
    cout << "memberVariable1: " << memberVariable1 << ", memberVariable2: " << memberVariable2 << endl;
}

private:
    int memberVariable1;
    int memberVariable2;
};
```

### 统一初始化（Uniform Initialization）

C++11引入了统一初始化（Uniform Initialization），这是一种新的对象初始化语法，可以用于初始化对象、数组、结构体等。它通过使用大括号 `{}` 来统一不同的初始化方式，使得代码更加简洁和一致。以下是统一初始化的几种常见用法：

#### 初始化基本类型

```c++
int a{10};
double b{3.14};
```

#### 初始化数组

```c++
int arr[3] {1, 2, 3};
```

#### 初始化结构体或类

```c++
struct Point {
    int x;
    int y;
};

Point p1 {10, 20};
```

#### 初始化类对象

```c++
class MyClass {
public:
    MyClass(int a, double b) : x{a}, y{b} {}
private:
    int x;
    double y;
};

MyClass obj {5, 6.7};
```

#### 使用统一初始化列表（std::initializer_list）

```c++
#include <initializer_list>
#include <vector>

class MyClass {
public:
    MyClass(std::initializer_list<int> list) {
        for (auto elem : list) {
            data.push_back(elem);
        }
    }
private:
    std::vector<int> data;
};

MyClass obj {1, 2, 3, 4, 5};
```

#### 初始化容器

```c++
#include <vector>

std::vector<int> vec {1, 2, 3, 4, 5};
```

#### 禁止窄化转换

```c++
int x {3.14}; // 错误，无法从 double 窄化转换为 int
```

#### 例子

```c++
#include <iostream>
#include <vector>
#include <initializer_list>

struct Point {
    int x;
    int y;
};

class MyClass {
public:
    MyClass(int a, double b) : x{a}, y{b} {}
    MyClass(std::initializer_list<int> list) {
        for (auto elem : list) {
            data.push_back(elem);
        }
    }
    void print() {
        for (auto elem : data) {
            std::cout << elem << " ";
        }
        std::cout << std::endl;
    }
private:
    int x;
    double y;
    std::vector<int> data;
};

int main() {
    int a{10};
    double b{3.14};
    int arr[3]{1, 2, 3};
    Point p1{10, 20};
    MyClass obj1{5, 6.7};
    MyClass obj2{1, 2, 3, 4, 5};
    std::vector<int> vec{1, 2, 3, 4, 5};

std::cout << "Array: ";
for (int i : arr) {
    std::cout << i << " ";
}
std::cout << std::endl;

std::cout << "Vector: ";
for (int i : vec) {
    std::cout << i << " ";
}
std::cout << std::endl;

std::cout << "MyClass data: ";
obj2.print();

return 0;

}
```



### 虚函数

在C++中，虚函数（virtual function）是用于实现多态性的一种机制。通过虚函数，可以在基类中定义一个函数，并允许派生类重新定义（覆盖）该函数。在运行时，C++会根据对象的实际类型调用相应的函数实现，而不是编译时类型。这种特性使得基于继承的类体系结构更加灵活和可扩展。

#### 定义虚函数

虚函数是在基类中使用关键字 `virtual` 定义的。派生类可以覆盖这个函数。以下是一个示例：

```c++
#include <iostream>

// 基类
class Base {
public:
    virtual void show() {
        std::cout << "Base class show function" << std::endl;
    }
    

virtual ~Base() = default; // 虚析构函数

};

// 派生类
class Derived : public Base {
public:
    void show() override {
        std::cout << "Derived class show function" << std::endl;
    }
};

int main() {
    Base* b = new Derived();
    b->show(); // 调用的是Derived类的show函数
    delete b;
    return 0;
}
```

#### 虚函数表（VTable）

虚函数的实现依赖于虚函数表（VTable）。每个包含虚函数的类都有一个虚函数表，表中记录了类的虚函数指针。在运行时，通过该表来调用正确的函数实现。

#### 纯虚函数和抽象类

纯虚函数是没有实现的虚函数，用于定义接口。在基类中定义纯虚函数使得该类成为抽象类，不能直接实例化。

```c++
class AbstractBase {
public:
    virtual void pureVirtualFunction() = 0; // 纯虚函数
};

class ConcreteDerived : public AbstractBase {
public:
    void pureVirtualFunction() override {
        std::cout << "Implementation of pure virtual function" << std::endl;
    }
};

int main() {
    // AbstractBase ab; // 错误，不能实例化抽象类
    ConcreteDerived cd;
    cd.pureVirtualFunction(); // 正确，调用实现的纯虚函数
    return 0;
}
```

纯虚函数示例

```c++
class AbstractBase {
public:
    virtual void pureVirtualFunction() = 0; // 纯虚函数
};

class ConcreteDerived : public AbstractBase {
public:
    void pureVirtualFunction() override {
        std::cout << "Implementation of pure virtual function" << std::endl;
    }
};

int main() {
    // AbstractBase ab; // 错误，不能实例化抽象类
    ConcreteDerived cd;
    cd.pureVirtualFunction(); // 正确，调用实现的纯虚函数
    return 0;
}


```

### 接口

C++ 中的接口（Interfaces）是一种抽象类，用于定义一组函数，而不提供其实现。接口定义了一些函数原型，任何实现接口的类必须提供这些函数的具体实现。接口可以用于多态性和代码的解耦。

#### 定义接口

在 C++ 中，接口通常是一个只有纯虚函数的类。纯虚函数的声明如下：

```c++
virtual void FunctionName() = 0;
```

例如，定义一个名为 `IMyInterface` 的接口：

```c++
class IMyInterface {
public:
    virtual ~IMyInterface() = default; // 虚析构函数，确保正确释放资源
    virtual void MyFunction() = 0;     // 纯虚函数，子类必须实现
};
```

#### 实现接口

任何类实现这个接口都必须提供 `MyFunction` 的具体实现。

```c++
class MyClass : public IMyInterface {
public:
    void MyFunction() override {
        // 提供具体实现
        std::cout << "MyFunction implementation in MyClass" << std::endl;
    }
};
```

#### 完整示例

```c++
#include <iostream>

// 定义接口
class IMyInterface {
public:
    virtual ~IMyInterface() = default;
    virtual void MyFunction() = 0;
};

// 实现接口的类
class MyClass : public IMyInterface {
public:
    void MyFunction() override {
        std::cout << "MyFunction implementation in MyClass" << std::endl;
    }
};

// 另一个实现接口的类
class AnotherClass : public IMyInterface {
public:
    void MyFunction() override {
        std::cout << "MyFunction implementation in AnotherClass" << std::endl;
    }
};

// 使用接口的函数
void ExecuteFunction(IMyInterface& obj) {
    obj.MyFunction();
}

int main() {
    MyClass obj1;
    AnotherClass obj2;

ExecuteFunction(obj1); // 调用 MyClass 的实现
ExecuteFunction(obj2); // 调用 AnotherClass 的实现

return 0;

}
```

#### 使用场景

多态性：接口允许你编写使用接口而不是具体实现的代码，从而实现多态性。例如，你可以编写一个函数，接受接口类型的参数，而不是具体类的参数。

解耦：接口可以将使用者与实现者解耦，使代码更加模块化和易于维护。

扩展性：通过使用接口，你可以轻松地添加新的实现而不需要修改使用接口的代码。

#### 在Unreal Engine中的使用

在Unreal Engine中，接口的定义和实现稍有不同。Unreal Engine使用宏定义和UINTERFACE来声明接口。

##### 1.定义接口

```c++
// 声明接口
UINTERFACE(MinimalAPI)
class UMyInterface : public UInterface {
    GENERATED_BODY()
};

// 定义接口
class IMyInterface {
    GENERATED_BODY()

public:
    virtual void MyFunction() = 0;
};
```

##### 2.实现接口

```c++
class AMyActor : public AActor, public IMyInterface {
    GENERATED_BODY()

public:
    void MyFunction() override {
        // 提供具体实现
        UE_LOG(LogTemp, Warning, TEXT("MyFunction implementation in AMyActor"));
    }
};
```

##### 3.使用接口

```c++
void ExecuteFunction(IMyInterface* Interface) {
    if (Interface) {
        Interface->MyFunction();
    }
}

AMyActor* MyActor = GetWorld()->SpawnActor<AMyActor>();
ExecuteFunction(Cast<IMyInterface>(MyActor));
```

#### 总结

接口是C++中实现多态性和解耦的一种重要手段。通过定义纯虚函数的类来创建接口，并在具体的类中实现这些函数，可以使代码更加灵活和可扩展。在Unreal Engine中，接口同样被广泛使用，通过UINTERFACE和UObject体系，提供了强大的面向对象编程支持。

## 标准模板库（STL，Standard Template Library）

C++ 标准模板库（STL，Standard Template Library）是 C++ 标准库的一部分，提供了一套通用的模板类和函数，用于数据结构和算法的实现。STL 是 C++ 中非常重要和强大的工具，涵盖了容器、迭代器、算法和函数对象（仿函数）等。以下是 STL 的详细介绍：

#### 容器（Containers）

容器是 STL 的核心部分，用于存储和管理数据。常见的 STL 容器包括：

##### 序列式容器（Sequence Containers）

**vector**：动态数组，支持快速随机访问和尾部插入/删除。

```c++
std::vector<int> vec = {1, 2, 3, 4};
vec.push_back(5);
```

**deque**：双端队列，支持快速随机访问和头尾部插入/删除。

```c++
std::deque<int> deq = {1, 2, 3, 4};
deq.push_front(0);
```

**list**：双向链表，支持快速插入/删除，但不支持随机访问。

```c++
std::list<int> lst = {1, 2, 3, 4};
lst.push_back(5);
```

##### 关联式容器（Associative Containers）

**set**：集合，元素唯一且自动排序。

```c++
std::set<int> s = {1, 2, 3, 4};
s.insert(5);
```

**map**：映射，键值对，键唯一且自动排序。

```c++
std::map<int, std::string> m;
m[1] = "one";
m[2] = "two";
```

**multiset**：多重集合，允许重复元素，自动排序。

```c++
std::multiset<int> ms = {1, 2, 2, 3};
ms.insert(2);
```

**multimap**：多重映射，允许重复键，自动排序。

```c++
std::multimap<int, std::string> mm;
mm.insert({1, "one"});
mm.insert({1, "uno"});
```

##### 无序容器（Unordered Containers）

**unordered_set**：哈希集合，元素唯一，无序。

```c++
std::unordered_set<int> us = {1, 2, 3, 4};
us.insert(5);
```

**unordered_map**：哈希映射，键值对，键唯一，无序。

```c++
std::unordered_map<int, std::string> um;
um[1] = "one";
um[2] = "two";
```

**unordered_multiset**：哈希多重集合，允许重复元素，无序。

```c++
std::unordered_multiset<int> ums = {1, 2, 2, 3};
ums.insert(2);
```

**unordered_multimap**：哈希多重映射，允许重复键，无序。

```c++
std::unordered_multimap<int, std::string> umm;
umm.insert({1, "one"});
umm.insert({1, "uno"});
```

#### 迭代器（Iterators）

迭代器是指针的抽象，用于遍历容器中的元素。常见的迭代器类型包括：

- **输入迭代器（Input Iterator）**：只读迭代器，用于从容器中读取数据。
- **输出迭代器（Output Iterator）**：只写迭代器，用于向容器中写入数据。
- **前向迭代器（Forward Iterator）**：既可读也可写，可以向前遍历。
- **双向迭代器（Bidirectional Iterator）**：既可读也可写，可以向前和向后遍历。
- **随机访问迭代器（Random Access Iterator）**：既可读也可写，可以进行随机访问。

```c++
std::vector<int> vec = {1, 2, 3, 4};
for (std::vector<int>::iterator it = vec.begin(); it != vec.end(); ++it) {
    std::cout << *it << " ";
}
```

#### 算法（Algorithms）

STL 提供了一系列通用算法，可以作用于容器，例如排序、搜索、复制、变换等。常见的算法包括：

**std::sort**：排序

```c++
std::sort(vec.begin(), vec.end());
```

**std::find**：查找

```c++
auto it = std::find(vec.begin(), vec.end(), 3);
```

**std::copy**：复制

```c++
std::vector<int> vec2(4);
std::copy(vec.begin(), vec.end(), vec2.begin());
```

**std::transform**：变换

```c++
std::transform(vec.begin(), vec.end(), vec.begin(), [](int x) { return x * 2; });
```

#### 函数对象（Function Objects）和Lambda表达式

函数对象是重载了`operator()`的对象，可以像函数一样调用。Lambda表达式是一种简洁的函数对象定义方式。

```c++
#include <algorithm>
#include <iostream>
#include <vector>

struct Multiply {
    int operator()(int x) const { return x * 2; }
};

int main() {
    std::vector<int> vec = {1, 2, 3, 4};
    std::transform(vec.begin(), vec.end(), vec.begin(), Multiply());
    

for (int x : vec) {
    std::cout << x << " ";
}

// 使用Lambda表达式
std::transform(vec.begin(), vec.end(), vec.begin(), [](int x) { return x * 2; });

for (int x : vec) {
    std::cout << x << " ";
}
return 0;

}
```

#### STL 使用示例

以下是一个综合示例，展示了如何使用STL容器、迭代器和算法：

```c++
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> vec = {4, 2, 5, 1, 3};

// 排序
std::sort(vec.begin(), vec.end());

// 查找
auto it = std::find(vec.begin(), vec.end(), 3);
if (it != vec.end()) {
    std::cout << "Found: " << *it << std::endl;
} else {
    std::cout << "Not found" << std::endl;
}

// 复制
std::vector<int> vec2(vec.size());
std::copy(vec.begin(), vec.end(), vec2.begin());

// 打印
for (const int& val : vec2) {
    std::cout << val << " ";
}
std::cout << std::endl;

// 变换
std::transform(vec2.begin(), vec2.end(), vec2.begin(), [](int x) { return x * 2; });

// 打印
for (const int& val : vec2) {
    std::cout << val << " ";
}
std::cout << std::endl;

return 0;

}
```

以上介绍了 C++ STL 的主要部分及其使用方法。STL 提供了强大的工具来管理数据和执行常见的算法，使得 C++ 编程更加高效和简洁。

### 线程池

在C++中，线程池（Thread Pool）是一种高效管理和使用线程的技术。线程池可以避免频繁创建和销毁线程的开销，允许复用线程以执行多个任务，从而提高程序的并发性能。

#### 线程池的基本概念

**任务队列**：存储需要执行的任务，每个任务通常是一个可调用对象（如`std::function`）。

**工作线程**：线程池中的线程，从任务队列中取出任务执行。

**任务调度器**：负责将任务添加到任务队列，并通知工作线程有新任务可以执行。

#### 实现线程池的步骤

##### 包含必要的头文件

```c++
#include <iostream>
#include <vector>
#include <queue>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <functional>
#include <future>
#include <memory>
```

##### 定义线程池类

```c++
class ThreadPool {
public:
    // 构造函数，初始化线程池并启动工作线程
    ThreadPool(size_t threads);

// 提交任务到线程池
template<class F, class... Args>
auto enqueue(F&& f, Args&&... args) -> std::future<typename std::result_of<F(Args...)>::type>;

// 析构函数，停止所有线程
~ThreadPool();

private:
    // 线程池中的工作线程
    std::vector<std::thread> workers;

// 任务队列
std::queue<std::function<void()>> tasks;

// 同步和互斥机制
std::mutex queue_mutex;
std::condition_variable condition;
bool stop;

};
```

##### 实现构造函数和析构函数

```c++
// 构造函数
ThreadPool::ThreadPool(size_t threads) : stop(false) {
    for (size_t i = 0; i < threads; ++i) {
        workers.emplace_back(
            [this] {
                for (;;) {
                    std::function<void()> task;

​                {
​                    std::unique_lock<std::mutex> lock(this->queue_mutex);
​                    this->condition.wait(lock, [this] { return this->stop || !this->tasks.empty(); });
​                    if (this->stop && this->tasks.empty())
​                        return;
​                    task = std::move(this->tasks.front());
​                    this->tasks.pop();
​                }

​                task();
​            }
​        }
​    );
}

}

// 析构函数
ThreadPool::~ThreadPool() {
    {
        std::unique_lock<std::mutex> lock(queue_mutex);
        stop = true;
    }
    condition.notify_all();
    for (std::thread &worker : workers)
        worker.join();
}
```

##### 实现任务提交方法

```
template<class F, class... Args>
auto ThreadPool::enqueue(F&& f, Args&&... args) -> std::future<typename std::result_of<F(Args...)>::type> {
    using return_type = typename std::result_of<F(Args...)>::type;

auto task = std::make_shared<std::packaged_task<return_type()>>(
    std::bind(std::forward<F>(f), std::forward<Args>(args)...)
);

std::future<return_type> res = task->get_future();
{
    std::unique_lock<std::mutex> lock(queue_mutex);

​    // 不允许在停止的线程池中添加任务
​    if (stop)
​        throw std::runtime_error("enqueue on stopped ThreadPool");

​    tasks.emplace([task]() { (*task)(); });
}
condition.notify_one();
return res;

}
```



#### 使用线程池

```c++
#include <iostream>
#include <vector>
#include <chrono>

// 假设已经定义了 ThreadPool

int main() {
    ThreadPool pool(4);

std::vector<std::future<int>> results;

for (int i = 0; i < 8; ++i) {
    results.emplace_back(
        pool.enqueue([i] {
            std::cout << "task " << i << " is running" << std::endl;
            std::this_thread::sleep_for(std::chrono::seconds(1));
            return i * i;
        })
    );
}

for (auto && result : results)
    std::cout << result.get() << ' ';
std::cout << std::endl;

return 0;

}
```

在这个示例中，创建了一个包含4个线程的线程池，然后提交了8个任务给线程池执行。每个任务计算一个整数的平方并返回结果。最后打印所有任务的结果。

#### 进一步优化和扩展

**动态调整线程池大小**：可以实现动态调整线程池中线程数量的功能，以便根据负载自动调整。

**任务优先级**：可以扩展任务队列以支持优先级，从而优先执行重要任务。

**错误处理**：在任务执行过程中添加异常处理，防止线程池因未处理的异常而崩溃。

## 设计模式

#### 创建型模式

##### 1.单例模式（Singleton）

单例模式确保一个类只有一个实例，并提供全局访问点。

```c++
class Singleton {
private:
    static Singleton* instance;
    Singleton() {} // 私有构造函数

public:
    static Singleton* getInstance() {
        if (instance == nullptr) {
            instance = new Singleton();
        }
        return instance;
    }
};

// 静态成员变量初始化
Singleton* Singleton::instance = nullptr;
```

##### 2.工厂模式（Factory Method）

工厂模式定义一个用于创建对象的接口，让子类决定实例化哪一个类。

```c++
class Product {
public:
    virtual void use() = 0;
};

class ConcreteProductA : public Product {
public:
    void use() override {
        std::cout << "Using ConcreteProductA" << std::endl;
    }
};

class ConcreteProductB : public Product {
public:
    void use() override {
        std::cout << "Using ConcreteProductB" << std::endl;
    }
};

class Factory {
public:
    virtual Product* createProduct() = 0;
};

class FactoryA : public Factory {
public:
    Product* createProduct() override {
        return new ConcreteProductA();
    }
};

class FactoryB : public Factory {
public:
    Product* createProduct() override {
        return new ConcreteProductB();
    }
};
```

##### 3.抽象工厂模式（Abstract Factory）

抽象工厂模式提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。

```c++
class ProductA {
public:
    virtual void use() = 0;
};

class ProductB {
public:
    virtual void eat() = 0;
};

class ConcreteProductA1 : public ProductA {
public:
    void use() override {
        std::cout << "Using ConcreteProductA1" << std::endl;
    }
};

class ConcreteProductB1 : public ProductB {
public:
    void eat() override {
        std::cout << "Eating ConcreteProductB1" << std::endl;
    }
};

class AbstractFactory {
public:
    virtual ProductA* createProductA() = 0;
    virtual ProductB* createProductB() = 0;
};

class ConcreteFactory1 : public AbstractFactory {
public:
    ProductA* createProductA() override {
        return new ConcreteProductA1();
    }

ProductB* createProductB() override {
    return new ConcreteProductB1();
}

};
```

#### 结构型模式

##### 1.适配器模式（Adapter）

适配器模式将一个类的接口转换成客户希望的另一个接口，使原本由于接口不兼容而不能一起工作的那些类可以一起工作。

```c++
class Target {
public:
    virtual void request() {
        std::cout << "Target request" << std::endl;
    }
};

class Adaptee {
public:
    void specificRequest() {
        std::cout << "Adaptee specificRequest" << std::endl;
    }
};

class Adapter : public Target {
private:
    Adaptee* adaptee;

public:
    Adapter(Adaptee* adaptee) : adaptee(adaptee) {}

void request() override {
    adaptee->specificRequest();
}

};
```

##### 2.装饰器模式（Decorator）

装饰器模式动态地给一个对象添加一些额外的职责。就增加功能来说，装饰器模式相比生成子类更为灵活。

```c++
class Component {
public:
    virtual void operation() = 0;
};

class ConcreteComponent : public Component {
public:
    void operation() override {
        std::cout << "ConcreteComponent operation" << std::endl;
    }
};

class Decorator : public Component {
protected:
    Component* component;

public:
    Decorator(Component* component) : component(component) {}

void operation() override {
    if (component) {
        component->operation();
    }
}

};

class ConcreteDecoratorA : public Decorator {
public:
    ConcreteDecoratorA(Component* component) : Decorator(component) {}

void operation() override {
    Decorator::operation();
    std::cout << "ConcreteDecoratorA operation" << std::endl;
}

};
```

#### 行为型模式

##### 1.观察者模式（Observer）

观察者模式定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。

```c++
class Observer {
public:
    virtual void update() = 0;
};

class Subject {
private:
    std::vector<Observer*> observers;

public:
    void attach(Observer* observer) {
        observers.push_back(observer);
    }

void detach(Observer* observer) {
    observers.erase(std::remove(observers.begin(), observers.end(), observer), observers.end());
}

void notify() {
    for (Observer* observer : observers) {
        observer->update();
    }
}

};

class ConcreteObserver : public Observer {
public:
    void update() override {
        std::cout << "ConcreteObserver update" << std::endl;
    }
};
```

##### 2.策略模式（Strategy）

策略模式定义一系列算法，把它们一个个封装起来，并且使它们可互相替换。本模式使得算法可独立于使用它的客户而变化。

```c++
class Strategy {
public:
    virtual void execute() = 0;
};

class ConcreteStrategyA : public Strategy {
public:
    void execute() override {
        std::cout << "ConcreteStrategyA execute" << std::endl;
    }
};

class ConcreteStrategyB : public Strategy {
public:
    void execute() override {
        std::cout << "ConcreteStrategyB execute" << std::endl;
    }
};

class Context {
private:
    Strategy* strategy;

public:
    void setStrategy(Strategy* strategy) {
        this->strategy = strategy;
    }

void executeStrategy() {
    if (strategy) {
        strategy->execute();
    }
}

};
```



### 强制类型转换

```c++
#include <iostream>

class Base {
public:
    virtual void show() {
        std::cout << "Base class show function\n";
    }
};

class Derived : public Base {
public:
    void show() override {
        std::cout << "Derived class show function\n";
    }
};
```

#### static_cast

static_cast 是一种在编译时进行的类型转换，适用于已知类型关系的情况。它主要用于安全的向上转换（子类到父类）和向下转换（父类到子类），但不会进行运行时检查。

向上转换（子类到父类）

```c++
Derived* derived = new Derived();
Base* base = static_cast<Base*>(derived);
base->show(); // Output: Derived class show function
```

向下转换（父类到子类）

```c++
Base* base = new Derived();
Derived* derived = static_cast<Derived*>(base);
derived->show(); // Output: Derived class show function
```

**注意**: static_cast在向下转换时不会进行运行时类型检查，因此，如果类型不匹配会导致未定义行为。

#### dynamic_cast

```c++
Derived* derived = new Derived();
Base* base = dynamic_cast<Base*>(derived);
base->show(); // Output: Derived class show function
```



```c++
Derived* derived = new Derived();
Base* base = dynamic_cast<Base*>(derived);
base->show(); // Output: Derived class show function
```



#### const_cast

#### reinterpret_cast



### 代码

```c++
//定义了 `APawn` 类的一个静态成员变量 `Controller`，它是一个 `TObjectPtr` 类型的智能指针，指向 `AController` 类对象
TObjectPtr<AController> APawn::Controller;
```

​	A.h

```c++
#pragma once

class ClassA
{
public:
    void PublicFunction();
protected:
    void ProtectedFunction();
private:
    void PrivateFunction();
};
```

B.h

```c++
#pragma once

#include "ClassA.h"

class ClassB
{
public:
    void CallFunctions(ClassA& a);
};
```

B.cpp

```c++
#include "ClassB.h"
#include <iostream>

void ClassB::CallFunctions(ClassA& a)
{
    a.PublicFunction();      // 可以调用

// a.ProtectedFunction(); // 编译错误，无法调用受保护的函数

// a.PrivateFunction();   // 编译错误，无法调用私有的函数

}
```



A.h

```c++
#pragma once

class A
{
public:
    void DoSomething();
};
```

B.h

```c++
#pragma once
#include "A.h"

class B
{
public:
    A* get();
private:
    A* aInstance;
};
```

B.cpp

```c++
#include "B.h"

A* B::get()
{
    return aInstance; //可以访问
}
```







```c++
// B.h
#pragma once

class B
{
public:
    void PublicMethod();
};

// B.cpp
#include "B.h"
#include <iostream>

void B::PublicMethod()
{
    std::cout << "B::PublicMethod() called" << std::endl;
}

// A.h
#pragma once
#include "B.h"

class A
{
public:
    A();
    void CallBMethod();

private:
    B bInstance;
};

// A.cpp
#include "A.h"

A::A()
{
    // 初始化成员变量 bInstance
}

void A::CallBMethod()
{
    bInstance.PublicMethod();
}
```

main.cpp

```c++
// main.cpp
#include "A.h"

int main()
{
    A a;
    a.CallBMethod();  // 通过 A 的方法间接调用 B 的方法

A* aPtr = &a;
aPtr->CallBMethod();  // 使用指向 A 的指针间接调用 B 的方法

return 0;

}


```

