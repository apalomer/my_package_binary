cmake_minimum_required(VERSION 3.8)
project(my_package_binary)

# Set default c++ version
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Set default c++ flags
if(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# Get the default GNU install directories
include(GNUInstallDirs)

find_package(rclcpp REQUIRED)
find_package(my_package_library REQUIRED)
ament_export_dependencies(rclcpp my_package_library)
message(
  STATUS "my_package_library_INCLUDE_DIRS: ${my_package_library_INCLUDE_DIRS}")
include_directories(${my_package_library_INCLUDE_DIRS})

add_executable(talker src/node.cpp)
ament_target_dependencies(talker rclcpp my_package_library)
install(TARGETS talker
        RUNTIME DESTINATION ${CMAKE_INSTALL_LIBDIR}/${PROJECT_NAME})

ament_package()
