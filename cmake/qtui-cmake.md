
# cmake编译qt widget

```c
cmake_minimum_required(VERSION 3.1.0)

project(qtui_cmake)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

set(CMAKE_INCLUDE_CURRENT_DIR ON)


find_package(Qt5 COMPONENTS Widgets REQUIRED Quick)

# ui文件需要跟cpp文件在一起
add_executable(qcmake
	src/mainwindow.ui
	src/main.cpp
	src/mainwindow.cpp
)

target_link_libraries(qcmake Qt5::Widgets Qt5::Quick)

message("Cmake run success")
```