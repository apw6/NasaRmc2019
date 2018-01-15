# If you're having trouble getting your things to build, visit the ROS
# tutorials at <http://wiki.ros.org/ROS/Tutorials> and also read the ROS
# CMake documentation at <http://wiki.ros.org/catkin/CMakeLists.txt>.

cmake_minimum_required(VERSION 2.8.3)
project(tfr_control)

add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  geometry_msgs
  tfr_msgs
  tfr_utilities
  hardware_interface
  controller_manager
  joint_state_controller
  effort_controllers
  joint_trajectory_controller
)

find_package(GTest REQUIRED)

# These are all for exporting to dependent packages/projects.
# Uncomment each if the dependent project requires it
catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES tfr_control
#  CATKIN_DEPENDS roscpp std_msgs
#  DEPENDS system_lib
)

# Specify additional locations of header files
# Your package locations should be listed before other locations
include_directories(
  include/${PROJECT_NAME}
  ${catkin_INCLUDE_DIRS}
  ${GTEST_INCLUDE_DIRS}
)

# Create executable process/node and link it
add_executable(proto_driver_station src/proto_driver_station.cpp)
target_link_libraries(proto_driver_station ${catkin_LIBRARIES} sfml-system
                        sfml-window sfml-graphics)

add_executable(controller_launcher src/controller_launcher.cpp
                                   src/controller.cpp)
target_link_libraries(controller_launcher ${catkin_LIBRARIES})

# This call is sometimes needed and sometimes not and I'm not really clear why
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread")

# Add gtest based cpp test target and link libraries
catkin_add_gtest(${PROJECT_NAME}-test test/test_tfr_control.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test)
# endif()