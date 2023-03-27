cmake_minimum_required(VERSION 3.5)

# 设置项目名称
project(mylibrary)

# 指定要编译的源代码文件
set(SOURCES
    mylibrary.cpp
)

# 指定要生成的共享库文件名及类型
set(LIBRARY_NAME mylibrary)
set(LIBRARY_TYPE SHARED)

# 指定生成的共享库输出路径
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

# 指定生成的共享库文件名及类型
add_library(${LIBRARY_NAME} ${LIBRARY_TYPE} ${SOURCES})

# 指定生成的共享库的安装路径
install(TARGETS ${LIBRARY_NAME} LIBRARY DESTINATION /usr/local/lib)

# 指定mylibrary.h文件的安装路径
install(FILES mylibrary.h DESTINATION /usr/local/include)
