# 其他编译方式

环境
  Win11 22H2
预计总占用空间10GB左右
VS2022 6GB左右
Blackbone 1.84GB
OP 1.66GB

1.首先安装VS2022
https://visualstudio.microsoft.com/zh-hans/downloads/
直接选择企业版免费试用(Visual Studio Enterprise 2022)
需要勾选"使用 C++ 的桌面开发"、".NET 桌面开发"(编译Blackbone需要.net)
单个组件->找到Windows 10 SDK字样组件，注意这里的版本10.0.22621.0及以上才支持wgc编译
找不到的则可选择不编译wgc或者（不推荐)升级windows版本

安装完成后
帮助->注册 Visual Studio -> 使用产品密匙解锁
企业版密匙：VHF9H-NXBBB-638P6-6JHCY-88JWH

2.编译Blackbone
https://github.com/DarthTon/Blackbone
使用vs2022克隆完毕后，直接使用vs打开项目编译"Release Win32"及"Release x64"
有报错？不用怕（略）
设置环境变量'BLACKBONE_ROOT'为Blackbone项目的根目录

3.编译OP
https://github.com/WallBreaker2/op
使用vs2022克隆完毕后，直接使用vs打开cmake项目会存在报错
1.编辑op工程下'CMakeSettings'文件(拖入vs）将报错的配置都删除掉
    "-DCMAKE_TOOLCHAIN_FILE=D:/workspace/vcpkg/scripts/buildsystems/vcpkg.cmake"

2.编辑op工程下'CMakeLists'文件(拖入vs) 注释掉'tests项目'
3.重新生成cmake缓存，此时是可以运行"x86-Debug op_x86.dll"的；运行后'报错dll不能运行'就代表生成成功
4.将'tests项目'注释还原(取消注释)，生成"x86-Debug op_test.exe"；此时报错exe不能运行代表生成成功
5.编辑'tests项目'的'CMakeLists'文件(拖入vs) 第一个'SET'语句下一行后面添加代码
```
IF(CMAKE_CL_64)
set(CMAKE_LIBRARY_PATH ${CMAKE_LIBRARY_PATH} "${PROJECT_SOURCE_DIR}/out/build/x64-Debug/libop/")
ELSE(CMAKE_CL_64)
set(CMAKE_LIBRARY_PATH ${CMAKE_LIBRARY_PATH} "${PROJECT_SOURCE_DIR}/out/build/x86-Debug/libop/")
ENDIF(CMAKE_CL_64)
```
'add_executable'语句下一行添加
```
SET(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/../libop/)
```
6.此时tests项目能正常运行