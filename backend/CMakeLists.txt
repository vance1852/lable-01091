cmake_minimum_required(VERSION 3.16)
project(es_demo VERSION 1.0.0 LANGUAGES CXX)

# C++17 标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# 编译选项
if(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
    add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# 查找 libcurl
find_package(CURL REQUIRED)

# 头文件目录
include_directories(${CMAKE_SOURCE_DIR}/include)
include_directories(${CURL_INCLUDE_DIRS})

# 源文件
set(SOURCES
    src/main.cpp
    src/http_client.cpp
    src/es_client.cpp
)

# 可执行文件
add_executable(es_demo ${SOURCES})

# 链接库
target_link_libraries(es_demo PRIVATE ${CURL_LIBRARIES})

# 安装规则
install(TARGETS es_demo DESTINATION bin)

# 复制数据文件到构建目录
file(COPY ${CMAKE_SOURCE_DIR}/data DESTINATION ${CMAKE_BINARY_DIR})

# 输出配置信息
message(STATUS "C++ Standard: ${CMAKE_CXX_STANDARD}")
message(STATUS "CURL Include: ${CURL_INCLUDE_DIRS}")
message(STATUS "CURL Libraries: ${CURL_LIBRARIES}")
