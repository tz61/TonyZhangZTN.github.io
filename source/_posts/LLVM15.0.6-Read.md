# CMakeFiles.txts

## llvm/CMakeFiles.txt

modules 在`llvm/CMakeLists.txt` 237行被添加到`CMAKE_MODULE_PATH`中

```cmake
list(INSERT CMAKE_MODULE_PATH 0
  "${CMAKE_CURRENT_SOURCE_DIR}/cmake"
  "${CMAKE_CURRENT_SOURCE_DIR}/cmake/modules"
  "${LLVM_COMMON_CMAKE_UTILS}/Modules" #under llvm-project/cmake/Modules
  )
#注意，这个source_dir是llvm-project/llvm
```

line 267 `include(VersionFromVCS)`  

line 308 检测是否为in source build

line 327 `include(GNUInstallPackageDir)` 

line 368 `LLVM_ALL_TARGETS` 为所有targets

line 623 控制各模块是否编译

```cmake
# Define options to control the inclusion and default build behavior for
# components which may not strictly be necessary (tools, examples, and tests).
#
# This is primarily to support building smaller or faster project files.
```

line 658 文档生成相关

```cmake
option (LLVM_BUILD_DOCS "Build the llvm documentation." OFF)
option (LLVM_INCLUDE_DOCS "Generate build targets for llvm documentation." ON)
option (LLVM_ENABLE_DOXYGEN "Use doxygen to generate llvm API documentation." OFF)
option (LLVM_ENABLE_SPHINX "Use Sphinx to generate llvm documentation." OFF)
option (LLVM_ENABLE_OCAMLDOC "Build OCaml bindings documentation." ON)
option (LLVM_ENABLE_BINDINGS "Build bindings." ON)
```

line 776 意思是说在这行之前，所有llvmoptions应该被设置完成了，否则会出现错误

```cmake
# All options referred to from HandleLLVMOptions have to be specified
# BEFORE this include, otherwise options will not be correctly set on
# first cmake run
include(config-ix) #llvm/cmake/config-ix.cmake 处理头文件查找等工作，以及
```

line 817 `include(HandleLLVMOptions)`  编译器选项+编译器检查

line 825 解析各targets并查找有无asmprinter

line 901~973 tensorflow lib相关

line 975  输出目录下config.h生成

```cmake
# Configure the three LLVM configuration header files.
configure_file(
  ${LLVM_MAIN_INCLUDE_DIR}/llvm/Config/config.h.cmake#llvm/include/llvm~
  ${LLVM_INCLUDE_DIR}/llvm/Config/config.h)#(-B的目录)/include/llvm/~
configure_file(
  ${LLVM_MAIN_INCLUDE_DIR}/llvm/Config/llvm-config.h.cmake
  ${LLVM_INCLUDE_DIR}/llvm/Config/llvm-config.h)
configure_file(
  ${LLVM_MAIN_INCLUDE_DIR}/llvm/Config/abi-breaking.h.cmake
  ${LLVM_INCLUDE_DIR}/llvm/Config/abi-breaking.h)
```

line 986~1003 rpm包生成相关

line 1005~ apple/AIX/OS390/SunOS 等平台 link command，FLAGS调整

line 1091 

```cmake
include(AddLLVM)
include(TableGen)

include(LLVMDistributionSupport)
```

line 1107

```cmake
add_subdirectory(lib/Demangle)
add_subdirectory(lib/Support)
add_subdirectory(lib/TableGen)

add_subdirectory(utils/TableGen)

add_subdirectory(include/llvm)

add_subdirectory(lib)

if( LLVM_INCLUDE_UTILS )
  add_subdirectory(utils/FileCheck)
  add_subdirectory(utils/PerfectShuffle)
  add_subdirectory(utils/count)
  add_subdirectory(utils/not)
  add_subdirectory(utils/UnicodeData)
  add_subdirectory(utils/yaml-bench)
else()
  if ( LLVM_INCLUDE_TESTS )
    message(FATAL_ERROR "Including tests when not building utils will not work.
    Either set LLVM_INCLUDE_UTILS to On, or set LLVM_INCLUDE_TESTS to Off.")
  endif()
endif()
```

后面是一些测试单元，没必要过多分析

如下为各module分析

# cmake Modules

## preview

主要的脚本在如下几个文件夹下
clang/cmake/cache
clang/cmake/modules
llvm/cmake/modules

# llvm/cmake/modules

## AddLLVM.cmake

### 

# clang/cmake/modules

## AddClang.cmake

### add_clang_library
