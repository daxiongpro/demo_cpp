cmake_minimum_required(VERSION 3.5)

project(MyProject)

# 设置C++标准为C++11
set(CMAKE_CXX_STANDARD 11)

# 添加头文件搜索路径
include_directories(/usr/local/include)

# 添加库文件搜索路径
link_directories(/usr/local/lib)

# 编译可执行文件
add_executable(demo demo.cpp)

# 链接库文件
target_link_libraries(demo mylibrary)
