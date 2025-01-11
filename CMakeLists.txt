cmake_minimum_required(VERSION 3.22)
project(matplot_test)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 单独设置 Address Sanitizer 和 Undefined Behavior Sanitizer
set(SANITIZER_FLAGS "-fsanitize=address -fsanitize=undefined")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${SANITIZER_FLAGS}")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${SANITIZER_FLAGS}")

# 查找 Matplot++ 库
find_package(Matplot++ REQUIRED)
if(TARGET Matplot++::matplot)
    message(STATUS "*** Matplot++ found ***")
else()
    message(FATAL_ERROR "Matplot++ not found. Please ensure it is installed correctly.")
endif()

# 添加可执行文件
add_executable(matplottest
    matplot_test.cpp
)

# 链接 Matplot++ 库
target_link_libraries(matplottest
    PUBLIC
    Matplot++::matplot
)

# 确保 Matplot++ 和项目一致使用 sanitizer
target_compile_options(matplottest PRIVATE -fsanitize=address -fsanitize=undefined)
target_link_options(matplottest PRIVATE -fsanitize=address -fsanitize=undefined)
