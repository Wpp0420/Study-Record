

# PENNANT

[TOC]

### MPI与openmp


在并行计算中，有两种常见的通信模式：进程内通信和进程间通信。

MPI用于实现跨多个计算节点的通信，而OpenMP用于在单个计算节点上进行多线程并行计算。

It can be run solely with MPI or in a hybrid MPI+OpenMP setup. 

它可以仅使用 MPI 运行，也可以在 MPI+OpenMP 混合
设置中运行。

It uses OpenMP’s static loop-scheduling, and makes use of gather/scatter to send data to and from the root to other ranks, and reductions to consolidate partial results.

它使用 OpenMP 的静态循环调度，并利用利用收集/散布功能将数据从根节点发送到其他节点，并利用还原功能将数据从根节点还原到其他节点。和还原来合并部分结果。



### 网格算法流程

下面是网格算法的一般运行过程：

1. 网格生成：首先，需要生成一个二维或三维的网格结构，将需要计算的区域划分为网格单元。通常，网格单元是规则的正方形或立方体，但也可以是非规则形状。
2. 数据初始化：为了进行计算，需要对每个网格单元进行初始化，设置初始值或条件。这些初始值可以是物理量（如温度、压力）或其他需要计算的量。
3. 计算循环：接下来，进行迭代的计算循环，直到达到收敛条件。在每次循环中，每个进程或线程负责计算和更新其负责的网格单元。
4. 局部计算：在计算循环的每个步骤中，进程或线程根据所采用的算法，在其负责的网格单元上进行局部计算。这些计算通常涉及该网格单元内的数据和邻居网格单元的数据。
5. 边界处理：在网格算法中，边界的处理通常是一个重要的问题。进程或线程需要正确处理边界的特殊条件，以确保计算结果的准确性和一致性。
6. 数据通信：在某些情况下，网格单元之间的计算可能需要数据的通信和交换。进程或线程之间需要通过消息传递或共享内存等机制来交换数据。
7. 收敛检查：在每次计算循环的末尾，需要检查计算结果是否满足收敛条件。如果不满足条件，则继续进行下一次循环。否则，算法终止并输出最终的计算结果。

通过这个过程，网格算法可以在并行环境中高效地进行计算。每个进程或线程负责处理局部的网格单元，通过数据通信和协调，最终得到整个网格域的计算结果。这种并行化的计算可以提高计算效率和处理大规模问题的能力。

==网格划分==：

 PENNANT 使用自适应网格划分技术来对计算域进行网格划分。自适应网格划分技术根据流场的特征和需求，自动调整网格的大小和分辨率，以提高模拟精度和效率



PENNANT 的网格划分涉及以下几个步骤：

==初始网格生成==：首先，PENNANT 会根据用户的输入或默认设置生成一个初始网格。初始网格可以是均匀的或根据特定的几何形状进行划分。这个初始网格将作为基础网格进行后续的自适应划分。
流场评估：在模拟过程中，PENNANT 会根据流场的特征和需求对网格进行评估。评估可以基于流体力学方程、梯度、边界条件等相关参数。通过评估流场，PENNANT 可以确定哪些区域需要更细的网格分辨率，以获取更准确的结果。
==网格自适应划分==：根据流场评估，PENNANT 对网格进行自适应划分。自适应划分可以包括以下几种操作：

​	网格细化：对流场中需要更高分辨率的区域进行网格细化，以更准确地描述流场细节。
​	网格粗化：对流场中不需要高分辨率的区域进行网格粗化，以减少计算量和提高计算效率。
​	网格重构：基于流场评估结果，PENNANT 可能会对网格进行重构，以适应流场的变化和演化。

通过不断的网格划分和调整，PENNANT 可以==自动地适应流场的特征==，提供更精确和高效的流体力学模拟。
值得注意的是，具体的网格划分和自适应策略会根据不同的流体力学问题和模拟需求而有所不同。PENNANT 提供了一系列的网格划分和自适应选项，以供用户根据具体问题进行设置和调整。

### legion

https://legion.stanford.edu/

==Legion== 是一个以数据为中心的并行编程系统，用于编写针对分布式异构架构的可移植高性能程序。Legion 提供了抽象，允许程序员描述程序数据的属性（例如独立性、局部性）。通过使 Legion 编程系统了解程序数据的结构，它可以自动执行程序员当前面临的许多繁琐的任务，包括正确提取任务和数据级并行性以及在复杂的内存层次结构中移动数据。新颖的映射接口提供了由程序员明确控制的内存层次结构中数据的放置以及以与正确性正交的方式将任务分配给处理器的功能，从而可以轻松地将 Legion 应用程序移植和调整到新架构。

### ==并行计算框架==

 ==并行计算框架==是一种软件工具或平台，用于简化和管理并行计算任务的开发和执行。它提供了一组工具、库和接口，帮助开发人员有效地利用计算资源，并实现并行计算的任务分配、调度和协调。
并行计算框架通常提供以下功能和特性：

**并行编程模型**：提供一种抽象和编程模型，使开发人员能够描述和组织并行计算任务的结构和行为。这些模型可以是任务并行、数据并行或混合模型，帮助开发人员以并行的方式表达计算需求。
**任务调度和分发**：自动管理和调度并行任务的执行，将任务分配给可用计算资源，以实现高效的并行计算。这包括任务调度算法、负载均衡和任务优先级管理等。
**数据管理和通信**：提供数据管理和通信机制，使任务间可以进行数据交换和共享。这包括数据传输、数据共享、数据缓存和分布式内存等功能。
**资源管理和优化**：自动管理计算资源的分配和利用，以提高并行计算的性能和效率。这涉及资源调度、内存管理、优化技术和动态调整等。
**调试和性能分析**：提供工具和接口，帮助开发人员调试并行程序并分析其性能。这包括错误检测、性能分析器和可视化工具等。

 ==以数据为中心的并行编程系统==是一种编程模型和框架，将**数据的组织和访问**放在并行计算的核心。传统的并行计算模型主要关注**任务的分配和调度**。
在以数据为中心的并行编程系统中，程序员可以==定义数据的结构、属性和依赖关系==，并在这些数据上执行并行操作。

这些系统提供了**高级的抽象和接口**，使程序员能够以更高层次的语义描述并行计算任务，而无需过多关注底层的任务调度和同步细节。
这些系统通常支持数据并行性和任务并行性的自动发现和利用。数据并行性指的是将数据划分为多个部分，每个部分独立地进行计算，从而实现并行化。任务并行性指的是将计算任务划分为多个独立的子任务，每个子任务可以并行地执行。
以数据为中心的并行编程系统还提供了数据管理和移动的机制，以优化数据访问和传输。它们可以通过数据放置策略来决定数据在内存层次结构中的位置，以最小化数据移动和延迟。同时，这些系统还可以提供数据共享和通信的接口，以便不同任务之间可以交换和共享数据。
总之，以数据为中心的并行编程系统通过以数据为核心，提供了更高级的抽象和接口，以简化并行计算任务的开发和执行。它们自动管理数据并行性和任务并行性，并提供数据管理、移动和通信的机制，以优化性能和可扩展性。

### pennant

实现了从LANL放射流体动力学代码FLAG中的基本物理学的一个小子集，只包括足够运行一些标准测试问题的部分。

![image-20231108140720659](C:\Users\WP\AppData\Roaming\Typora\typora-user-images\image-20231108140720659.png)

 PENNANT的MPI并行化（类似于FLAG和许多其他代码）。
私有（private），主节点（master），从节点（slave）。
代理（proxy）（临时代理，用于通信）

几何域的领域分解到MPI秩（MPI ranks）。
区域（zones），面（sides），角点（corners）对每个秩（rank）是私有的。
边界点在秩之间是共享的，在秩之间重复；使用MPI运行时调用实现gather-sum-scatter。

![image-20231108141023749](C:\Users\WP\AppData\Roaming\Typora\typora-user-images\image-20231108141023749.png)

PENNANT” 是一个计算流体力学（Computational Fluid Dynamics，简称CFD）软件，
PENNANT 旨在提供一个高性能、可扩展、灵活和易于使用的CFD软件解决方案。它使用了大规模并行计算技术，可以在超级计算机和高性能计算集群上进行高效的流体力学模拟。PENNANT 软件使用了数值方法和数值算法来解决流体力学方程，并提供了各种模型和选项，以适应不同类型和复杂度的流体力学问题。
PENNANT 的主要特点包括：

==并行计算==：PENNANT 可以利用大规模并行计算资源，实现高性能的流体力学模拟。
==多物理场耦合==：PENNANT 支持多种物理场的耦合模拟，包括流体力学、热传导、化学反应等。
==自适应网格==：PENNANT 支持自适应网格技术，能够根据流场特征自动调整网格，提高模拟精度和效率。
==高精度数值方法==：PENNANT 使用高阶数值方法，如高阶有限体积法（High-Order Finite Volume Method）和高阶有限元法（High-Order Finite Element Method），以提供更准确和稳定的数值解。



==并行模型==：PENNANT 使用了并行计算模型，通过将任务分配给多个处理器或计算节点，以实现并行计算。它支持==共享内存并行==（如 OpenMP）和==分布式内存并行==（如 MPI）两种并行模型。
==网格划分==：PENNANT 将计算域划分为多个区域或子域，并将每个区域分配给不同的处理器或计算节点。这种划分可以使不同的处理器并行计算各自的区域，从而加快整体计算速度。
==任务分配和负载均衡==：PENNANT 通过动态任务分配和负载均衡技术，将计算任务均匀分配给不同的处理器或计算节点。这样可以确保每个处理器或计算节点都能充分利用计算资源，避免出现计算能力浪费或负载不均衡的情况。
==通信优化==：PENNANT 在并行计算中优化了数据通信和同步操作，以减少通信开销和延迟。它使用高效的通信库和算法，以最小化处理器之间的数据传输和同步操作，从而提高并行计算效率。
==可扩展性==：PENNANT 的并行计算设计具有良好的可扩展性，可以根据计算资源的增加来扩展计算能力。它可以利用大规模并行计算资源，如超级计算机和高性能计算集群，进行高性能的流体力学模拟。

这些并行计算设计和优化使得 PENNANT 能够充分利用并行计算资源，实现高性能的流体力学模拟。对于大规模、复杂的流体力学问题，PENNANT 的并行计算能力可以提供更高的计算效率和精确度。



PENNANT 使用了两种常见的并行模型：共享内存并行和分布式内存并行。具体的实现方式取决于所使用的并行计算库和编程语言。

1. 共享内存并行：共享内存并行模型在多核或多处理器系统中使用共享内存来进行并行计算。PENNANT 可以使用 OpenMP 这样的并行计算库来实现共享内存并行。在共享内存并行模型中，不同的线程可以并发地访问共享内存，并执行各自的计算任务。PENNANT 可以将计算域划分成多个区域，并使用 OpenMP 的指令和指令集来实现并行任务的分配和同步。

   对于共享内存并行模型，PENNANT 可以使用 C++ 的线程库（如 std::thread）或并行计算库（如 OpenMP）来实现。这些库允许在多核或多处理器系统中创建并发的线程，从而实现共享内存并行计算。

2. 分布式内存并行：分布式内存并行模型在多个计算节点之间使用消息传递接口（Message Passing Interface，简称MPI）来进行并行计算。PENNANT 可以使用 MPI 库来实现分布式内存并行。在分布式内存并行模型中，不同的计算节点通过消息传递来交换数据和实现同步。PENNANT 可以将计算域划分成多个子域，并使用 MPI 的通信操作来实现不同计算节点之间的数据传输和同步。



### hpc应用

==并行计算能力==：WRE和VASP都是针对并行计算环境进行开发和优化的科学计算软件。它们能够有效地利用HPC系统中的多个处理器核心和计算节点，将计算任务分解为多个并行子任务，并通过高度并行化来加速计算过程。
==可扩展性==：HPC系统通常由大量的计算节点组成，每个节点具有多个处理器核心。WRE和VASP能够充分利用这些计算资源，实现良好的可扩展性。它们能够在大规模并行计算环境下，有效地处理大量数据和复杂的计算任务。
==内存管理==：WRE和VASP在设计上考虑到了内存管理的问题。它们能够有效地分配和管理计算所需的内存，避免内存溢出或资源浪费的问题。这对于在大规模计算中处理大量数据和复杂模型是至关重要的。
==优化算法和数据结构==：WRE和VASP使用了一系列优化算法和数据结构，以提高计算效率和准确性。这些算法和数据结构的选择是针对材料科学和化学计算问题的特点进行优化的，并考虑了HPC环境下的并行计算要求。



 

# 4.code

## 1.cpp与h文件区别

### 1.总结

一般来说，".cc" 文件是C源代码文件，也称为 “.cpp” 文件，用于实现具体的功能和逻辑。

而 “.hh” 文件是C的头文件，也称为 “.h” 文件，通常包含类、函数和变量的声明。

与 “.cc” 文件相比，".hh" 文件更多地用于提供接口和声明，而不包含具体的实现代码。

头文件通常用于在==多个源代码文件之间实现代码的共享和模块化，以便在编译时进行链接==。

总结来说，".cc" 文件一般用于实现功能逻辑，而 “.hh” 文件用于声明接口和共享代码。这种分离可以提高代码的可读性、可维护性和可重用性。

### 2.例子

假设我们有两个源代码文件：main.cc 和 helper.cc，以及一个头文件 helper.h。

helper.h 文件内容如下：

```cpp
#ifndef HELPER_H
#define HELPER_H

void printMessage();

#endif
```

helper.cc 文件内容如下：

```cpp
#include <iostream>
#include "helper.h"

void printMessage() {
    std::cout << "Hello from helper.cc!" << std::endl;
}
```

main.cc 文件内容如下：

```cpp
#include "helper.h"

int main() {
    printMessage();
    return 0;
}
```

这样，我们可以==将 `printMessage()` 函数的实现放在 helper.cc 文件中，并通过头文件 helper.h 在 main.cc 文件中访问该函数==。这样，我们实现了代码共享和模块化，使得代码更加可读和易于维护。



`math.h`：包含了数学函数的声明和定义，例如三角函数、指数函数、对数函数等。

```cpp
#include <cmath>

double squareRoot(double x) {
    return sqrt(x);
}

double sine(double x) {
    return sin(x);
}
```

```cpp
#include <vector>

void printVector(const std::vector<int>& vec) {
    for (int i : vec) {
        std::cout << i << " ";
    }
    std::cout << std::endl;
}

void sortVector(std::vector<int>& vec) {
    std::sort(vec.begin(), vec.end());
}
```

### `for (int i : vec)` 

是C++11引入的一种范围-based for循环，也称为范围for循环或foreach循环。它用于遍历一个范围内的元素，如容器或数组。

在这个语法中，`vec` 是一个容器（如`std::vector`），`int i` 则是迭代过程中的临时变量，起到遍历容器元素的作用。在每次迭代中，容器中的每个元素都会被依次赋值给 `i`，然后执行循环体中的代码。

以下是一个简单的示例，展示了如何使用范围-based for循环遍历一个整型向量：

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec = {1, 2, 3, 4, 5};

    for (int i : vec) {
        std::cout << i << " ";
    }

    return 0;
}
```

以上代码将输出：`1 2 3 4 5`，每个元素都会被打印出来。

范围-based for循环简化了遍历容器的过程，使代码更加简洁和易读。它适用于大多数需要遍历容器元素的情况，并且==在访问容器元素时是只读的，不会改变容器本身==。

### 3.宏定义 结构体声明  类声明

1、宏定义：头文件中可以包含宏定义，用于在代码中==定义常量、函数宏、条件编译==等。宏定义使用 `#define` 关键字，将一个==标识符与一个特定的值或代码片段关联起来==。

```cpp
#define PI 3.14159
#define MAX(a, b) ((a) > (b) ? (a) : (b))

// 使用宏定义
double radius = 5.0;
double circumference = 2 * PI * radius;

int maxNumber = MAX(10, 20); // 展开为 ((10) > (20) ? (10) : (20))
```

2、结构体声明：头文件中可以包含结构体的声明，结构体是一种用户自定义的数据类型，可以包含多个不同类型的成员变量。

```cpp
struct Point {
    int x;
    int y;
};

// 使用结构体
Point p;
p.x = 10;
p.y = 20;
```

3.类的声明：头文件中通常包含类的声明，类是一种用户自定义的数据结构，可以包含成员变量和成员函数。

```cpp
class Rectangle {
public:
    int width;
    int height;
    
    int calculateArea();
};

// 使用类
Rectangle r;
r.width = 5;
r.height = 10;
int area = r.calculateArea();
```

==在头文件中声明结构体和类可以让其他源代码文件在包含头文件后使用这些结构体和类的定义，而无需了解其具体实现==。这种分离允许在不暴露实现细节的情况下，实现代码的共享和模块化。

### 4.结构体和类在头文件中（区别）（共同）

需要注意的是，==结构体和类的定义通常会放在头文件的声明部分==，而==实际的实现代码则放在源代码文件中==，以确保头文件的可读性和可维护性。

#### 1.区别 （默认访问权限） （继承嵌套）  （默认构造函数） （类的封装）

==结构体（struct）==和==类（class）==在C++中是两种用户自定义的数据类型，它们有一些共同点，但也有一些==区别==。

1.默认访问权限：在结构体中，默认的成员访问权限是公共的（public），而在类中，默认的成员访问权限是私有的（private）。这意味着，==结构体中的成员变量和成员函数可以在结构体外部直接访问==，而==类中的成员变量和成员函数只能通过公共的接口（成员函数）进行访问==。

```cpp
struct Point {
    int x;  // 默认为 public
    int y;  // 默认为 public
};

class Rectangle {
public:
    int width;   // 默认为 private
    int height;  // 默认为 private
};
```

2.继承方式：==类可以使用继承==（inheritance）来实现类与类之间的关系，包括单继承和多继承。而==结构体只能通过嵌套==（nesting）的方式来组合其他结构体或类。

```cpp
class A : public B {  // 类的继承
    // ...
};

struct Point3D {
    int x;
    int y;
    int z;
};

struct Box {
    Point3D position;  // 嵌套结构体
    int width;
    int height;
};
```

3.默认构造函数：在==类==中，如果没有显式定义构造函数，编译器会自动生成==默认构造函数==。

而==结构体==中，如果没有定义任何构造函数，它也会有一个默认构造函数。但是，如果结构体中有自定义的构造函数，那么编译器将不会生成默认构造函数。

```cpp
class MyClass {
public:
    MyClass() {}  // 默认构造函数
};

struct MyStruct {
    MyStruct() {}  // 默认构造函数
};
```

#### (1)构造函数示例(结构体的默认自定义构造函数)（类 委托构造 带默认参数构造 拷贝构造）

结构体的构造函数示例：

```
struct Point {
    int x;
    int y;
    Point() {  // 默认构造函数
        x = 0;
        y = 0;
    }
    Point(int newX, int newY) {  // 自定义构造函数
        x = newX;
        y = newY;
    }
};

Point p1;  // 使用默认构造函数
Point p2(5, 10);  // 使用自定义构造函数
```


类的构造函数示例：

```
class Rectangle {
private:
    int width;
    int height;
public:
    Rectangle() {  // 默认构造函数
        width = 0;
        height = 0;
    }
    Rectangle(int w, int h) {  // 自定义构造函数
        width = w;
        height = h;
    }
    //!!!
    Rectangle(int size) : Rectangle(size, size) {}  // 委托构造函数
    //!!!
};

Rectangle r1;  // 使用默认构造函数
Rectangle r2(2, 4);  // 使用自定义构造函数
Rectangle r3(5);  // 使用委托构造函数
```

委托构造函数是指在一个构造函数中==调用同类的另一个构造函数来完成对象的初始化==。通过使用委托构造函数，可以避免在不同的构造函数中重复编写相同的初始化代码，提高代码的复用性和可维护性。
在上述示例中，Rectangle(int size) : Rectangle(size, size) {} 是一个带有默认参数的构造函数，它接受一个整数参数 size。在函数体中，==使用冒号初始化列表调用了类的另一个构造函数 Rectangle(int w, int h)==，并传递了相同的 size 值两次。
这意味着，当调用这个委托构造函数时，它实际上会调用 Rectangle(int w, int h) 构造函数，同时将 size 值传递给它的 w 和 h 参数，从而创建一个正方形的矩形对象。
例如，使用委托构造函数创建一个正方形矩形对象的示例：

```
class Rectangle {
private:
    int width;
    int height;
public:
    Rectangle(int w, int h) : width(w), height(h) {}
    Rectangle(int size) : Rectangle(size, size) {}
};

Rectangle r1(2, 4);  // 使用正常构造函数创建矩形对象
Rectangle r2(5);     // 使用委托构造函数创建正方形对象
```

类的带默认参数的构造函数示例：

```
class Circle {
private:
    double radius;
public:
    //!!!
    Circle(double r = 1.0) {  // 带有默认参数的构造函数
    //!!!                       
        radius = r;
    }
};

Circle c1;  // 使用默认参数1.0
Circle c2(3.5);  // 使用指定的半径3.5
```


类的拷贝构造函数示例：

```
class String {
private:
    char* buffer;
public:
    String(const char* str) {
        // 构造函数逻辑
    }
    String(const String& other) {
        // 拷贝构造函数
        // 通常需要深拷贝数据成员
    }
};

String s1("Hello");
String s2 = s1;  // 使用拷贝构造函数创建新对象
```

这些例子展示了结构体和类的构造函数的不同用法。构造函数用于初始化对象的成员变量，可以根据需要定义多个构造函数，并支持默认参数、委托构造函数和拷贝构造函数等特性，以满足不同的对象创建需求。

4.==类的封装性==：类通常更加注重数据的封装性和隐藏内部细节，它可以通过==私有成员变量和公共成员函数==来实现对数据的访问控制。

而结构体在设计上更加简单，更倾向于将数据暴露给外部。

#### 2.公共点（成员变量 函数）（封装性）（对象实例化）（构造函数：初始化列表）

 结构体（struct）和类（class）在C++中有以下一些共同点：

==成员变量==：结构体和类都可以包含成员变量，用于存储数据。这些成员变量可以是内置类型（如int、float等）或自定义类型（如结构体、类等）。

```
struct Point {
    int x;
    int y;
};

class Rectangle {
public:
    int width;
    int height;
};
```

==成员函数==：结构体和类都可以包含成员函数，用于操作和处理数据。这些成员函数可以访问和操作成员变量，并且可以定义返回值和参数。

```
struct Point {
    int x;
    int y;
    void printCoordinates() {
        std::cout << "Coordinates: (" << x << ", " << y << ")" << std::endl;
    }
};

class Rectangle {
public:
    int width;
    int height;
    int calculateArea() {
        return width * height;
    }
};
```

==封装性==：结构体和类都具有封装性，可以将数据和相关的操作函数封装在一起，提供了更加清晰和可维护的代码结构。通过封装，可以隐藏内部细节，只暴露必要的接口。
==对象实例化==：结构体和类都可以用来创建对象实例，通过声明变量并调用构造函数进行初始化。

```
Point p;
p.x = 10;
p.y = 20;

Rectangle r;
r.width = 5;
r.height = 10;
```


可以使用==构造函数==：结构体和类都可以定义构造函数，用于初始化对象的成员变量。构造函数会在对象创建时自动调用。

```
struct Point {
    int x;
    int y;
    Point(int xValue, int yValue) {
        x = xValue;
        y = yValue;
    }
};

class Rectangle {
public:
    int width;
    int height;
    Rectangle(int w, int h) : width(w), height(h) {}
};
```

#### (2)成员初始化列表

通过使用初始化列表，可以直接为成员变量赋值，而不需要在构造函数的函数体中进行赋值操作。这种方式更加高效和简洁，并且可以在成员变量的构造过程中进行一些额外的逻辑操作。

```
Rectangle(int w, int h) : width(w), height(h) {}
```

初始化==普通成员变量==：使用成员初始化列表来直接初始化类中的普通成员变量。

```
class Rectangle {
private:
    int width;
    int height;
public:
    Rectangle(int w, int h) : width(w), height(h) {}
};
```

在上述示例中，Rectangle 类的构造函数使用成员初始化列表直接初始化 width 和 height 成员变量。

初始化==常量成员变量==：使用成员初始化列表来初始化类中的常量成员变量。

```
class Circle {
private:
    const double pi;
    double radius;
public:
    Circle(double r) : pi(3.14159), radius(r) {}
};
```

在上述示例中，Circle 类的构造函数使用成员初始化列表来初始化 pi 常量成员变量。

初始化==引用成员变量：==使用成员初始化列表来初始化类中的引用成员变量。

```
class Car {
private:
    const std::string& model;
public:
    Car(const std::string& m) : model(m) {}
};
```

在上述示例中，Car 类的构造函数使用成员初始化列表来初始化 model 引用成员变量。

初始化==基类的构造函数==：使用成员初始化列表来初始化派生类中的基类的构造函数。

```
class Base {
public:
    Base(int x) {
        // 构造函数体
    }
};

class Derived : public Base {
public:
    Derived(int x, int y) : Base(x) {
        // 构造函数体
    }
};
```

在上述示例中，Derived 类使用成员初始化列表来初始化 Base 类的构造函数。
需要注意的是，成员初始化列表仅适用于构造函数，并且==不能在结构体中使用==，因为结构体没有构造函数的概念。结构体的成员变量可以在定义时直接初始化，或者在创建结构体对象时使用赋值语句进行初始化。

#### (3)成员函数（特殊的：构造函数）

 成员函数是定义在类（或结构体）内部的函数，它们可以访问类的成员变量和其他成员函数。成员函数是类的一部分，可以通过类的对象或指针来调用。
成员函数有以下几种类型：

==实例成员函数==：也称为非静态成员函数，它们是与类的对象关联的函数。实例成员函数访问和操作对象的成员变量，==可以被类的对象调用==。

```
class MyClass {
public:
    void instanceFunction() {
        // 实例成员函数的定义
    }
};

MyClass obj;
obj.instanceFunction();  // 调用实例成员函数
```

==静态成员函数==：也称为类成员函数，它们不==与类的对象关联，而是与类本身相关联==。静态成员函数没有this指针，因此不能访问非静态成员变量，只能访问静态成员变量。

```
class MyClass {
public:
    static void staticFunction() {
        // 静态成员函数的定义
    }
};

MyClass::staticFunction();  // 调用静态成员函数
```

==常量成员函数==：也称为常量方法，它们在成员函数的声明和定义中通过const关键字进行修饰。常量成员函数表示==该函数不会修改对象的状态==，因此==不能修改非静态成员变量==（除非成员变量被声明为mutable）。

```
class MyClass {
public:
    void constFunction() const {
        // 常量成员函数的定义
    }
};

const MyClass obj;
obj.constFunction();  // 调用常量成员函数
```

成员函数可以访问类的成员变量和其他成员函数，它们提供了在类内部定义和封装相关功能的机制。成员函数可以用于操作对象的数据、执行特定的任务，并与其他函数共同构成类的接口。

#### 3.应用场景

结构体（struct）和类（class)在C++中有不同的使用场景，下面举例说明一些常见的应用场景：

1.使用结构体（struct）的场景：

- 数据组织：结构体适用于简单的数据组织，用于存储多个相关的数据字段，并且这些字段通常是公共的。
- 数据传递：结构体可以用于传递数据块，如传递多个相关的参数给函数，或在不同的模块之间传递数据。
- 文件解析：当需要解析二进制或文本文件中的结构化数据时，可以使用结构体来表示和组织文件中的数据结构。
- 数据交换：在网络通信或进程间通信中，结构体可以用于定义消息格式，方便数据传输和解析。

```cpp
struct Point {
    int x;
    int y;
};

struct Person {
    std::string name;
    int age;
};

struct Employee {
    std::string name;
    std::string department;
    double salary;
};
```

2.使用类（class）的场景：

- 封装数据和行为：类适用于封装数据和相关的行为（成员函数），提供了一种更加高级的抽象方式，使代码更加模块化和可维护。
- 对象的建模：类可以用于建模现实世界的对象，如人、汽车等，通过类的成员变量和成员函数来表示对象的属性和行为，并且可以创建对象实例。
- 继承和多态：类可以使用继承和多态的特性来实现对象之间的关系，实现代码的重用和灵活性。
- 数据封装和访问控制：类通过封装数据和定义访问控制级别，提供了对数据的更精确的控制，以加强数据的安全性和合理性。

```cpp
class Shape {
public:
    virtual double calculateArea() = 0;
    virtual double calculatePerimeter() = 0;
};

class Circle : public Shape {
private:
    double radius;
public:
    Circle(double r) : radius(r) {}
    double calculateArea() override {
        return 3.14 * radius * radius;
    }
    double calculatePerimeter() override {
        return 2 * 3.14 * radius;
    }
};

class BankAccount {
private:
    std::string accountNumber;
    double balance;
public:
    BankAccount(const std::string& accNum, double initialBalance) : accountNumber(accNum), balance(initialBalance) {}
    void deposit(double amount) {
        balance += amount;
    }
    void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
        }
    }
};
```

### 5.`#ifndef MESH_HH_` 和 `#define MESH_HH_`

 是一种常见的宏定义组合，用于==防止头文件的重复包含==。

在C++中，头文件通常包含==类型定义、函数声明和类的定义==等内容。当多个源代码文件需要使用同一个头文件时，通常使用 `#include` 预处理指令来引入头文件。然而，如果==多个源代码文件中都包含了同一个头文件，那么在编译时会导致头文件的内容被重复定义，从而引发编译错误==。

为了解决这个问题，可以使用 `#ifndef` 和 `#define` 这对宏定义组合来包裹头文件的内容。具体步骤如下：

1. 在头文件的开头使用 `#ifndef` 判断宏是否已经定义。
2. 如果==宏没有定义（即头文件没有被包含过），则执行下面的代码块==。
3. 在代码块中使用 `#define` 来定义宏，并定义一个唯一的标识符，用于标记头文件已经被包含。
4. 在代码块的末尾，添加 `#endif` 来结束条件编译块。

这样，当一个源代码文件第一次包含头文件时，宏是未定义的，条件成立，头文件的内容会被包含进来。当其他源代码文件再次包含同一个头文件时，因为宏已经定义了，条件不成立，==头文件的内容会被忽略，避免了重复定义的问题==。

例如，假设有一个头文件 `mesh.hh`，其中包含了一些类和函数的声明。可以使用以下方式来防止重复包含：

```cpp
#ifndef MESH_HH_
#define MESH_HH_

// 头文件的内容

#endif // MESH_HH_
```

这种方式能够确保头文件只被包含一次，避免了重复定义带来的编译错误，同时也提高了编译效率。

### 6.多个头文件引发编译错误原因

 当多个源代码文件都包含了同一个头文件时，会导致头文件的内容被重复定义，从而==引发编译错误的原因==如下：

==重复定义变量==：如果头文件包含了==全局变量的定义==，那么当多个源代码文件都包含该头文件时，==每个源代码文件会生成一个独立的全局变量，导致重复定义==。

重复定义函数：如果头文件包含了函数的定义（而不只是声明），那么当多个源代码文件都包含该头文件时，每个源代码文件会生成一个独立的函数定义，导致重复定义。

重复定义类/结构体的成员函数：如果头文件中包含了类或结构体的成员函数的定义（而不只是声明），那么当多个源代码文件都包含该头文件时，每个源代码文件会生成一个独立的成员函数定义，导致重复定义。

这些==重复定义==的情况会导致编译器在链接阶段无法确定正确的符号引用，从而引发编译错误，如重复定义的错误（multiple definition error）或符号重复定义的错误（duplicate symbol error）。
为了避免这种情况，可以使用条件编译技术来防止头文件的重复包含，如前面所提到的 #ifndef 和 #define 组合。这样可以确保头文件只被包含一次，并且头文件的内容只会在第一次包含时被定义，后续的包含将会被忽略。
通过避免重复定义，可以确保每个变量、函数或成员函数只有一个定义，从而避免了编译错误，并保持了程序的一致性和正确性。

### 7.static constant public private

## 2.总变量统计

leblancbig

```
tstop   6.0
meshtype rect
meshparams 160 1440 1.0 9.0
int gnzx, gnzy;             // global number of zones, in x and y directions表示在 x 和 y 方向上的全局（总体）网格区域的区域数
    double lenx, leny;          // length of mesh sides, in x and y directions 网格的边长，分别对应 x 和 y 方向
subregion 0.0 1.0  3.0 9.0
//    std::vector<double> subregion; // bounding box for a subregion
                                   // if nonempty, should have 4 entries:
                                   // xmin, xmax, ymin, ymax
rinit   1.0
einit   0.1
    double rinit;               // initial density for main mesh
    double einit;               // initial energy for main mesh
rinitsub 1.0e-3
einitsub 1.0e-7
    double rinitsub;            // initial density in subregion
    double einitsub;            // initial energy in subregion
bcx     0.0 1.0
bcy     0.0 9.0
    std::vector<double> bcx;    // x values of x-plane fixed boundaries
    std::vector<double> bcy;    // y values of y-plane fixed boundaries
ssmin   0.1
double ssmin;                  // minimum sound speed
q1      0.3
q2      2.0
    double q1, q2;                 // linear and quadratic coefficients
                                   // for Q model Q模型的线性和二次系数
dtinit  1.e-3
double dtinit;                 // initial timestep size
 int dtreport;                  // frequency for timestep reports
     这段注释表示dtinit是初始时间步长的大小。
在数值计算中，时间步长（time step）是模拟过程中时间的离散间隔。它决定了模拟算法在每个时间步中更新系统状态的频率。初始时间步长则是在模拟开始时使用的时间步长大小。
dtinit被声明为一个双精度浮点数类型的变量，用于存储初始时间步长的大小。根据具体的模拟算法和需求，初始时间步长可以根据经验或计算需求进行选择和调整。通常，较小的时间步长可以提供更精确的结果，但也可能需要更长的计算时间。因此，在选择初始时间步长时需要权衡精度和计算效率的要求。

dtreport 100

 这段注释表示dtreport是用于时间步长报告的频率。
在数值计算中，时间步长报告的频率决定了在模拟过程中输出时间步长信息的间隔。通过定期报告时间步长的大小，可以帮助用户了解模拟的进展情况，并对模拟的稳定性和准确性进行评估。
dtreport被声明为一个整数类型的变量，用于存储时间步长报告的频率。具体来说，它表示每隔多少个时间步长，就会进行一次时间步长的报告。这个值可以根据模拟的要求和目的进行选择，以提供对时间步长变化的更详细的了解。
注意，时间步长报告的频率应该适度，避免过于频繁或过于稀疏。过于频繁的报告可能会导致输出信息过于冗长，而过于稀疏的报告可能会导致对时间步长变化的掌握不足。因此，需要根据具体情况选择适当的报告频率。

chunksize 512
int chunksize;                 // max size for processing chunks
 这段注释表示chunksize是用于处理块的最大大小。
在并行计算中，将任务划分为多个较小的块（chunks）可以提高计算效率和并行性。chunksize变量被声明为一个整数类型的变量，用于存储处理块的最大大小。这意味着任务将被分割成大小不超过chunksize的较小块进行并行处理。
通过将任务划分为较小的块，可以更好地利用计算资源，例如多核处理器或分布式计算系统。较小的块可以更均匀地分配到不同的计算单元中，从而提高计算效率和并行性。
具体来说，chunksize的值可以根据任务的性质、计算资源的可用性和性能需求进行选择和调整。通常，较大的chunksize可以减少任务划分和通信的开销，但可能会导致负载不均衡。较小的chunksize可以提高负载均衡，但可能会增加划分和通信的开销。因此，在选择chunksize时需要综合考虑这些因素。

```



```c++
//exportgold 
std::vector<int> tris;         // zone index list for 3-sided zones
    std::vector<int> quads;        // same, for 4-sided zones
    std::vector<int> others;       // same, for n-sided zones, n > 4
    std::vector<int> mapzs;        // map: zone -> first side

    // parallel info, meaningful on PE 0 only:
    std::vector<int> pentris;      // number of tris on each PE
    std::vector<int> penquads;     // same, for quads
    std::vector<int> penothers;    // same, for others
    int gntris, gnquads, gnothers; // total number across all PEs
                                   //     of tris/quads/others

//parallel
extern int numpe;           // number of MPI PEs in use numpe：表示正在使用的 MPI（Message Passing Interface）进程的数量。如果没有使用 MPI，则值为 1。
extern int mype;            // PE number for my rank 表示当前进程的编号（PE number）。如果没有使用 MPI，则值为 0。
```



## 3.MESH

#### 1..hh

```c++
#ifndef MESH_HH_
#define MESH_HH_

#include <string>
#include <vector>

#include "Vec2.hh"

// forward declarations
class InputFile;
class GenMesh;
class WriteXY;
class ExportGold;


class Mesh {
public:

    // children
    GenMesh* gmesh;
    WriteXY* wxy;
    ExportGold* egold;

    // parameters
    int chunksize;                 // max size for processing chunks 最大处理块的大小 "chunk" 是指用于处理网格数据的块或片段。它是一个最大处理大小的参数，可以控制在处理网格时每次处理的数据量。当处理较大的网格时，将整个网格一次性加载到内存中可能会导致内存不足或性能问题。为了解决这个问题，可以将网格划分为多个小块（或称为"chunks"），每次只处理一个块。这样可以有效地管理内存和提高处理效率。
    std::vector<double> subregion; // 
    //Bounding box"（边界框）是一个用于描述物体或区域的最小矩形框。它通过最小和最大的边界值来定义一个矩形范围，可以完全包围住该物体或区域。通过定义这个bounding box，可以限制对网格数据的处理范围，只处理在这个边界框内的部分数据。这在处理大型网格时可以提高效率，并且可以更精确地控制处理的区域。
    bool writexy;                  // flag:  write .xy file?
    bool writegold;                // flag:  write Ensight file?
    int nump, nume, numz, nums, numc;
                       // number of points, edges, zones,
                       // sides, corners, resp.
    int numsbad;       // number of bad sides (negative volume)
    int* mapsp1;       // maps: side -> points 1 and 2
    int* mapsp2;
    int* mapsz;        // map: side -> zone
    int* mapse;        // map: side -> edge
    int* mapss3;       // map: side -> previous side
    int* mapss4;       // map: side -> next side

    // point-to-corner inverse map is stored as a linked list...这段注释指的是代码中关于点到角的反向映射（point-to-corner inverse map）的存储方式。按照注释所说，这个映射被存储为一个链表（linked list）的形式。链表是一种常见的数据结构，由一系列称为节点（node）的元素组成，每个节点包含数据和指向下一个节点的指针。在这种映射中，每个点与一个角相关联，而点到角的映射关系是通过链表来表示的。
    int* mappcfirst;   // map:  point -> first corner
    int* mapccnext;    // map:  corner -> next corner

    // mpi comm variables
    //主PEs是在并行计算中扮演主要协调和管理任务的角色。它们负责分配任务、收集结果和协调通信等任务。主PEs通常具有特殊的权限和职责，用于协调整个系统的运行
    int nummstrpe;     // number of messages mype sends to master pes mype发送给主PE的消息数量
    //从属PEs是主PE的下属，负责执行分配给它们的任务。它们通常按照主PE的指示来执行任务，处理计算和通信等工作。从属PEs可能会将处理的结果发送给主PE，以供其进一步处理或收集。
    int numslvpe;      // number of messages mype receives from slave pes mype从从属PE接收的消息数量
    
    int numprx;        // number of proxies on mype mype上的代理数量
    //代理是指在并行计算中，一个进程或处理单元代表另一个进程或处理单元执行任务的情况。代理可以用于将任务分配给其他进程执行，并收集结果。在代码中，numprx 变量表示在当前进程上的代理数量。这意味着当前进程（mype）扮演着其他进程的代理角色，负责执行它们的任务。通过这些代理，当前进程可以与其他进程进行通信，并协调他们的计算任务。
    int numslv;        // number of slaves on mype mype上的从属数量
    //在当前进程上的从属数量。这意味着当前进程（mype）有多少个从属进程或从属PE被分配给它，用于执行任务。通过 numslv 变量，我们可以了解当前进程负责协调和管理多少个从属进程的执行。这些从属进程通常会将计算结果发送回主进程以进行进一步处理或收集。
    //PE是指在并行计算系统中的一个处理单元或进程。它可以是主PE或从属PE。"Global"一词可能是指在整个系统范围内或全局范围内的PE。
    int* mapslvpepe;   // map: slave pe -> (global) pe
    //在并行计算环境中，通常会有多个从属PE和全局PE，它们分别扮演不同的角色和职责。从属PE执行具体的计算任务，而全局PE负责协调和管理整个系统通过建立从属PE到全局PE的映射关系，可以实现从属PE与全局PE之间的通信和协调。这样，每个从属PE就知道它所对应的全局PE，可以与其进行通信和共享信息。
    int* mapslvpeprx1; // map: slave pe -> first proxy in proxy buffer
    //建立从属PE到代理缓冲区中的第一个代理的映射关系。在并行计算中，代理缓冲区是一个存储代理对象的数据结构，用于存放从属PE所需的代理。代理对象可以包含任务、数据或其他需要在从属PE上执行的内容。
	//通过建立从属PE到代理缓冲区中第一个代理的映射关系，可以确定每个从属PE所需执行的具体任务或操作。从属PE可以通过映射关系找到自己在代理缓冲区中的代理，并进行执行。
    int* mapprxp;      // map: proxy -> corresponding (master) point
    //代理可以是指在主点上执行的任务的表示或占位符。主点是指在并行计算中执行任务的特定位置或处理单元。通过建立代理到相应的主点之间的映射关系，可以实现代理与主点之间的任务分配和执行。代理可以通过映射关系找到与之对应的主点，并将任务分配给主点执行。
    int* slvpenumprx;  // number of proxies for each slave pe
    int* mapmstrpepe;  // map: master pe -> (global) pe
    int* mstrpenumslv; // number of slaves for each master pe
    int* mapmstrpeslv1;// map: master pe -> first slave in slave buffer
    int* mapslvp;      // map: slave -> corresponding (slave) point

    int* znump;        // number of points in zone

    double2* px;       // point coordinates点的坐标。
    double2* ex;       // edge center coordinates 边的中心的坐标
    double2* zx;       // zone center coordinates
    double2* pxp;      // point coords, middle of cycle当在循环过程中处理点的坐标时，可能需要在每次循环迭代中更新 pxp 数组，以存储每个点在循环的中间阶段的坐标 将中间结果存储在 pxp 数组
    double2* exp;      // edge ctr coords, middle of cycle 边的中心的坐标
    double2* zxp;      // zone ctr coords, middle of cycle
    double2* px0;      // point coords, start of cycle

    double* sarea;     // side area边的面积
    double* svol;      // side volume边的体积
    double* zarea;     // zone area
    double* zvol;      // zone volume
    double* sareap;    // side area, middle of cycle
    double* svolp;     // side volume, middle of cycle
    double* zareap;    // zone area, middle of cycle
    double* zvolp;     // zone volume, middle of cycle
    double* zvol0;     // zone volume, start of cycle

    double2* ssurfp;   // side surface vector
    //表面向量通常用于描述边界或曲面的方向和法向量。在数值计算中，边的表面向量可能用于计算边界条件、梯度或通量等
    double* elen;      // edge length
    double* smf;       // side mass fraction
    //边的质量分数 边上的质量分布比例
    double* zdl;       // zone characteristic length

    int numsch;                    // number of side chunks
    std::vector<int> schsfirst;    // start/stop index for side chunks每个边界分块的起始位置的索引
    std::vector<int> schslast; 		//每个边界分块的结束位置的索引
    std::vector<int> schzfirst;    // start/stop index for zone chunks每个边界分块所涉及的区域的起始位置
    std::vector<int> schzlast;		//每个边界分块所涉及的区域的结束位置
    int numpch;                    // number of point chunks
    std::vector<int> pchpfirst;    // start/stop index for point chunks
    std::vector<int> pchplast;
    int numzch;                    // number of zone chunks
    std::vector<int> zchzfirst;    // start/stop index for zone chunks
    std::vector<int> zchzlast;

    Mesh(const InputFile* inp);
    ~Mesh();

  
```



```c++
  void init();

    // populate mapping arrays
    void initSides(
            const std::vector<int>& cellstart,
            const std::vector<int>& cellsize,
            const std::vector<int>& cellnodes);
    void initEdges();

    // populate chunk information
    void initChunks();

    // populate inverse map
    void initInvMap();

    void initParallel(
            const std::vector<int>& slavemstrpes,
            const std::vector<int>& slavemstrcounts,
            const std::vector<int>& slavepoints,
            const std::vector<int>& masterslvpes,
            const std::vector<int>& masterslvcounts,
            const std::vector<int>& masterpoints);

    // write mesh statistics
    void writeStats();

    // write mesh
    void write(
            const std::string& probname,
            const int cycle,
            const double time,
            const double* zr,
            const double* ze,
            const double* zp);

    // find plane with constant x, y value
    std::vector<int> getXPlane(const double c);
    std::vector<int> getYPlane(const double c);

    // compute chunks for a given plane
    void getPlaneChunks(
            const int numb,
            const int* mapbp,
            std::vector<int>& pchbfirst,
            std::vector<int>& pchblast);

    // compute edge, zone centers
    void calcCtrs(
            const double2* px,
            double2* ex,
            double2* zx,
            const int sfirst,
            const int slast);

    // compute side, corner, zone volumes
    void calcVols(
            const double2* px,
            const double2* zx,
            double* sarea,
            double* svol,
            double* zarea,
            double* zvol,
            const int sfirst,
            const int slast);

    // check to see if previous volume computation had any
    // sides with negative volumes
    void checkBadSides();

    // compute side mass fractions
    void calcSideFracs(
            const double* sarea,
            const double* zarea,
            double* smf,
            const int sfirst,
            const int slast);

    // compute surface vectors for median mesh
    void calcSurfVecs(
            const double2* zx,
            const double2* ex,
            double2* ssurf,
            const int sfirst,
            const int slast);

    // compute edge lengths
    void calcEdgeLen(
            const double2* px,
            double* elen,
            const int sfirst,
            const int slast);

    // compute characteristic lengths
    void calcCharLen(
            const double* sarea,
            double* zdl,
            const int sfirst,
            const int slast);

    // sum corner variables to points (double or double2)
    template <typename T>
    void sumToPoints(
            const T* cvar,
            T* pvar);

    // helper routines for sumToPoints
    template <typename T>
    void sumOnProc(
            const T* cvar,
            T* pvar);
    template <typename T>
    void sumAcrossProcs(T* pvar);
    template <typename T>
    void parallelGather(
            const T* pvar,
            T* prxvar);
    template <typename T>
    void parallelSum(
            T* pvar,
            T* prxvar);
    template <typename T>
    void parallelScatter(
            T* pvar,
            const T* prxvar);

}; // class Mesh



#endif /* MESH_HH_ */
```

#### 2..cc

##### 1.构造函数 析构函数(inputfile)(GenMesh  WriteXY ExportGold)

```c++
Mesh::Mesh(const InputFile* inp) :
    gmesh(NULL), egold(NULL), wxy(NULL) {

    using Parallel::mype;

    chunksize = inp->getInt("chunksize", 0);
    if (chunksize < 0) {
        if (mype == 0)
            cerr << "Error: bad chunksize " << chunksize << endl;
        exit(1);
    }

    subregion = inp->getDoubleList("subregion", vector<double>());//如果 subregion 大小为0，表示没有指定子区域，网格将被认为是整个区域。如果 subregion 大小为4，表示指定了一个具体的子区域，其中4个值分别表示子区域的最小 x 坐标、最小 y 坐标、最大 x 坐标和最大 y 坐标。
    if (subregion.size() != 0 && subregion.size() != 4) {
        if (mype == 0)
            cerr << "Error:  subregion must have 4 entries" << endl;
        exit(1);
    }

    writexy = inp->getInt("writexy", 0);
    writegold = inp->getInt("writegold", 0);

    gmesh = new GenMesh(inp);
    wxy = new WriteXY(this);
    egold = new ExportGold(this);
        //this 关键字是用来指代当前对象的指针mesh

    init();
}
Mesh::~Mesh() {
    delete gmesh;
    delete wxy;
    delete egold;
}
```

##### 2.init（）

```c++
void Mesh::init() {

    // generate mesh
    //声明了一些存储数据的向量，并调用 gmesh->generate() 方法生成了一些网格相关的数据，如节点位置、单元起始位置、单元大小和单元节点等
    vector<double2> nodepos;
    vector<int> cellstart, cellsize, cellnodes;
    //cellstart 存储了单元在 cellnodes 中的起始位置，cellsize 存储了每个单元的大小（节点数量），而 cellnodes 存储了单元的节点索引。
    vector<int> slavemstrpes, slavemstrcounts, slavepoints;
    vector<int> masterslvpes, masterslvcounts, masterpoints;
    gmesh->generate(nodepos, cellstart, cellsize, cellnodes,
            slavemstrpes, slavemstrcounts, slavepoints,
            masterslvpes, masterslvcounts, masterpoints);

    nump = nodepos.size();
    //nump 表示节点的数量。它通过调用 nodepos.size() 来获取 nodepos 向量中存储的节点位置的数量。节点是组成网格的基本元素之一，在这个上下文中，它可能代表网格中的顶点或节点。
    numz = cellstart.size();
    //numz 表示单元的数量。它通过调用 cellstart.size() 来获取 cellstart 向量中存储的单元起始位置的数量。单元是组成网格的另一个基本元素，通常是由一组节点或顶点组成的几何实体，例如三角形单元或四边形单元。
    nums = cellnodes.size();
    //表示所有单元的节点总数
    //nums 表示边界（side）的数量。它通过调用 cellnodes.size() 来获取 cellnodes 向量中存储的单元节点的数量。在这种上下文中，边界（side）是定义单元的边界的一组节点或顶点。边界可以通过连接单元的节点来描述单元的边界形状。
    numc = nums;

    // copy cell sizes to mesh
    znump = Memory::alloc<int>(numz);
    copy(cellsize.begin(), cellsize.end(), znump);

    // populate maps:
    // use the cell* arrays to populate the side maps这些代码通过调用 initSides() 方法，使用单元起始位置、单元大小和单元节点数据来初始化边界（side）的映射关系。然后，释放了对应的单元数据的内存空间
    initSides(cellstart, cellsize, cellnodes);
    // release memory from cell* arrays
    cellstart.resize(0);
    cellsize.resize(0);
    cellnodes.resize(0);
    
    // now populate edge maps using side maps根据已初始化的边界（side）映射关系，初始化边（edge）的映射关系
    initEdges();

    // populate chunk information初始化块（chunk）的相关信息
    initChunks();

    // create inverse map for corner-to-point gathers创建角点到点（corner-to-point）聚集的反向映射
    initInvMap();

    // calculate parallel data structures
    //计算并初始化并行化的数据结构。然后，释放了与并行化相关的数组的内存空间。
    initParallel(slavemstrpes, slavemstrcounts, slavepoints,
            masterslvpes, masterslvcounts, masterpoints);
    // release memory from parallel-related arrays
    slavemstrpes.resize(0);
    slavemstrcounts.resize(0);
    slavepoints.resize(0);
    masterslvpes.resize(0);
    masterslvcounts.resize(0);
    masterpoints.resize(0);

    // write mesh statistics
    //写入网格的统计信息
    writeStats();

    // allocate remaining arrays
    //为各种数组变量分配了内存空间
    px = Memory::alloc<double2>(nump);
    ex = Memory::alloc<double2>(nume);
    zx = Memory::alloc<double2>(numz);
    px0 = Memory::alloc<double2>(nump);
    pxp = Memory::alloc<double2>(nump);
    exp = Memory::alloc<double2>(nume);
    zxp = Memory::alloc<double2>(numz);
    sarea = Memory::alloc<double>(nums);
    svol = Memory::alloc<double>(nums);
    zarea = Memory::alloc<double>(numz);
    zvol = Memory::alloc<double>(numz);
    sareap = Memory::alloc<double>(nums);
    svolp = Memory::alloc<double>(nums);
    zareap = Memory::alloc<double>(numz);
    zvolp = Memory::alloc<double>(numz);
    zvol0 = Memory::alloc<double>(numz);
    ssurfp = Memory::alloc<double2>(nums);
    elen = Memory::alloc<double>(nume);
    zdl = Memory::alloc<double>(numz);
    smf = Memory::alloc<double>(nums);

    // do a few initial calculations
    //用 OpenMP 的并行化指令，在多个线程上并行地将节点位置复制到 px 数组中
    #pragma omp parallel for schedule(static)
    for (int pch = 0; pch < numpch; ++pch) {
        int pfirst = pchpfirst[pch];
        int plast = pchplast[pch];
        // copy nodepos into px, distributed across threads
        for (int p = pfirst; p < plast; ++p)
            px[p] = nodepos[p];

    }
	
    //使用 OpenMP 的并行化指令，在多个线程上并行地进行一些计算操作，包括计算中心位置、体积、面积以及一些侧面相关的计算。最后，调用 checkBadSides() 方法检查是否存在错误的侧面。
    numsbad = 0;
    #pragma omp parallel for schedule(static)
    for (int sch = 0; sch < numsch; ++sch) {
        int sfirst = schsfirst[sch];
        int slast = schslast[sch];
        calcCtrs(px, ex, zx, sfirst, slast);
        calcVols(px, zx, sarea, svol, zarea, zvol, sfirst, slast);
        calcSideFracs(sarea, zarea, smf, sfirst, slast);
    }
    checkBadSides();

}
```

##### 3.initsides（）

```c++
void Mesh::initSides(
        const vector<int>& cellstart,
        const vector<int>& cellsize,
        const vector<int>& cellnodes) {//单元起始位置、单元大小和单元节点数
    
    mapsp1 = Memory::alloc<int>(nums);//maps: side -> points 1
    mapsp2 = Memory::alloc<int>(nums);//maps: side -> points 2
    mapsz  = Memory::alloc<int>(nums);//map: side -> zone
    mapss3 = Memory::alloc<int>(nums);//map: side -> previous side
    mapss4 = Memory::alloc<int>(nums);//map: side -> next side
		
    //numz = cellstart.size();
    //numz 表示单元的数量
    for (int z = 0; z < numz; ++z) {
        int sbase = cellstart[z];
        int size = cellsize[z];
        //int sbase = cellstart[z];获取当前层起始位置， 
        //int size = cellsize[z];获取当前层的单元数量
        for (int n = 0; n < size; ++n) {
            int s = sbase + n;
            //计算当前side的索引
            int snext = sbase + (n + 1 == size ? 0 : n + 1);
            //计算当前side的下一条side的索引。如果当前side是最后一条边，下一条边的索引为当前层的起始位置（0），否则为当前边的下一条边
            int slast = sbase + (n == 0 ? size : n) - 1;
            //计算当前单元的前一条边的索引。如果当前边是第一条边，前一条边的索引为当前层的起始位置加上当前层的单元数量减一，否则为当前边的前一条边。
            mapsz[s] = z;//当前side所在的zero号
            mapsp1[s] = cellnodes[s];//当前side的第1个顶点的节点号
            mapsp2[s] = cellnodes[snext];//(即下一条side的初始点)当前side的第2个顶点的节点号
            mapss3[s] = slast;//当前边的前一条边的节点号
            mapss4[s] = snext;//当前边的下一条边的节点号
        } // for n
    } // for z

}
```

##### 4.initedges（）

```c++
void Mesh::initEdges() {//根据已初始化的边界（side）映射关系，初始化边（edge）的映射关系

    vector<vector<int> > edgepp(nump), edgepe(nump);

    mapse = Memory::alloc<int>(nums);

    int e = 0;
    //nums number of sides
    for (int s = 0; s < nums; ++s) {
        int p1 = min(mapsp1[s], mapsp2[s]);
        int p2 = max(mapsp1[s], mapsp2[s]);

        vector<int>& vpp = edgepp[p1];//vpp p1->其他所有p点
        vector<int>& vpe = edgepe[p1];//vpe p1->与p1相关的所有边
        
        int i = find(vpp.begin(), vpp.end(), p2) - vpp.begin();
        //在vpp中查找第二个顶点p2，并获取其在向量中的索引i
        
        if (i == vpp.size()) {
            // (p, p2) isn't in the edge list - add it
            //如果i等于vpp.size()，表示(p1, p2)不在边界信息向量中，需要添加新的edge
            vpp.push_back(p2);//将p2添加到vpp中
            vpe.push_back(e);//当前边的索引e添加到vpe
            ++e//增加一条edge
        }
        mapse[s] = vpe[i];//side->edge
    }  // for s

    nume = e;

}

```

##### 5.initchunks() chunksize = inp->getInt("chunksize", 0);

```c++
void Mesh::initChunks() {

    if (chunksize == 0) chunksize = max(nump, nums);

    // compute side chunks边界分块
    // use 'chunksize' for maximum chunksize; decrease as needed to ensure that no zone has its sides split across chunk boundaries
    //确保没有zera的边，跨过chunk的边界
    //这段代码的注释指出，在计算边界分块的过程中，使用chunksize作为最大分块大小。
    //然而，如果使用该大小会导致某个zrea的边界被分割在不同的chunk中，那么需要逐渐减小分块大小，直到不再出现该问题。
	//当检测到chunk边界跨越不同的zera时，会将分块的结束位置向前移动，直到不再跨越不同的区域。
    //这样可以确保每个分块的边界都位于同一个区域内
    int s1, s2 = 0;
    //通过s2对每个side遍历
    while (s2 < nums) {
        s1 = s2;
        s2 = min(s2 + chunksize, nums);
        //计算边界分块的结束位置s2的。
        //它将s2的值设置为当前s2加上chunksize和nums中的较小值。
		//该代码用于确定边界分块的起始位置s1和结束位置s2，并保证每个分块的长度不超过chunksize，同时确保不超过边界的索引范围。
        while (s2 < nums && mapsz[s2] == mapsz[s2-1])
        --s2;
        //当检测到s2所指示的side与它前一个side的area相同时，s2会递减1。
        //这是为了确保边界分块的结束位置不会落在相同area的连续side上，而是在不同area的side之间。
        schsfirst.push_back(s1);
        schslast.push_back(s2);
        schzfirst.push_back(mapsz[s1]);//每个边界分块所涉及的 区域的起始位置
        schzlast.push_back(mapsz[s2-1] + 1);//每个边界分块所涉及的 区域的结束位置
        //通过mapsz[s2-1]可以获取边界分块结束位置s2前一个位置对应的区域的结束位置。加1是为了获取下一个区域的起始位置。
    }
    numsch = schsfirst.size();

    // compute point chunks
    int p1, p2 = 0;
    while (p2 < nump) {
        p1 = p2;
        p2 = min(p2 + chunksize, nump);
        pchpfirst.push_back(p1);
        pchplast.push_back(p2);//将p1和p2分别添加到pchpfirst和pchplast向量中，以记录每个点分块的起始位置和结束位置。
    }
    numpch = pchpfirst.size();

    // compute zone chunks
    int z1, z2 = 0;
    while (z2 < numz) {
        z1 = z2;
        z2 = min(z2 + chunksize, numz);
        zchzfirst.push_back(z1);
        zchzlast.push_back(z2);
    }
    numzch = zchzfirst.size();

}
```

##### 6.initinvmap point->corner(链表)

```c++
void Mesh::initInvMap() {
     // 点到角的反向映射（point-to-corner inverse map）
    //按照注释所说，这个映射被存储为一个链表（linked list）的形式。链表由一系列称为节点（node）的元素组成，每个节点包含数据和指向下一个节点的指针。在这种映射中，每个点与一个角相关联，而点到角的映射关系是通过链表来表示的。
    int* mappcfirst;   // map:  point -> first corner
    int* mapccnext;    // map:  corner -> next corner
    mappcfirst = Memory::alloc<int>(nump);
    mapccnext = Memory::alloc<int>(nums);

    vector<pair<int, int> > pcpair(nums);
    
    //对每个corner
    //nums 表示边界（side）的数量。它通过调用 cellnodes.size() 来获取 cellnodes 向量中存储的单元节点的数量。在这种上下文中，边界（side）是定义单元的边界的一组节点或顶点。边界可以通过连接单元的节点来描述单元的边界形状。
    //numc = nums;
    for (int c = 0; c < numc; ++c)
        pcpair[c] = make_pair(mapsp1[c], c);//(p1,corner)
    sort(pcpair.begin(), pcpair.end());
    //其中元素为(mapsp1[c], c)，即每个corner的起始点和corner的索引
    
    
    //写点到角的反向映射 链表形式
    for (int i = 0; i < numc; ++i) {
        int p = pcpair[i].first;
        int c = pcpair[i].second;

        if (i == 0) mappcfirst[p] = c;
        
        //获取上一个corner的起始点pm。如果当前处理的corner的起始点p与上一个corne的起始点pm不相等，则将mappcfirst[p]设为该corner的索引c。
        else {
            int pm = pcpair[i-1].first;
            if (p != pm) mappcfirst[p] = c;
        }
        mapccnext[c] = -1;
        //如果当前处理的单元不是最后一个单元（即i+1 != numc），则获取下一个单元的起始点pp和单元索引cp。如果当前单元的起始点p与下一个单元的起始点pp相等，则将当前单元的下一个相邻单元索引mapccnext[c]设为下一个单元的索引cp。

//通过这样的处理，逆映射mappcfirst中的每个点索引对应着对应的第一个单元索引，而mapccnext中的每个单元索引对应着下一个相邻单元的索引。
        if (i+1 != numc) {
            int pp = pcpair[i+1].first;
            int cp = pcpair[i+1].second;
            if (p == pp) mapccnext[c] = cp;
        }
    }
}
```

##### 7.initparallel

```c++
void Mesh::initParallel(
        const vector<int>& slavemstrpes,
        const vector<int>& slavemstrcounts,
        const vector<int>& slavepoints,
        const vector<int>& masterslvpes,
        const vector<int>& masterslvcounts,
        const vector<int>& masterpoints) {
    if (Parallel::numpe == 1) return;

    nummstrpe = slavemstrpes.size();//从属点主点索引数组
    //number of messages mype sends to master pes mype
    mapmstrpepe = Memory::alloc<int>(nummstrpe);//map: slave pe -> (global) pe
    //在并行计算环境中，通常会有多个从属PE和全局PE，它们分别扮演不同的角色和职责。从属PE执行具体的计算任务，而全局PE负责协调和管理整个系统通过建立从属PE到全局PE的映射关系，可以实现从属PE与全局PE之间的通信和协调。这样，每个从属PE就知道它所对应的全局PE，可以与其进行通信和共享信息。
    copy(slavemstrpes.begin(), slavemstrpes.end(), mapmstrpepe);
    mstrpenumslv = Memory::alloc<int>(nummstrpe);
    
    copy(slavemstrcounts.begin(), slavemstrcounts.end(), mstrpenumslv);
    mapmstrpeslv1 = Memory::alloc<int>(nummstrpe);
    int count = 0;
    for (int mstrpe = 0; mstrpe < nummstrpe; ++mstrpe) {
        mapmstrpeslv1[mstrpe] = count;
        count += mstrpenumslv[mstrpe];
    }
    numslv = slavepoints.size();
    mapslvp = Memory::alloc<int>(numslv);
    copy(slavepoints.begin(), slavepoints.end(), mapslvp);

    numslvpe = masterslvpes.size();
    mapslvpepe = Memory::alloc<int>(numslvpe);
    copy(masterslvpes.begin(), masterslvpes.end(), mapslvpepe);
    slvpenumprx = Memory::alloc<int>(numslvpe);
    copy(masterslvcounts.begin(), masterslvcounts.end(), slvpenumprx);
    mapslvpeprx1 = Memory::alloc<int>(numslvpe);
    count = 0;
    for (int slvpe = 0; slvpe < numslvpe; ++slvpe) {
        mapslvpeprx1[slvpe] = count;
        count += slvpenumprx[slvpe];
    }
    numprx = masterpoints.size();
    mapprxp = Memory::alloc<int>(numprx);
    copy(masterpoints.begin(), masterpoints.end(), mapprxp);

}


```

##### 8.writetats

```c
void Mesh::writeStats() {//通过求和操作将各个节点的统计数据进行全局汇总，并在编号为0的节点输出统计信息

    int64_t gnump = nump;
    // make sure that boundary points aren't double-counted only count them if they are masters
    if (Parallel::numpe > 1) gnump -= numslv;
    int64_t gnumz = numz;
    int64_t gnums = nums;
    int64_t gnume = nume;
    int gnumpch = numpch;
    int gnumzch = numzch;
    int gnumsch = numsch;
//使用Parallel::globalSum函数将全局变量gnump、gnumz、gnums、gnume、gnumpch、gnumzch和gnumsch进行全局求和，以确保所有节点都具有相同的统计数据。
    Parallel::globalSum(gnump);
    Parallel::globalSum(gnumz);
    Parallel::globalSum(gnums);
    Parallel::globalSum(gnume);
    Parallel::globalSum(gnumpch);
    Parallel::globalSum(gnumzch);
    Parallel::globalSum(gnumsch);

    if (Parallel::mype > 0) return;

    cout << "--- Mesh Information ---" << endl;
    cout << "Points:  " << gnump << endl;
    cout << "Zones:  "  << gnumz << endl;
    cout << "Sides:  "  << gnums << endl;
    cout << "Edges:  "  << gnume << endl;
    cout << "Side chunks:  " << gnumsch << endl;
    cout << "Point chunks:  " << gnumpch << endl;
    cout << "Zone chunks:  " << gnumzch << endl;
    cout << "Chunk size:  " << chunksize << endl;
    cout << "------------------------" << endl;

}

```

##### 9.write

```c++
void Mesh::write(//该函数根据写入标志writexy和writegold的值，选择性地将网格数据写入.xy文件和gold文件
        const string& probname,
        const int cycle,
        const double time,
        const double* zr,
        const double* ze,
        const double* zp) {

    if (writexy) {
        if (Parallel::mype == 0)
            cout << "Writing .xy file..." << endl;
        wxy->write(probname, zr, ze, zp);
    }
    if (writegold) {
        if (Parallel::mype == 0) 
            cout << "Writing gold file..." << endl;
        egold->write(probname, cycle, time, zr, ze, zp);
    }

}
```

##### 10.getxplane

```c++
//该函数用于获取位于指定平面上的点的索引。
//该函数通过比较点的x坐标与指定平面位置之间的差的绝对值，找到位于指定平面上的点，并将其索引存储在mapbp向量中返回
vector<int> Mesh::getXPlane(const double c) {

    vector<int> mapbp;
    const double eps = 1.e-12;

    for (int p = 0; p < nump; ++p) {
        if (fabs(px[p].x - c) < eps) {
            mapbp.push_back(p);
        }
    }
    return mapbp;

}
```

##### 11.getplanechunk

```c++
void Mesh::getPlaneChunks(//找到每个点块边界点的索引范围
        const int numb,
        const int* mapbp,
        vector<int>& pchbfirst,
        vector<int>& pchblast) {

    pchbfirst.resize(0);
    pchblast.resize(0);

    // compute boundary point chunks
    // (boundary points contained in each point chunk)
    int bf, bl = 0;
    for (int pch = 0; pch < numpch; ++pch) {
         int pl = pchplast[pch];
         bf = bl;
         bl = lower_bound(&mapbp[bf], &mapbp[numb], pl) - &mapbp[0];
         pchbfirst.push_back(bf);
         pchblast.push_back(bl);
    }
    

}
```

##### 12.calcvols

```c++
void Mesh::calcVols(//compute side, corner, zone volumes
        const double2* px,
        const double2* zx,// zone center coordinates
        double* sarea,
        double* svol,
        double* zarea,
        double* zvol,
        const int sfirst,
        const int slast) {

    int zfirst = mapsz[sfirst];
    int zlast = (slast < nums ? mapsz[slast] : numz);
    fill(&zvol[zfirst], &zvol[zlast], 0.);
    fill(&zarea[zfirst], &zarea[zlast], 0.);

    const double third = 1. / 3.;
    int count = 0;
    for (int s = sfirst; s < slast; ++s) {
        int p1 = mapsp1[s];
        int p2 = mapsp2[s];
        int z = mapsz[s];

        // compute side volumes, sum to zone
        double sa = 0.5 * cross(px[p2] - px[p1], zx[z] - px[p1]);
        double sv = third * sa * (px[p1].x + px[p2].x + zx[z].x);
        sarea[s] = sa;
        svol[s] = sv;
        zarea[z] += sa;
        zvol[z] += sv;

        // check for negative side volumes
        if (sv <= 0.) count += 1;

    } // for s

    if (count > 0) {
        #pragma omp atomic
        numsbad += count;
    }

}
```



## .其他类

### 1.inputfile

### 2.genmesh(接收inputfile)

#### 1..hh

```c++
class GenMesh {
public:

    std::string meshtype;       // generated mesh type 生成的网格类型
    int gnzx, gnzy;             // global number of zones, in x and y directions表示在 x 和 y 方向上的全局（总体）网格区域的区域数
    double lenx, leny;          // length of mesh sides, in x and y directions 网格的边长，分别对应 x 和 y 方向
    int numpex, numpey;         // number of PEs to use, in x and ydirections 表示在 x 和 y 方向上使用的处理器（PE）的数量
    int mypex, mypey;           // my PE index, in x and y directions 表示当前 PE 在 x 和 y 方向上的索引 进程的x轴位置
    int nzx, nzy;               // (local) number of zones, in x and y directions在 x 和 y 方向上的本地网格区域（每个 PE 上的区域）的区域数
    int zxoffset, zyoffset;     // offsets of local zone array into global, in x and y directions 表示本地网格区域（每个 PE 上的区域）在全局网格区域中的偏移量，分别对应 x 和 y 方向
    GenMesh(const InputFile* inp);
    ~GenMesh();

    void generate(
            std::vector<double2>& pointpos,
            std::vector<int>& zonestart,
            std::vector<int>& zonesize,
            std::vector<int>& zonepoints,
            std::vector<int>& slavemstrpes,
            std::vector<int>& slavemstrcounts,
            std::vector<int>& slavepoints,
            std::vector<int>& masterslvpes,
            std::vector<int>& masterslvcounts,
            std::vector<int>& masterpoints);

    void generateRect(
            std::vector<double2>& pointpos,
            std::vector<int>& zonestart,
            std::vector<int>& zonesize,
            std::vector<int>& zonepoints,
            std::vector<int>& slavemstrpes,
            std::vector<int>& slavemstrcounts,
            std::vector<int>& slavepoints,
            std::vector<int>& masterslvpes,
            std::vector<int>& masterslvcounts,
            std::vector<int>& masterpoints);

    void generatePie(
            std::vector<double2>& pointpos,
            std::vector<int>& zonestart,
            std::vector<int>& zonesize,
            std::vector<int>& zonepoints,
            std::vector<int>& slavemstrpes,
            std::vector<int>& slavemstrcounts,
            std::vector<int>& slavepoints,
            std::vector<int>& masterslvpes,
            std::vector<int>& masterslvcounts,
            std::vector<int>& masterpoints);

    void generateHex(
            std::vector<double2>& pointpos,
            std::vector<int>& zonestart,
            std::vector<int>& zonesize,
            std::vector<int>& zonepoints,
            std::vector<int>& slavemstrpes,
            std::vector<int>& slavemstrcounts,
            std::vector<int>& slavepoints,
            std::vector<int>& masterslvpes,
            std::vector<int>& masterslvcounts,
            std::vector<int>& masterpoints);

    void calcNumPE();

}; // class GenMesh


#endif /* GENMESH_HH_ */
```

#### 2..cc

##### 1.构造函数（mesh类型 pie rect hex）

```c++
GenMesh::GenMesh(const InputFile* inp) {

    using Parallel::mype;

    meshtype = inp->getString("meshtype", "");
    //这个参数用于指定网格类型，可以是"pie"、"rect"或"hex"之一
    if (meshtype.empty()) {
        if (mype == 0)
            cerr << "Error:  must specify meshtype" << endl;
        exit(1);
    }
    if (meshtype != "pie" &&
            meshtype != "rect" &&
            meshtype != "hex") {
        if (mype == 0)
            cerr << "Error:  invalid meshtype " << meshtype << endl;
        exit(1);
    }
    vector<double> params =
            inp->getDoubleList("meshparams", vector<double>());
    if (params.empty()) {
        if (mype == 0)
            cerr << "Error:  must specify meshparams" << endl;
        exit(1);
    }
    if (params.size() > 4) {
        if (mype == 0)
            cerr << "Error:  meshparams must have <= 4 values" << endl;
        exit(1);
    }
	//将params列表中的第一个值赋给变量gnzx，将第二个值赋给变量gnzy
    //表示在 x 和 y 方向上的全局（总体）网格区域的区域数
    gnzx = params[0];
    gnzy = (params.size() >= 2 ? params[1] : gnzx);
    if (meshtype != "pie")
        lenx = (params.size() >= 3 ? params[2] : 1.0);
    //网格的边长，分别对应 x 和 y 方向
    //如果params列表长度大于等于3且meshtype不为"pie"，则将第三个值赋给变量lenx
    else
        // convention:  x = theta, y = r
        lenx = (params.size() >= 3 ? params[2] : 90.0)
                * M_PI / 180.0;
    //为pie类型 将第三个值乘以π/180后赋给lenx
    leny = (params.size() >= 4 ? params[3] : 1.0);

    if (gnzx <= 0 || gnzy <= 0 || lenx <= 0. || leny <= 0. ) {
        if (mype == 0)
            cerr << "Error:  meshparams values must be positive" << endl;
        exit(1);
    }
    if (meshtype == "pie" && lenx >= 2. * M_PI) {
        if (mype == 0)
            cerr << "Error:  meshparams theta must be < 360" << endl;
        exit(1);
    }

}
```

##### 2.generate

```c++
void GenMesh::generate(
    //根据输入的参数和网格类型来生成一个网格，并将生成的网格的相关信息存储在对应的向量中
        std::vector<double2>& pointpos,
        std::vector<int>& zonestart,
        std::vector<int>& zonesize,
        std::vector<int>& zonepoints,
        std::vector<int>& slavemstrpes,
        std::vector<int>& slavemstrcounts,
        std::vector<int>& slavepoints,
        std::vector<int>& masterslvpes,
        std::vector<int>& masterslvcounts,
        std::vector<int>& masterpoints){

    // do calculations common to all mesh types
    calcNumPE();
    zxoffset = mypex * gnzx / numpex;
    const int zxstop = (mypex + 1) * gnzx / numpex;
    nzx = zxstop - zxoffset;
    zyoffset = mypey * gnzy / numpey;
    const int zystop = (mypey + 1) * gnzy / numpey;
    nzy = zystop - zyoffset;

    // mesh type-specific calculations
    if (meshtype == "pie")
        generatePie(pointpos, zonestart, zonesize, zonepoints,
                slavemstrpes, slavemstrcounts, slavepoints,
                masterslvpes, masterslvcounts, masterpoints);
    else if (meshtype == "rect")
        generateRect(pointpos, zonestart, zonesize, zonepoints,
                slavemstrpes, slavemstrcounts, slavepoints,
                masterslvpes, masterslvcounts, masterpoints);
    else if (meshtype == "hex")
        generateHex(pointpos, zonestart, zonesize, zonepoints,
                slavemstrpes, slavemstrcounts, slavepoints,
                masterslvpes, masterslvcounts, masterpoints);

}
```

##### 3.generaterect

```c++
void GenMesh::generateRect(
    //生成矩形网格，并将生成的点坐标、区域邻接列表、从属点和主点等信息存储在对应的向量中。
        std::vector<double2>& pointpos,
        std::vector<int>& zonestart,
        std::vector<int>& zonesize,
        std::vector<int>& zonepoints,
        std::vector<int>& slavemstrpes,
        std::vector<int>& slavemstrcounts,
        std::vector<int>& slavepoints,
        std::vector<int>& masterslvpes,
        std::vector<int>& masterslvcounts,
        std::vector<int>& masterpoints) {

    using Parallel::numpe;
    using Parallel::mype;

    const int nz = nzx * nzy;
    //总区域数
    const int npx = nzx + 1;
    //每行的点数
    const int npy = nzy + 1;
    //每列的点数
    const int np = npx * npy;
	//总点数
    
    // generate point coordinates
    pointpos.reserve(np);
    //预先为pointpos向量分配了足够的内存空间，以容纳至少np个元素。这样做是为了减少向量在生成点坐标时的内存重新分配和拷贝操作的次数
    double dx = lenx / (double) gnzx;
    double dy = leny / (double) gnzy;
    for (int j = 0; j < npy; ++j) {
        double y = dy * (double) (j + zyoffset);
        for (int i = 0; i < npx; ++i) {
            double x = dx * (double) (i + zxoffset);
            pointpos.push_back(make_double2(x, y));
        //向std::vector<double2>类型的pointpos向量中添加一个新的元素的成员函数。它将指定的元素添加到向量的末尾
        }
    }
	//生成区域的邻接列表。它通过循环遍历每个区域的索引，计算每个区域的起始点、区域的大小和区域中的点列表，并将这些信息存储在zonestart、zonesize和zonepoints向量中
    // generate zone adjacency lists
    zonestart.reserve(nz);
    zonesize.reserve(nz);
    zonepoints.reserve(4 * nz);
    for (int j = 0; j < nzy; ++j) {
        for (int i = 0; i < nzx; ++i) {
            zonestart.push_back(zonepoints.size());
            //为了记录每个区域点列表的起始位置
            zonesize.push_back(4);
            //在这个特定的代码段中，每个区域的大小都是4，因为这是一个矩形网格，每个区域由四个点组成。
            int p0 = j * npx + i;
            //根据当前区域的索引和网格的大小，计算出该区域的起始点p0
            //zonepoints向量存储所有区域的点列表
            zonepoints.push_back(p0);
            zonepoints.push_back(p0 + 1);
            zonepoints.push_back(p0 + npx + 1);
            zonepoints.push_back(p0 + npx);
       }
    }

    if (numpe == 1) return;
	//
    // estimate sizes of slave/master arrays
    //如果进程的y轴位置(mypey)不等于0，则需要存储npx个从属点（因为每一行的点数为npx）。如果进程的x轴位置(mypex)不等于0，则需要存储npy个从属点（因为每一列的点数为npy）。
    //如果mypey不等于0，则说明当前进程不在第一行，需要存储从属点。类似地，如果mypex不等于0，则说明当前进程不在第一列，也需要存储从属点。
    //在并行计算中，通常涉及多个进程或线程同时执行任务。这些进程或线程被分配到一个二维网格或拓扑结构中，每个进程或线程在网格中占据一个位置。
    slavepoints.reserve((mypey != 0) * npx + (mypex != 0) * npy);
    //根据当前进程的位置，计算出需要存储的主点的数量。如果进程的y轴位置(mypey)不等于总y轴进程数减去1（即不在右边界进程），则需要存储npx个主点。如果进程的x轴位置(mypex)不等于总x轴进程数减去1（即不在上边界进程），则需要存储npy个主点。最后，还需要额外存储一个主点，即右上角的主点。通过将这三个数量相加，即可得出主点数组的预分配大小。
    masterpoints.reserve((mypey != numpey - 1) * npx + (mypex != numpex - 1) * npy + 1);

    // enumerate slave points
    
    // slave point with master at lower left 左下的从属点
    //确定不在（0，0）位置，它为从属点
    if (mypex != 0 && mypey != 0) {
        //计算主点的索引：通过减去numpex和1，可以得到主点的索引mstrpe。
        //numpex:总x轴进程数
        int mstrpe = mype - numpex - 1;
		//添加从属点信息：将主点索引mstrpe添加到从属点主点索引数组（slavemstrpes）中，将从属点计数设为1，并将从属点计数（1）添加到从属点计数数组（slavemstrcounts）中。
        slavepoints.push_back(0);
        slavemstrpes.push_back(mstrpe);
        slavemstrcounts.push_back(1);
    }
    
    //在代码中，从属点的判断条件如下：对于从属点位于左下方的条件：mypex != 0 && mypey != 0。如果当前进程的mypex和mypey均不等于0，则不满足从属点位于左下方的条件，因此可以将其视为主点。
	//对于从属点位于下方的条件：mypey != 0。如果当前进程的mypey不等于0，则不满足从属点位于下方的条件，因此可以将其视为主点
	//通过以上判断条件，如果一个进程不满足从属点的条件，那么它就可以被认为是主点。
    
    // slave points with master below 下方的从属点
    if (mypey != 0) {
        //主点的索引mstrpe
        int mstrpe = mype - numpex;
        int oldsize = slavepoints.size();
        int p = 0;
        //通过循环遍历每一列的网格单元，除了第一列，将从属点的主点索引（p）添加到从属点主点索引数组（slavemstrpes）中
        for (int i = 0; i < npx; ++i) {
            if (i == 0 && mypex != 0) { p++; continue; }
            slavepoints.push_back(p);
            p++;
        }
        slavemstrpes.push_back(mstrpe);
 		slavemstrcounts.push_back(slavepoints.size() - oldsize);
    }
    
    // slave points with master to left
    if (mypex != 0) {
        int mstrpe = mype - 1;
        int oldsize = slavepoints.size();
        int p = 0;
        for (int j = 0; j < npy; ++j) {
            if (j == 0 && mypey != 0) { p += npx; continue; }
            slavepoints.push_back(p);
            p += npx;
        }
        slavemstrpes.push_back(mstrpe);
        slavemstrcounts.push_back(slavepoints.size() - oldsize);
    }

    // enumerate master points
    // master points with slave to right
    if (mypex != numpex - 1) {
        int slvpe = mype + 1;
        int oldsize = masterpoints.size();
        int p = npx - 1;
        for (int j = 0; j < npy; ++j) {
            if (j == 0 && mypey != 0) { p += npx; continue; }
            masterpoints.push_back(p);
            p += npx;
        }
        masterslvpes.push_back(slvpe);
        masterslvcounts.push_back(masterpoints.size() - oldsize);
    }
    // master points with slave above
    if (mypey != numpey - 1) {
        int slvpe = mype + numpex;
        int oldsize = masterpoints.size();
        int p = (npy - 1) * npx;
        for (int i = 0; i < npx; ++i) {
            if (i == 0 && mypex != 0) { p++; continue; }
            masterpoints.push_back(p);
            p++;
        }
        masterslvpes.push_back(slvpe);
        masterslvcounts.push_back(masterpoints.size() - oldsize);
    }
    // master point with slave at upper right
    if (mypex != numpex - 1 && mypey != numpey - 1) {
        int slvpe = mype + numpex + 1;
        int p = npx * npy - 1;
        masterpoints.push_back(p);
        masterslvpes.push_back(slvpe);
        masterslvcounts.push_back(1);
    }

}

```



### 3.ExportGold

##### 1.hh

```c++
#ifndef EXPORTGOLD_HH_
#define EXPORTGOLD_HH_

#include <string>
#include <vector>
// forward declarations
class Mesh;
class ExportGold {
public:
    Mesh* mesh;
    std::vector<int> tris;         // zone index list for 3-sided zones 存储三角形区域的索引列表
    std::vector<int> quads;        // same, for 4-sided zones 存储四边形区域的索引列表
    std::vector<int> others;       // same, for n-sided zones, n > 4 
    std::vector<int> mapzs;        // map: zone -> first side 存储大于四边形的多边形区域的索引列表

    // parallel info, meaningful on PE 0 only:
    //只在PE 0上有意义。PE是指处理器元素（Processor Element），在并行计算中表示一个独立的处理单元。通常，在并行计算中，任务会被分配到多个处理器上并行执行，每个处理器执行其中的一部分任务。PE 0是指处理器元素编号为0的处理器，在某些并行计算环境中可能被指定为主处理器。因此，这句注释指出，下面的一些数据成员（pentris、penquads、penothers和gntris、gnquads、gnothers）只在PE 0上有意义，即它们的值只在PE 0上有有效的数据。对于其他处理器，这些值可能是无效或未定义的。
    std::vector<int> pentris;      // number of tris on each PE 表示每个处理器上的三角形数量。’存储了每个处理器上三角形数量的整数向量。每个元素表示一个处理器上的三角形数量。这种设计可以用于在并行计算环境中跟踪每个处理器上的特定数据量。通过这样的分布方式，可以将三角形的计算和处理工作分布在多个处理器上，以实现并行计算。
    std::vector<int> penquads;     // same, for quads
    std::vector<int> penothers;    // same, for others
    int gntris, gnquads, gnothers; // total number across all PEs
                                   //     of tris/quads/others

    ExportGold(Mesh* m);
    //构造函数ExportGold(Mesh* m)是类的特殊成员函数，用于初始化ExportGold对象。它在创建对象时自动调用，并且可以接受参数以提供必要的初始值。在这里，构造函数接受一个Mesh*类型的指针作为参数，用于初始化mesh数据成员。
    ~ExportGold();
    //用于销毁ExportGold对象。它在对象的生命周期结束时自动调用，并且不接受参数。在这里，析构函数的作用是在销毁ExportGold对象时执行一些必要的清理操作，如释放内存或关闭资源。

    void write(
            const std::string& basename,
            const int cycle,
            const double time,
            const double* zr,
            const double* ze,
            const double* zp);

    void writeCaseFile(
            const std::string& basename);

    void writeGeoFile(
            const std::string& basename,
            const int cycle,
            const double time);

    void writeVarFile(
            const std::string& basename,
            const std::string& varname,
            const double* var);

    void sortZones();
};



#endif /* EXPORTGOLD_HH_ */
```

 若您想确保某些数据只在处理器0中有效，可以使用条件语句来限制对数据的访问。在并行环境中，可以使用处理器的标识来确定当前代码是否在处理器0上执行。
以下是一个示例，展示如何使用条件语句来限制数据在处理器0上的有效性：



##### 2.cc

###### 1.引入

```c++
#include "ExportGold.hh"

#include <iostream>

#include <fstream>
//可以在程序中使用ifstream和ofstream类来进行文件的读取和写入操作。ifstream（输入文件流）类用于从文件中读取数据。ofstream（输出文件流）类用于将数据写入文件中。例如，您可以使用ifstream来打开一个文件并从中读取数据，或者使用ofstream来创建/打开一个文件并将数据写入其中。
#include <iomanip>
//iomanip是C++标准库中的一个头文件，提供了对输入输出流的格式化操作的支持。可以访问各种流操纵符（manipulators），用于控制流的格式、精度、对齐等。setprecision：设置输出流的浮点数精度。setw：设置输出流的字段宽度。setfill：设置输出流的填充字符。setiosflags：设置输出流的格式标志。resetiosflags：重置输出流的格式标志。通过使用这些流操纵符，您可以控制输出流的格式，使其满足特定的需求，如指定小数点后的位数、对齐输出、添加填充字符等。std::cout << std::setprecision(4) << pi << std::endl;
#include <cstdlib>
//包含了一些常用的函数，用于在程序中进行基本的数值转换、随机数生成、内存管理和程序终止等操作。内存管理函数：malloc：分配指定大小的内存块。calloc：分配并清零指定大小的内存块。realloc：重新分配已分配内存块的大小。free：释放先前分配的内存块。程序终止函数：exit：终止程序的执行。
#include <numeric>
//它包含了一些常用的函数，用于对序列中的数值进行累加、求和、乘积、最大值、最小值等操作。accumulate：计算序列中元素的累加值。inner_product：计算两个序列的内积。partial_sum：计算序列的部分和。adjacent_difference：计算序列的相邻差值。iota：为序列赋值递增的连续值。gcd：计算两个整数的最大公约数。lcm：计算两个整数的最小公倍数。

#include "Parallel.hh"
#include "Vec2.hh"
#include "Mesh.hh"

using namespace std;

```

###### 2.writecasefile

```c++
void ExportGold::writeCaseFile(//用于将数据导出到一个 .case 文件中
        const string& basename) {

    if (Parallel::mype > 0) return;//这个条件语句检查当前进程的排名是否大于 0。如果是，表示当前进程不需要执行 .case 文件的写入操作
    //即当前得要是非主进程，才能开始写入

    // open file
    const string filename = basename + ".case";
    ofstream ofs(filename.c_str());
    // filename.c_str() 是一个 C 风格的字符串，它返回一个指向 filename 字符串的
    //非常量字符数组的指针。
	//ofs 是一个 ofstream 类型的对象，它需要一个指向字符数组的指针作为参数来打开文件。
    //可以将 filename.c_str() 作为参数传递给 ofstream 对象的构造函数，以打开相应的文件。
    if (!ofs.good()) {
        cerr << "Cannot open file " << filename << " for writing"
             << endl;
        exit(1);
    }//个条件语句检查文件流是否成功打开。如果打开失败，输出错误信息并退出程序

    ofs << "#" << endl;
    ofs << "# Created by PENNANT" << endl;
    ofs << "#" << endl;
	//向 .case 文件写入一行注释。

ofs << "type: ensight gold" << endl;：

ofs << "GEOMETRY" << endl;：向 .case 文件写入 GEOMETRY 字段。
    ofs << "FORMAT" << endl;
    ofs << "type: ensight gold" << endl;
//ofs << "FORMAT" << endl;：向 .case 文件写入 FORMAT 字段。
//向 .case 文件写入 type: ensight gold，表示使用 ensight gold 格式。

    ofs << "GEOMETRY" << endl;
//向 .case 文件写入 GEOMETRY 字段。
    ofs << "model: " << basename << ".geo" << endl;

    ofs << "VARIABLE" << endl;
    ofs << "scalar per element: zr " << basename << ".zr" << endl;
    ofs << "scalar per element: ze " << basename << ".ze" << endl;
    ofs << "scalar per element: zp " << basename << ".zp" << endl;

    ofs.close();

}
```

函数的主要功能是创建一个名为 basename.case 的文件，并将一些固定格式的信息写入到该文件中。
然后，函数会向输出文件中写入一些固定的信息，作为文件的头部注释。使用 ofs 对象的 << 运算符将信息逐行写入文件。
依次写入以下信息：

“#”
“# Created by PENNANT”
“#”
“FORMAT”
“type: ensight gold”
“GEOMETRY”
"model: " + basename + “.geo”
“VARIABLE”
"scalar per element: zr " + basename + “.zr”
"scalar per element: ze " + basename + “.ze”
"scalar per element: zp " + basename + “.zp”

最后，函数关闭输出文件流，完成文件写入操作。

###### 2.writegoefile

```c++
void ExportGold::writeGeoFile(//将几何信息写入 .geo 文件
        const string& basename,
        const int cycle,
        const double time) {
    using Parallel::numpe;
    using Parallel::mype;

    // open file
    ofstream ofs;
    if (mype == 0) {
        const string filename = basename + ".geo";
        ofs.open(filename.c_str());
        if (!ofs.good()) {
            cerr << "Cannot open file " << filename << " for writing"
                 << endl;
            exit(1);
        }
    }//有排名为 0 的进程才会打开 .geo 文件并写入几何信息。其他进程会跳过这部分代码，不执行文件操作。这是为了避免多个进程同时写入同一个文件，导致文件内容被覆盖或混乱

    // write general header
    if (mype == 0) {
        ofs << scientific;
        ofs << "cycle = " << setw(8) << cycle << endl;
        //cycle = <cycle>：将周期号 cycle 写入文件，表示模型的当前周期。
        ofs << setprecision(8);
        ofs << "t = " << setw(15) << time << endl;
        //t = <time>：将时间 time 写入文件，表示模型的当前时间。
        ofs << "node id assign" << endl;
        //"node id assign"：表示节点 ID 的分配方式，这里是固定的字符串。
        ofs << "element id given" << endl;
		//"element id given"：表示元素 ID 的分配方式，这里是固定的字符串。
        // write header for the one "part" (entire mesh)
        ofs << "part" << endl;
        //"part"：表示下面的内容是关于一个部件的信息。
        ofs << setw(10) << 1 << endl;
        //1：部件的编号，这里是固定的。
        ofs << "universe" << endl;
    } 
    
    
    // gather node info to PE 0
    const int nump = mesh->nump;
    const double2* px = mesh->px;

    int gnump = nump;
    Parallel::globalSum(gnump);
    //对全局节点数进行求和，得到所有进程中的节点总数。
    
    
    //penump
    vector<int> penump(mype == 0 ? numpe : 0);
    //定义一个整数向量 penump，长度为 numpe（进程数）或者 0，根据当前进程的排名决定。
    Parallel::gather(nump, &penump[0]);
    //将各个进程的节点数收集到进程 0 上。将当前进程的节点数 nump 传递给 penump 向量。
    
    
    //peoffset
    vector<int> peoffset(mype == 0 ? numpe + 1 : 1);//定义一个整数向量 peoffset，长度为 numpe+1（进程数加 1）或者 1，根据当前进程的排名决定。
    partial_sum(penump.begin(), penump.end(), &peoffset[1]);
    //计算 penump 向量的前缀和，将结果存储到 peoffset 向量中。这个向量表示每个进程的起始节点索引。
    int offset;
    Parallel::scatter(&peoffset[0], offset);
    vector<double2> gpx(mype == 0 ? gnump : 0);
    //将 peoffset 向量中的数据进行分发，每个进程获取自己的偏移量 offset
    Parallel::gatherv(&px[0], nump, &gpx[0], &penump[0]);//将各个进程的节点坐标收集到进程 0 上。将当前进程的节点坐标传递给 gpx 向量。
	//将每个进程的节点数和节点坐标收集到进程 0 上，以便进一步处理和输出节点信息。通过这些收集操作，进程 0 将拥有全部节点的信息，并可以进行后续的处理和输出。
    
    
    
    // write node info
    if (mype == 0) {
        ofs << "coordinates" << endl;
        ofs << setw(10) << gnump << endl;
        ofs << setprecision(5);
        for (int p = 0; p < gnump; ++p)
            ofs << setw(12) << gpx[p].x << endl;
        for (int p = 0; p < gnump; ++p)
            ofs << setw(12) << gpx[p].y << endl;
        // Ensight expects z-coordinates, so write 0 for those
        for (int p = 0; p < gnump; ++p)
            ofs << setw(12) << 0. << endl;
    } // if mype

    const int* znump = mesh->znump;
    const int* mapsp1 = mesh->mapsp1;

    const int ntris = tris.size();
    const int nquads = quads.size();
    const int nothers = others.size();

    if (mype == 0) {
        pentris.resize(numpe);
        penquads.resize(numpe);
        penothers.resize(numpe);
    }
    Parallel::gather(ntris, &pentris[0]);
    Parallel::gather(nquads, &penquads[0]);
    Parallel::gather(nothers, &penothers[0]);

    gntris = accumulate(pentris.begin(), pentris.end(), 0);
    gnquads = accumulate(penquads.begin(), penquads.end(), 0);
    gnothers = accumulate(penothers.begin(), penothers.end(), 0);

    vector<int> pesizes(mype == 0 ? numpe : 0);

    // gather triangle info to PE 0
    vector<int> trip(3 * ntris);
    vector<int> gtris(gntris), gtrip(3 * gntris);
    Parallel::gatherv(&tris[0], ntris, &gtris[0], &pentris[0]);
    if (mype == 0) {
        for (int pe = 0; pe < numpe; ++pe)
            pesizes[pe] = pentris[pe] * 3;
    }
    for (int t = 0; t < ntris; ++t) {
        int z = tris[t];
        int sbase = mapzs[z];
        for (int i = 0; i < 3; ++i) {
            trip[t * 3 + i] = mapsp1[sbase + i] + offset;
        }
    }
    Parallel::gatherv(&trip[0], 3 * ntris, &gtrip[0], &pesizes[0]);

    // write triangles
    if (mype == 0 && gntris > 0) {
        ofs << "tria3" << endl;
        ofs << setw(10) << gntris << endl;
        for (int t = 0; t < gntris; ++t)
            ofs << setw(10) << gtris[t] + 1 << endl;
        for (int t = 0; t < gntris; ++t) {
            for (int i = 0; i < 3; ++i)
                ofs << setw(10) << gtrip[t * 3 + i] + 1;
            ofs << endl;
        }
    } // if mype == 0 ...

    // gather quad info to PE 0
    vector<int> quadp(4 * nquads);
    vector<int> gquads(gnquads), gquadp(4 * gnquads);
    Parallel::gatherv(&quads[0], nquads, &gquads[0], &penquads[0]);
    if (mype == 0) {
        for (int pe = 0; pe < numpe; ++pe)
            pesizes[pe] = penquads[pe] * 4;
    }
    for (int q = 0; q < nquads; ++q) {
        int z = quads[q];
        int sbase = mapzs[z];
        for (int i = 0; i < 4; ++i) {
            quadp[q * 4 + i] = mapsp1[sbase + i] + offset;
        }
    }
    Parallel::gatherv(&quadp[0], 4 * nquads, &gquadp[0], &pesizes[0]);

    // write quads
    if (mype == 0 && gnquads > 0) {
        ofs << "quad4" << endl;
        ofs << setw(10) << gnquads << endl;
        for (int q = 0; q < gnquads; ++q)
            ofs << setw(10) << gquads[q] + 1 << endl;
        for (int q = 0; q < gnquads; ++q) {
            for (int i = 0; i < 4; ++i)
                ofs << setw(10) << gquadp[q * 4 + i] + 1;
            ofs << endl;
        }
    } // if mype == 0 ...

    // gather other info to PE 0
    vector<int> othernump(nothers), otherp;
    vector<int> gothers(gnothers), gothernump(gnothers);
    Parallel::gatherv(&others[0], nothers, &gothers[0], &penothers[0]);
    for (int n = 0; n < nothers; ++n) {
        int z = others[n];
        int sbase = mapzs[z];
        othernump[n] = znump[z];
        for (int i = 0; i < znump[z]; ++i) {
            otherp.push_back(mapsp1[sbase + i] + offset);
        }
    }
    Parallel::gatherv(&othernump[0], nothers, &gothernump[0], &penothers[0]);
    int size = otherp.size();
    Parallel::gather(size, &pesizes[0]);
    int gsize = accumulate(pesizes.begin(), pesizes.end(), 0);
    vector<int> gotherp(gsize);
    Parallel::gatherv(&otherp[0], size, &gotherp[0], &pesizes[0]);

    // write others
    if (mype == 0 && gnothers > 0) {
        ofs << "nsided" << endl;
        ofs << setw(10) << gnothers << endl;
        for (int n = 0; n < gnothers; ++n)
            ofs << setw(10) << gothers[n] + 1 << endl;
        for (int n = 0; n < gnothers; ++n)
            ofs << setw(10) << gothernump[n] << endl;
        int gp = 0;
        for (int n = 0; n < gnothers; ++n) {
            for (int i = 0; i < gothernump[n]; ++i)
                ofs << setw(10) << gotherp[gp + i] + 1;
            ofs << endl;
            gp += gothernump[n];
        }
    } // if mype == 0 ...

    if (mype == 0) ofs.close();

}
```



### 4.WriteXY

### 5.parallel

##### 1.头文件分析

###### 1.stdint.h

`stdint.h` 是 C/C++ 标准库中的一个头文件，定义了一些固定大小的整数类型，以及相关的宏定义。该头文件提供了一种跨平台和可移植的方式来==使用特定大小的整数类型==，以==确保代码在不同平台和编译器上的一致性==。

在 `stdint.h` 头文件中定义了以下一些类型和宏：

- 固定大小的整数类型：
  - `int8_t`、`int16_t`、`int32_t`、`int64_t`：有符号整数类型，分别为8位、16位、32位、64位。
  - `uint8_t`、`uint16_t`、`uint32_t`、`uint64_t`：无符号整数类型，分别为8位、16位、32位、64位。
- 最小值和最大值宏定义：
  - `INT8_MIN`、`INT16_MIN`、`INT32_MIN`、`INT64_MIN`：有符号整数的最小值。
  - `INT8_MAX`、`INT16_MAX`、`INT32_MAX`、`INT64_MAX`：有符号整数的最大值。
  - `UINT8_MAX`、`UINT16_MAX`、`UINT32_MAX`、`UINT64_MAX`：无符号整数的最大值。

通过包含 `stdint.h` 头文件，可以使用这些固定大小的整数类型和宏定义，以便在代码中明确指定所需的整数类型大小，并保证代码的可移植性和可靠性。这==在需要确切控制整数类型大小的应用场景中非常有用==，如==嵌入式系统开发、网络编程和与硬件交互==等。

###### 2.mpi.h

MPI 是一种并行计算框架和标准，用于编写并行程序，实现==多个进程之间的消息传递和协同计算==。它提供了一套函数和数据类型，用于==在分布式内存系统上进行并行计算==。MPI 库的实现通常包括一个或多个头文件，其中 "mpi.h" 是 MPI 标准库中的主要头文件。
通过包含 "mpi.h" 头文件，可以在程序中使用 MPI 提供的函数和数据类型，实现并行计算和进程间通信。这些函数和数据类型包括：

进程管理函数：如初始化 MPI、获取进程编号和进程总数等。
通信函数：如发送和接收消息、广播数据、集合通信等。
数据类型：如自定义数据类型，用于在不同进程之间传递复杂的数据结构。
状态和错误处理：如获取通信状态、处理错误等。

###### 3.template typename T

  是 C++ 中的模板声明语法，用于定义一个通用的、可用于不同数据类型的模板。
在这个代码片段中，template<typename T> 表示要定义的函数 gatherv() 和 gathervImpl() 是一个==模板函数==，并且该模板函数可以==接受任意类型的参数 T==。
使用模板可以实现代码的复用和泛化，使得==函数能够适用于不同的数据类型==，而不仅限于特定的数据类型。通过在模板定义中使用 typename 关键字，可以在定义函数时使用通用的类型参数 T，而不需要指定具体的类型。
例如，当调用 gatherv() 函数时，可以根据需要将不同的数据类型作为模板参数 T，例如 int、float 或自定义的类类型。编译器会根据传递的参数类型自动生成相应的函数定义，并在编译时进行类型推断。
使用模板可以提高代码的灵活性和可重用性，使函数适用于多种数据类型的操作，并减少了需要编写多个类似函数的重复工作。

###### 4.impl

`impl` 通常用于表示与实现相关的部分或者具体的实现代码，以区分于接口或者抽象部分

##### 2.namespace parallel .hh

```c++
//提供了一些用于分布式并行计算的辅助函数和变量，并描述了这些函数和变量的功能。
// Namespace Parallel provides helper functions and variables for running in distributed parallel mode using MPI, or for stubbing these out if not using MPI.

namespace Parallel {
    extern int numpe;           // number of MPI PEs in use numpe：表示正在使用的 MPI（Message Passing Interface）进程的数量。如果没有使用 MPI，则值为 1。
    extern int mype;            // PE number for my rank 表示当前进程的编号（PE number）。如果没有使用 MPI，则值为 0。
    void init();                // initialize MPI 用于初始化 MPI。
    void final();               // finalize MPI 用于结束并关闭 MPI。
    void globalMinLoc(double& x, int& xpe);
                                // find minimum over all PEs, and report which PE had the minimum 在所有进程中找到最小值，并报告具有最小值的进程编号。
    void globalSum(int& x);     // find sum over all PEs - overloaded 分别用于求和（sum）操作，将结果汇总到所有进程。
    void globalSum(int64_t& x);
    void globalSum(double& x);
    void gather(const int x, int* y);
                                // gather list of ints from all PEs 从所有进程中收集整数列表。
    void scatter(const int* x, int& y);
                                // gather list of ints from all PEs 将整数列表分发到所有进程。

    template<typename T>
    void gatherv(               // gather variable-length list用于收集变长列表
            const T *x, const int numx,
            T* y, const int* numy);
    template<typename T>
    void gathervImpl(           // helper function for gatherv 是 gatherv() 函数的辅助函数
            const T *x, const int numx,
            T* y, const int* numy);

}  // namespace Parallel
```

###### (1).namespace与class区别

在给定的上下文中，选择使用 `namespace` 而不是 `class` 可能是因为作者想要创建一个命名空间，用于组织一些辅助函数和变量，而==不需要定义一个具有状态和行为的类==。使用命名空间可以更轻量地组织代码，而==不需要定义和实例化一个类的对象==。

使用 namespace 的示例：

```
// 使用 namespace 组织辅助函数和常量
namespace MathUtils {
    const double PI = 3.14159265359;

double square(double x) {
    return x * x;
}

double cube(double x) {
    return x * x * x;
}

}

// 使用 namespace 避免命名冲突
namespace LibraryA {
    void foo() {
        // ...
    }
}

namespace LibraryB {
    void foo() {
        // ...
    }
}

int main() {
    double result = MathUtils::square(5.0);  // 调用 MathUtils 命名空间中的函数
    // ...

LibraryA::foo();  // 调用 LibraryA 命名空间中的函数
LibraryB::foo();  // 调用 LibraryB 命名空间中的函数

return 0;

}
```


使用 class 的示例：

```
// 使用 class 封装数据和行为
class Rectangle {
private:
    double length;
    double width;

public:
    Rectangle(double l, double w) : length(l), width(w) {}

double getLength() const {
    return length;
}

double getWidth() const {
    return width;
}

double getArea() const {
    return length * width;
}

};

// 使用 class 创建对象
int main() {
    Rectangle r(5.0, 3.0);  // 创建 Rectangle 类的对象

double length = r.getLength();  // 调用对象的成员函数
double width = r.getWidth();
double area = r.getArea();

// ...

return 0;

}
```

在这两个示例中，namespace 用于组织辅助函数、常量和避免命名冲突，而 class 则用于封装数据和操作，并通过创建对象来使用类的功能。
需要根据具体的需求和设计来选择使用 namespace 还是 class。在实际开发中，常常会同时使用两者，使用 namespace 来组织不同的功能模块和避免命名冲突，使用 class 来封装数据和操作，并实现面向对象的特性。

###### (2).辅助函数

 辅助函数（Helper Function）是在程序中用来==支持主要功能函数的额外函数。它们通常用于处理一些特定的任务，以提供更好的==代码组织和可重用性。
辅助函数具有以下特点：

辅助函数通常被设计为==可重复使用==的代码块，用于执行一些通用的操作或算法。它们==可以被多个函数或类调用==，提供了代码复用的机制。
辅助函数通常不直接与用户交互或进行复杂的业务逻辑处理。相反，它们常常用于执行一些简单而具体的计算、转换、验证等任务。
辅助函数可以提高代码的可读性和可维护性，通过将复杂的逻辑分解为更小、更可理解的函数，使得主要功能函数更加简洁和易于理解。
辅助函数可以帮助减少代码的重复性，通过将一些常用的操作封装为函数，可以在不同的上下文中使用，避免了重复编写相似的代码。
辅助函数通常位于命名空间中，以组织相关的函数和变量，并提供更好的代码模块化和组织性。

辅助函数的具体应用可以有很多种场景，比如字符串处理、数学计算、数据转换、输入验证等。它们的作用是为主要功能函数提供支持和辅助，以提高代码的可读性、可维护性和重用性。

###### (3)常量成员函数constant

 在函数声明中加上 const 关键字，表示该成员函数是一个==常量成员函数==。常量成员函数在函数体内不会修改对象的状态（即成员变量的值），它们被设计为==不会对对象进行任何修改==的操作。
在给定的示例中，==getLength()== 函数获取矩形对象的长度，但它并不会修改对象的状态。因此，将其声明为常量成员函数可以提供以下几个好处：

类型安全性：通过将常量成员函数声明为 const，编译器可以强制确保函数内部不会修改对象的状态。如果在常量成员函数内部尝试修改成员变量的值，编译器会报错，从而提供了类型安全性。
代码可读性：在函数声明中加上 const 可以提醒其他开发人员和代码维护者，该函数不会修改对象的状态。这有助于读者更好地理解函数的行为和目的，提高代码的可读性和可理解性。
对常量对象的支持：常量成员函数可以在常量对象上调用，而不仅限于非常量对象。这意味着即使通过常量对象或常量引用调用函数，仍然可以访问和使用常量成员函数。

总而言之，将成员函数声明为常量成员函数是一种良好的编程实践，它可以提供类型安全性、可读性，以及对常量对象的支持。然而，并非所有的成员函数都适合声明为常量成员函数，只有在==函数内部不会修改对象状态==的情况下，才应该使用 const 修饰符。

##### 3.parallen .cc

###### 1.ifdef USE_MPI

```c++
#ifdef USE_MPI
// We're running under MPI, so set these to dummy values
// that will be overwritten on MPI_Init.
int numpe = 0;
int mype = -1;
#else
// We're in serial mode, so only 1 PE.
int numpe = 1;
int mype = 0;
#endif
```

 根据给定的代码片段，根据 USE_MPI 宏的定义与否，初始化了 numpe 和 mype 变量的值。
如果 USE_MPI 宏==被定义==，即代码在 MPI 环境中编译和执行，那么 numpe 和 mype 的初始值将被==设置为虚拟值==。这些虚拟值在调用 MPI_==Init()== 函数后将被 ==MPI 库中的实际值覆盖==。因此，在 MPI 初始化之前，numpe 的初始值为 0，mype 的初始值为 -1。

如果 USE_MPI 宏未定义，即代码在==串行模式下==编译和执行，那么 numpe 和 mype 的初始值将被设置为串行模式下的值。在这种情况下，numpe 的初始值为 1，表示只有一个处理单元（Processing Element），mype 的初始值为 0，表示当前进程的编号为 0。

该代码片段的目的是为了在编译时根据是否使用 MPI 来设置并行环境中的处理单元数和当前进程的编号。这样可以在后续的程序中根据这些变量的值来进行相应的并行计算和通信操作。

###### 2.init()

```
void init() {
#ifdef USE_MPI
    MPI_Init(0, 0);
    MPI_Comm_size(MPI_COMM_WORLD, &numpe);
    MPI_Comm_rank(MPI_COMM_WORLD, &mype);
#endif
}  // init
```

 给定的 init() 函数的作用是在使用 MPI（Message Passing Interface）的情况下初始化 MPI 环境，并获取并行进程的数量和当前进程的编号。
具体而言，以下是 init() 函数的作用：

MPI_Init(0, 0);：这是 MPI 库的初始化函数，用于初始化 MPI 环境。==通过调用 MPI_Init，程序将进入 MPI 并行计算环境==。

在调用 `MPI_Init` 之前，程序处于串行模式下，调用该函数后，程序进入 MPI 并行计算环境。

`0, 0`：这两个参数是传递给 `MPI_Init` 函数的参数。通常情况下，这两个参数都是 0，表示使用默认的参数值。第一个参数是 `argc`，表示==命令行参数的数量==，第二个参数是 `argv`，表示==命令行参数的数组==。通过将这两个参数设置为 0，表示不传递任何命令行参数给 MPI 环境。

需要注意的是，`MPI_Init` 函数只能在程序中调用一次，通常在程序的起始阶段调用。在程序结束时，应使用 `MPI_Finalize` 函数来终止 MPI 环境，并释放相关的资源

MPI_Comm_size(MPI_COMM_WORLD, &numpe);：这是一个 MPI 函数，用于获取并行计算中的进程数量。MPI_COMM_WORLD 是 MPI 的==默认通信域==，MPI_Comm_size 函数将==返回该通信域中的进程数量==，并将该值存储在 numpe 变量中。
MPI_Comm_rank(MPI_COMM_WORLD, &mype);：这也是一个 MPI 函数，用于获取当前进程在并行计算中的编号。MPI_Comm_rank 函数将返回当前进程的编号，并将该值存储在 mype 变量中。

因此，init() 函数的作用是在 MPI 环境中初始化并行计算，以及获取并行进程的数量和当前进程的编号。这些信息可以在后续的并行计算和通信操作中使用，以实现并行算法和任务的分发与协调。需要注意的是，为了使用 init() 函数，需要在程序的其他部分正确调用和初始化 numpe 和 mype 变量。

###### 3.globalMinLoc

```c++
void globalMinLoc(double& x, int& xpe) {
    if (numpe == 1) {
        xpe = 0;
        return;
    }//这个条件判断语句检查并行环境中的进程数量 numpe 是否为 1。如果是，表示只有一个进程在运行，无需进行全局最小值的计算。在这种情况下，将 xpe 设置为 0，表示最小值所在的进程编号为 0，并直接返回。
#ifdef USE_MPI//如果定义了，表示代码在 MPI 环境中进行编译和执行，需要执行 MPI 相关的计算和通信操作。如果没有定义，表示代码在串行模式下编译和执行，不需要进行 MPI 相关操作。
    struct doubleInt {
        double d;
        int i;//用于在 MPI_Allreduce 函数中传递并存储最小值和对应的进程编号==。
    } xdi, ydi;
    xdi.d = x;
    xdi.i = mype;
    
    MPI_Allreduce(&xdi, &ydi, 1, MPI_DOUBLE_INT, MPI_MINLOC,
            MPI_COMM_WORLD);//用于在所有进程中进行全局的最小值计算。MPI_Allreduce 函数将 xdi 结构体中的数据在所有进程中进行归约操作，以找到全局最小值，并将结果存储在 ydi 结构体中。
    //1：这是发送缓冲区中要归约的元素的数量。在这个例子中，只有一个元素需要进行归约。MPI_DOUBLE_INT：这是 MPI 的预定义数据类型之一，用于指定发送和接收缓冲区的数据类型。在这个例子中，xdi 和 ydi 都是由一个 double 类型的值和一个 int 类型的值组成。MPI_MINLOC：这是归约操作的操作符，表示进行最小值归约。在这个例子中，归约操作将找到最小值并返回其在进程中的位置。
    x = ydi.d;
    xpe = ydi.i;
#endif
}
```

 给定的 globalMinLoc() 函数用于==在并行环境中找到全局的最小值以及该最小值所在的进程编号==。

在==串行模式==下或只有一个进程的情况下，直接==将最小值设置为传入的参数 x，并将进程编号设置为 0==。

###### （1）规约操作

 归约（reduction）操作是一种在并行计算中广泛使用的操作，用于==将多个进程或线程的数据合并成一个结果==。
在归约操作中，每个进程或线程都有一个本地数据，这些本地数据将经过指定的操作（如求和、求最大值、求最小值等）进行合并，形成一个全局的结果。
归约操作的主要目的是在并行计算中进行数据的聚合和整合，以便得到==全局的结果==。这样，每个进程或线程都可以通过归约操作得到整个并行计算的结果，而不需进行显式的通信和同步。
在 MPI（Message Passing Interface）中，提供了一系列的归约操作函数，如求和、求最大值、求最小值、位逻辑操作（与、或、异或）等。这些函数可用于在 MPI 并行环境中进行归约操作。
通过归约操作，可以实现并行计算中的全局通信和数据聚合，提高并行算法的效率和可扩展性。归约操作在许多并行计算任务中都是必不可少的，如并行排序、并行搜索、并行优化等。

###### 4.globalsum(int64_t)

```c++
void globalSum(int& x) {
    if (numpe == 1) return;//如果是，表示只有一个进程在运行，无需进行全局整数和的计算
#ifdef USE_MPI
    int y;
    MPI_Allreduce(&x, &y, 1, MPI_INT, MPI_SUM, MPI_COMM_WORLD);//这是一个 MPI 函数调用，用于在所有进程中进行全局的整数和计算。MPI_Allreduce 函数将所有进程中的 x 的值相加，并将结果存储在 y 中。MPI_INT 是一个 MPI 的预定义数据类型，用于指定 x 和 y 的数据类型。
    x = y;
#endif
}//使用 MPI_Allreduce 函数进行全局整数和的计算，并将结果存储在传入的参数 x 中
```

```c++
void globalSum(int64_t& x) {
    if (numpe == 1) return;
#ifdef USE_MPI
    int64_t y;
    MPI_Allreduce(&x, &y, 1, MPI_INT64_T, MPI_SUM, MPI_COMM_WORLD);
    x = y;
#endif
}//大小：int 是一种标准整数数据类型，通常为32位，可以表示的整数范围为约-2,147,483,648到2,147,483,647。而 int64 是一种64位整数数据类型，可以表示更大范围的整数，大约为-9,223,372,036,854,775,808到9,223,372,036,854,775,807。
```

###### 5.gather

```c++
void gather(int x, int* y) {//在并行计算中将多个进程中的数据收集到一个进程中
    if (numpe == 1) {
        y[0] = x;
        return;
    }
#ifdef USE_MPI
    MPI_Gather(&x, 1, MPI_INT, y, 1, MPI_INT, 0, MPI_COMM_WORLD);//将所有进程中的 x 值收集到 y 数组中
    //&x：发送缓冲区的起始地址，即要发送的数据。1：发送缓冲区中的数据数量。
    //0：数据将被收集到进程标识号为 0 的进程
#endif
}//在 MPI 环境中，使用 MPI_Gather 函数将所有进程中的 x 值收集到 y 数组中。
```

###### 6.scatter

```c++
void scatter(const int* x, int& y) {
    if (numpe == 1) {
        y = x[0];
        return;
    }
#ifdef USE_MPI
    MPI_Scatter((void*) x, 1, MPI_INT, &y, 1, MPI_INT, 0, MPI_COMM_WORLD);
    //发送缓冲区的起始地址，1：发送缓冲区中的数据数量 
    //0:数据最初的来源进程的标识号
    //将进程 0 中的整数数组 x 的数据散布到各个进程中的整数变量 y 中
#endif
}
```

###### 7.gatherv( template typename T)

作用是允许在编写函数或类时使用通用的数据类型，而不是特定的数据类型。通过使用模板，可以在代码中==使用 T 作为占位符==，表示将来==可以使用任何数据类型作为实参==。
例如，对于函数模板 template<typename T> void foo(T value)，可以根据需要在不同的地方使用不同类型的参数调用它，例如 foo(5)、foo("hello") 或 foo(3.14)。

==模板会根据实际调用时的参数类型来生成特定的函数定义==，从而实现对不同数据类型的通用处理。

```c++
template<>
void gatherv(
        const double2 *x, const int numx,
        double2* y, const int* numy) {
    gathervImpl(x, numx, y, numy);
}


template<>
void gatherv(
        const double *x, const int numx,
        double* y, const int* numy) {
    gathervImpl(x, numx, y, numy);
}


template<>
void gatherv(
        const int *x, const int numx,
        int* y, const int* numy) {
    gathervImpl(x, numx, y, numy);
}

```

```c++
template<typename T>
void gathervImpl(//将数据从各个进程收集到一个进程中
        const T *x, const int numx,
    //参数 x 是指向待发送数据的指针，numx 是待发送数据的数量
        T* y, const int* numy) {
	//参数 y 是指向接收数据的指针，numy 是一个整型数组，表示每个进程接收到的数据数量
    if (numpe == 1) {
        std::copy(x, x + numx, y);
        return;
    }// std::copy 函数将数据从 x 复制到 y
#ifdef USE_MPI
    const int type_size = sizeof(T);
    int sendcount = type_size * numx;
    std::vector<int> recvcount(1, 0), disp(1, 0);//定义两个整型向量 recvcount 和 disp，初始大小都为 1，并初始化为 0。
    //recvcount存储接收数据的数量
    //disp相对于接收缓冲区的偏移量
    if (mype == 0) {
        recvcount.resize(numpe);
        for (int pe = 0; pe < numpe; ++pe) {
            recvcount[pe] = type_size * numy[pe];}
        //为每个进程设置相应的接收数据数量
        disp.resize(numpe + 1);
        //每个进程接收到的数据的偏移量。偏移量是相对于接收缓冲区起始地址的位置
        std::partial_sum(recvcount.begin(), recvcount.end(), &disp[1]);
        // 计算 recvcount 向量的部分和，并将结果存储在 disp 向量中的第二个元素开始的位置。
		//通过计算部分和，可以得到每个进程在接收缓冲区中的偏移量，这样就能够正确地将数据放置在接收缓冲区中的相应位置。
    } //该条件语句只在主进程（进程 0）中执行，用于设置接收数据的数量和偏移量
	MPI_Gatherv((void*) x, sendcount, MPI_BYTE,y, &recvcount[0], &disp[0], MPI_BYTE, 0, MPI_COMM_WORLD);
    //(void*) x：这是发送缓冲区的起始地址，其中 x 是一个指向待发送数据的指针。
	//sendcount：这是发送缓冲区中的数据数量。在这个例子中，sendcount 的值是通过计算数据类型大小和待发送数据数量得到的。
    //MPI_BYTE：这是发送缓冲区中数据的数据类型，指示数据按字节发送。在这个例子中，T 是模板类型，sizeof(T) 表示数据类型 T 的大小，因此使用 MPI_BYTE 表示按字节发送数据。
	//y：这是接收缓冲区的起始地址，其中 y 是一个指向接收数据的指针。
	//&recvcount[0]：这是一个整型数组，用于指定每个进程接收到的数据数量。&recvcount[0] 是指向指定接收数据数量的数组的指针。在这个例子中，recvcount 是一个整型向量，存储了每个进程接收到的数据数量。
	//&disp[0]：这是一个整型数组，用于指定每个进程接收到的数据的偏移量。&disp[0] 是指向指定接收数据偏移量的数组的指针。在这个例子中，disp 是一个整型向量，存储了每个进程接收到的数据的偏移量。
	//0：这是接收数据的进程的排名。在这个例子中，进程 0 是接收数据的进程。
	//MPI_COMM_WORLD：这是 MPI 通信子（communicator），用于指定通信的范围。MPI_COMM_WORLD 表示所有正在运行的进程。

	//综上所述，MPI_Gatherv() 函数的作用是将各个进程中的数据收集到一个进程中。它使用发送缓冲区的起始地址、发送缓冲区中的数据数量、发送数据的数据类型、接收缓冲区的起始地址、每个进程接收到的数据数量数组、每个进程接收到的数据的偏移量数组、接收数据的进程的排名和通信子来进行数据的收集操作。
    //使用 MPI_Gatherv 函数将各个进程中的数据收集到一个进程中，并使用 recvcount 和 disp 数组指定每个进程收集到的数据数量和偏移量

```

























































































































