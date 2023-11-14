
### 设置版本号
```
cmake_minimum_required(VERSION 3.14)   //指定运行此配置文件所需的 CMake 的最低版本；
```

### 设置工程名

```
project(PROJECT_NAME)
```

### 添加编译选项
```
add_compile_options(-g -Wunused)
```

### 添加执行
```
add_executable(main main.cpp)   //将名为 main.cpp 的源文件编译成一个名称为 main 的可执行文件
```

### 添加目标编译选项
```
target_compile_options(main PUBLIC -Wall -Werror)
```
### 增加-std=c++11
```
set(CMAKE_CXX_STANDARD 11)
```

### 添加源文件路径
```
file(GLOB_RECURSE SOURCES "src/*.cpp" "src1/*.cpp*")
```

### 添加头文件搜索路径
```
include_directories(./src/ ./include/)
```

### 添加动静态链接库
```
set(MUL_SOURCES ./mul/mul.cpp)
add_library(mul STATIC ${MUL_SOURCES})   # 静态链接库
add_library(mul SHARED ${MUL_SOURCES})   # 动态链接库

# 链接到所有目标
link_directories(./)
link_libraries(mul)

# 链接到目标
target_link_directories(main PUBLIC ./)
target_link_libraries(main mul)
```