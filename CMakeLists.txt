cmake_minimum_required(VERSION 3.8)
project(lanefollowing)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(ament_cmake_python REQUIRED)
find_package(rclcpp REQUIRED)
find_package(cv_bridge REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(OpenCV REQUIRED)

ament_python_install_package(${PROJECT_NAME})

#Build
add_executable(lanefollowing src/lanefollowing.cpp)
target_include_directories(lanefollowing PUBLIC ${CMAKE_SOURCE_DIR}/include/${PROJECT_NAME})
ament_target_dependencies(lanefollowing rclcpp cv_bridge sensor_msgs geometry_msgs OpenCV)

add_executable(detector src/detector.cpp)
ament_target_dependencies(detector rclcpp sensor_msgs cv_bridge OpenCV)

install(
  DIRECTORY 
    
  DESTINATION share/${PROJECT_NAME}
)

install(
  PROGRAMS 
    src/detectorpy.py
  DESTINATION lib/${PROJECT_NAME}
)

install(TARGETS
  lanefollowing
  detector
  DESTINATION lib/${PROJECT_NAME})

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # uncomment the line when a copyright and license is not present in all source files
  #set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # uncomment the line when this package is not in a git repo
  #set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
