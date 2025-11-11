cmake_minimum_required(VERSION 3.0)
project(unitree_lidar_sdk)

set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS "-std=c++17")
set(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g2 -ggdb")
set(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall -DNDEBUG") 

include_directories(include)

# 自动选择平台架构
if(CMAKE_SYSTEM_PROCESSOR MATCHES "aarch64")
    set(LIB_ARCH_DIR "aarch64")
else()
    set(LIB_ARCH_DIR "x86_64")
endif()

link_directories(${PROJECT_SOURCE_DIR}/lib/${LIB_ARCH_DIR})

SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib/${LIB_ARCH_DIR})

# === 可执行文件 ===
add_executable(example_lidar_udp examples/example_lidar_udp.cpp)
target_link_libraries(example_lidar_udp libunilidar_sdk2.a)

add_executable(example_lidar_serial examples/example_lidar_serial.cpp)
target_link_libraries(example_lidar_serial libunilidar_sdk2.a)

add_executable(set_ip_address examples/set_ip_address.cpp)
target_link_libraries(set_ip_address libunilidar_sdk2.a)

add_executable(set_to_serial_mode examples/set_to_serial_mode.cpp)
target_link_libraries(set_to_serial_mode libunilidar_sdk2.a)

add_executable(set_to_udp_mode examples/set_to_udp_mode.cpp)
target_link_libraries(set_to_udp_mode libunilidar_sdk2.a)

# === 安装规则 ===
install(TARGETS
    example_lidar_udp
    example_lidar_serial
    set_ip_address
    set_to_serial_mode
    set_to_udp_mode
    RUNTIME DESTINATION bin
)

install(FILES
    lib/${LIB_ARCH_DIR}/libunilidar_sdk2.a
    DESTINATION lib
)

install(DIRECTORY include/
    DESTINATION include
)

install(FILES README.md
    DESTINATION share
)

