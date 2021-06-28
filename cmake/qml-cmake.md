
# 用cmake编译qt-qml项目

## 工具
- windows



- linux

    cmake和GUNC的make就行了

## 开始

先创建一个Qt qml项目，可以通过Qt Creator或手动创建，毕竟用不到.pro文件。

创建一个简易项目后，在项目文件夹下新建`CMakeLists.txt`文件，编写该文件。

代码：
```c
# cmake的最低版本
cmake_minimum_required(VERSION 3.1.0)
# 工程名
project(qml_cmake)

# 自动扫描当前路径下的include文件
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# 自动运行moc
set(CMAKE_AUTOMOC ON)

# 添加qt包
find_package(Qt5 REQUIRED Qml Quick Gui)
# 添加qml界面文件
qt5_add_resources(qml_QRC 
    qml.qrc
# other.qrc
)

# 添加代码文件
set(SOURCE
	main.cpp
# other.cpp
)


# 指定可执行文件
add_executable(qcmake ${SOURCE} ${qml_QRC})

# 设置链接库
target_link_libraries(qcmake Qt5::Qml Qt5::Quick)

message("Run cmake success")
```

## 编译
```
cmake CMakeLists.txt
make
```

如果将编译结果输出到一个指定文件夹，可以使用
```
cmake CMakeLists.txt -B ${buildFile}
```
也可以在`${buildFile}`中`cmake ${srcPath}`