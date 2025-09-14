# 高级编程 - 作业4
<p align="center"><b>作业4 - 2022年春季学期<br>截止日期： Ordibehesht 13日（星期二）晚上11:59</b></p>

## 概述

在本次作业中，我们要实现自己的智能指针。具体来说，我们要实现自定义的`SharedPtr`和`UniquePtr`类，使其具备`std::shared_ptr`和`std::unique_ptr`的几乎所有功能。

我们要实现两个模板类，分别名为`UniquePtr`和`SharedPtr`，并为其添加以下各节所描述的函数。

**注意**：你只允许修改`unique_ptr.hpp/h`、`shared_ptr.hpp/h`以及`main.cpp`的调试部分。

</br>

# UniquePtr类
定义一个名为`UniquePtr`的模板类，并向该类中添加以下函数。

该类应使用一个名为`T* _p`（T是模板参数）的成员变量来存储给定的指针。

- **构造函数**
为你的类实现一个构造函数，使下面的代码能够正常工作。你的构造函数应能使用`_p`变量将给定的动态指针正确存储在类中。

```cpp
UniquePtr<int> ptr{new int{10}};
```

- **make_unique**（类外部）
构造`std::unique_ptr`的推荐方式是使用一个名为`std::make_unique`的函数。实现一个类似的函数，使以下代码能够工作：

```cpp
UniquePtr<int> ptr{make_unique<int>(10)};
```

- **默认构造函数**
为你的类实现一个默认构造函数，使下面的代码能够工作，并将`nullptr`赋值给`_p`。

```cpp
UniquePtr<int> ptr;
```

- **析构函数**
如你所知，当在类中处理动态指针时，实现析构函数是必要的，因此要实现一个合适的析构函数并删除动态指针（提示：删除后将其赋值为`nullptr`）。

```cpp
~UniquePtr()
```

- **拷贝构造函数**
如你所知，不能拷贝`UniquePtr`，请进行设置，使以下代码会导致编译错误。

```cpp
UniquePtr<int> ptr1{new int{10}};
UniquePtr<int> ptr2{ptr1};
```

- **运算符=**
与上一节完全一样，我们也不应该能够编写以下代码。让编译器为此代码产生错误。

```cpp
UniquePtr<int> ptr1{new int{10}};
UniquePtr<int> ptr2{new int{11}};
ptr2 = ptr1;
```

- **get**
get()函数应返回存储在类中的原始指针。

```cpp
UniquePtr<int> ptr{new int{10}};
std::cout << ptr.get() << std::endl; // 输出：类中存储的原始指针
```

- **运算符***
智能指针应该能够像原始指针一样被解引用。使以下代码能够工作：

```cpp
UniquePtr<int> ptr{new int{10}};
std::cout << *ptr << std::endl; // 输出：10
```

- **运算符->**
智能指针可以像普通指针一样使用箭头运算符。也使以下代码能够工作：

```cpp
UniquePtr<std::string> ptr{new std::string{"hello"}};
std::cout << ptr->length() << std::endl; // 输出：5
```

- **reset**
reset()函数将删除指针并为其赋值`nullptr`：

```cpp
void reset();
```

- **reset**
reset()函数可以接受一个输入，在删除旧指针后用该输入创建一个新指针：

```cpp
UniquePtr<std::string> ptr{new std::string{"hello"}};
ptr.reset(new std::string{"nice"});
std::cout << *ptr << std::endl; // 输出：nice
```

- **release**
release()函数返回类中存储的指针（类似于get），但有两个区别：调用release()后，UniquePtr类将不再存储该指针，并且该指针的删除也不应再由UniquePtr类来完成。

```cpp
UniquePtr<double> ptr{new double{1.567}};
double *tmp{ptr.release()};
std::cout << *tmp << std::endl; // 输出：1.567
delete tmp; // 手动删除
```


</br>

# SharedPtr类
定义一个名为`SharedPtr`的模板类，并向该类中添加以下函数。

该类应使用一个名为`T* _p`（T是模板参数）的成员变量来存储给定的指针。


- **构造函数**
为你的类实现一个构造函数，使下面的代码能够正常工作。你的构造函数应能使用`_p`变量将给定的动态指针正确存储在类中。

```cpp
UniquePtr<int> ptr{new int{10}};
```

- **make_shared**（类外部）
构造`std::shared_ptr`的推荐方式是使用一个名为`std::make_shared`的函数。实现一个类似的函数，使以下代码能够工作。

```cpp
SharedPtr<int> ptr{make_shared<int>(10)};
```

- **默认构造函数**
为你的类实现一个默认构造函数，使下面的代码能够工作，并将`nullptr`赋值给`_p`。

```cpp
SharedPtr<int> ptr;
```

- **析构函数**
如你所知，当在类中处理动态指针时，实现析构函数是必要的，因此要实现一个合适的析构函数并删除动态指针（不要忘记在删除后赋值为`nullptr`）。

```cpp
~SharedPtr()
```

- **拷贝构造函数**
如你所知，`SharedPtr`和`UniquePtr`类的一个关键区别是，我们可以使用拷贝构造函数来拷贝`SharedPtr`。因此下面的代码应能顺利运行。

```cpp
SharedPtr<int> ptr1{new int{10}};
SharedPtr<int> ptr2{ptr1};
```

- **运算符=**
与上一节完全一样，`SharedPtr`可以有`operator=`。同样，下面的代码应能无错误地运行。

```cpp
SharedPtr<int> ptr1{new int{10}};
SharedPtr<int> ptr2{new int{11}};
ptr2 = ptr1;
```

- **use_count**
在`SharedPtr`中，我们应该能够统计指向同一位置的实例数量。要做到这一点，你必须为`SharedPtr`类定义另一个成员变量，并跟踪这个数量。

**注意**：你可能需要对之前的函数（如构造函数等）做一些调整来实现这一点。

```cpp
SharedPtr<int> ptr1{make_shared<int>(10)};
std::cout << ptr1.use_count() << std::endl; // 输出：1
SharedPtr<int> ptr2{ptr1};
std::cout << ptr1.use_count() << std::endl; // 输出：2
std::cout << ptr2.use_count() << std::endl; // 输出：2
```

- **get**
get()函数应返回存储在类中的原始指针。

```cpp
SharedPtr<int> ptr{new int{10}};
std::cout << ptr.get() << std::endl; // 输出：类的原始指针
```

- **运算符***
智能指针应该能够像原始指针一样被解引用。使以下代码能够工作：

```cpp
SharedPtr<int> ptr{new int{10}};
std::cout << *ptr << std::endl; // 输出：10
```

- **运算符->**
智能指针可以像原始指针一样使用箭头运算符。使以下代码能够工作：

```cpp
SharedPtr<std::string> ptr{new std::string{"hello"}};
std::cout << ptr->length() << std::endl; // 输出：5
```

- **reset**
reset()函数将删除指针并为其赋值`nullptr`：

```cpp
void reset();
```

- **reset**
reset()函数可以接受一个输入，在删除旧指针后用该输入创建一个新指针：

```cpp
SharedPtr<std::string> ptr{new std::string{"hello"}};
ptr.reset(new std::string{"nice"});
std::cout << *ptr << std::endl; // 输出：nice
```

</br>

# 挑战
- 如果你到达了这一部分，恭喜你，只剩下一个部分了。进行设置，使你能够在`if`条件中使用自定义的智能指针，当智能指针包含`nullptr`时，条件返回`false`，否则返回`true`。

```cpp
UniquePtr<double> ptr{new double{1.567}};
if(ptr) // => true
    // 一些操作
ptr.reset();
if(ptr) // => false
    // 另一些操作
```
为`UniquePtr`和`SharedPtr`类都进行这样的设置。
</br>

# 最后
如前所述，除另有说明外，不要修改已填充的其他文件。如果你想测试你的代码，可以使用`main.cpp`的`debug`部分。

```cpp
if (true) // 设为false以运行单元测试
{ 
    // 调试部分 
} 
else 
{ 
    ::testing::InitGoogleTest(&argc, argv); 
    std::cout << "RUNNING TESTS ..." << std::endl; 
    int ret{RUN_ALL_TESTS()}; 
    if (!ret) 
        std::cout << "<<<SUCCESS>>>" << std::endl; 
    else 
        std::cout << "FAILED" << std::endl; 
} 
return 0;
```
<br/>
<p align="center"><b>祝你好运</b></p>