# C++ 极简 demo

本项目介绍，如何写一个 C++ 库，安装到 Linux 系统中，并在另一个项目中使用这个库。

* 本项目的系统头文件是：`/usr/local/include`
* 系统库文件目录是：`/usr/local/lib`

原理：

* 自己编写的库的源文件首先编译成动态链接库 .so，动态链接库放到系统目录中。
* 当使用这个库的时候，会自动从系统中寻找这个 .so 文件。
* 系统如何寻找这个 .so 文件？根据环境变量：`$LD_LIBRARY_PATH`。

## 1.目录介绍

* `mylibrary` 为本项目库文件夹，里面存放库的源文件（.cpp）和头文件（.h）
* `my_demo_proj` 为本项目 demo 文件夹，模拟另一个项目，来使用 mylibarary 库。
* `项目工作目录` = 项目根目录： `/demo_cpp`。

```shell
.
├── my_demo_proj
│   ├── CMakeLists.txt
│   ├── demo.cpp
│   └── README.md
├── mylibrary
│   ├── CMakeLists.txt
│   ├── mylibrary.cpp
│   └── mylibrary.h
└── README.md
```

## 2.将 mylibrary 库编译并安装到系统

简而言之分为两步：

* 将源文件（.cpp） 编译成库文件（.so）
* 将库文件（.so）拷贝（安装）到系统中。

### 2.1.编译安装

#### 方法一：使用 cmake 编译安装（推荐）

```
# 切换到项目工作目录
cd mylibrary
mkdir build && cd build
cmake .. && sudo make install
```

#### 方法二：使用 g++ 直接编译

* 2.1.1.编译

```
# 切换到项目工作目录
cd mylibrary
mkdir build && cd build
g++ -shared -fPIC ../mylibrary.cpp -o libmylibrary.so
```

此时编译出 `libmylibrary.so` 文件

* 2.1.2.复制头文件和库文件到系统中

```
# 切换到项目工作目录
cd mylibrary/build
sudo cp libmylibrary.so /usr/local/lib
sudo cp mylibrary.h /usr/local/include
```

通过以上两种方法都可以将库安装到系统中。

### 2.2.配置环境变量

* Linux 系统通过环境变量 `$LD_LIBRARY_PATH` 寻找 `.so` 文件，可以将 `/usr/local/lib` 添加到这个环境变量中。
* 当 `#include "mylibrary.h"` 的时候，系统会自动从 `"/usr/include"` `"/usr/local/include"` 等系统目录中寻找头文件。
* 当编译的时候，系统会自动从 `$LD_LIBRARY_PATH` 目录中寻找 `.so` 文件。

## 3.在 demo 项目使用 mylibrary 库

#### 方法1：使用 cmake

```
# 切换到项目工作目录
cd my_demo_proj
mkdir build && cd build
cmake .. && make -j8
```

此时在 `{项目工作目录}/my_demo_proj/build` 目录里面生成可执行文件 `demo`。

#### 方法2：使用 g++

```
# 切换到项目工作目录
cd my_demo_proj
mkdir build && cd build
g++ ../demo.cpp -lmylibrary -o demo
```

## 4.运行 demo 项目

```
# 切换到项目工作目录
cd my_demo_proj/build
./demo
```
