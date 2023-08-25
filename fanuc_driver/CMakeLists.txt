cmake_minimum_required(VERSION 3.5)

project(fanuc_driver)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# let industrial_robot_client pull in all other dependencies
find_package(ament_cmake REQUIRED )
find_package(rclcpp REQUIRED)
find_package(industrial_robot_client REQUIRED)

ament_export_dependencies(industrial_robot_client)

include_directories(include)


# non-swapping Fanuc specific robot state node
add_executable(${PROJECT_NAME}_robot_state
  src/fanuc_robot_state_node.cpp
  src/fanuc_utils.cpp)
ament_target_dependencies(${PROJECT_NAME}_robot_state
  industrial_robot_client
  simple_message
  rclcpp)
set_target_properties(${PROJECT_NAME}_robot_state
  PROPERTIES OUTPUT_NAME robot_state
  PREFIX "")
target_compile_definitions(
  ${PROJECT_NAME}_robot_state
  PRIVATE
  ${industrial_robot_client_DEFINITIONS})


# byte-swapping Fanuc specific robot state node
add_executable(${PROJECT_NAME}_robot_state_bswap
  src/fanuc_robot_state_node.cpp
  src/fanuc_utils.cpp)
ament_target_dependencies(${PROJECT_NAME}_robot_state_bswap
  industrial_robot_client_bswap
  simple_message_bswap
  rclcpp)
set_target_properties(${PROJECT_NAME}_robot_state_bswap
  PROPERTIES OUTPUT_NAME robot_state_bswap
  PREFIX "")
target_compile_definitions(
  ${PROJECT_NAME}_robot_state_bswap
  PRIVATE
  ${industrial_robot_client_bswap_DEFINITIONS})


# non-swapping Fanuc specific motion streaming interface
add_executable(${PROJECT_NAME}_motion_streaming_interface
  src/fanuc_joint_streamer_node.cpp
  src/fanuc_utils.cpp)
ament_target_dependencies(${PROJECT_NAME}_motion_streaming_interface
  industrial_robot_client
  simple_message
  rclcpp)
set_target_properties(${PROJECT_NAME}_motion_streaming_interface
  PROPERTIES OUTPUT_NAME motion_streaming_interface
  PREFIX "")
target_compile_definitions(
  ${PROJECT_NAME}_motion_streaming_interface
  PRIVATE
  ${industrial_robot_client_DEFINITIONS})


# byte-swapping Fanuc specific motion streaming interface
add_executable(${PROJECT_NAME}_motion_streaming_interface_bswap
  src/fanuc_joint_streamer_node.cpp
  src/fanuc_utils.cpp)
ament_target_dependencies(${PROJECT_NAME}_motion_streaming_interface_bswap
  industrial_robot_client_bswap
  simple_message_bswap
  rclcpp)
set_target_properties(${PROJECT_NAME}_motion_streaming_interface_bswap
  PROPERTIES OUTPUT_NAME motion_streaming_interface_bswap
  PREFIX "")
target_compile_definitions(
  ${PROJECT_NAME}_motion_streaming_interface_bswap
  PRIVATE
  ${industrial_robot_client_bswap_DEFINITIONS})

# if (CATKIN_ENABLE_TESTING)
#   find_package(roslaunch REQUIRED)

#   set(rl_test_deps
#     ${PROJECT_NAME}_robot_state
#     ${PROJECT_NAME}_robot_state_bswap
#     ${PROJECT_NAME}_motion_streaming_interface
#     ${PROJECT_NAME}_motion_streaming_interface_bswap)

#   if (TARGET joint_trajectory_action)
#     # not nice, but industrial_robot_client doesn't prefix its targets
#     list(APPEND rl_test_deps joint_trajectory_action)
#   endif()

#   roslaunch_add_file_check(tests/roslaunch_test.xml
#     DEPENDENCIES ${rl_test_deps})
# endif()


install(TARGETS
  ${PROJECT_NAME}_motion_streaming_interface
  ${PROJECT_NAME}_motion_streaming_interface_bswap

  ${PROJECT_NAME}_robot_state
  ${PROJECT_NAME}_robot_state_bswap

  DESTINATION lib/${PROJECT_NAME}
)

install(DIRECTORY karel launch tpe
  DESTINATION share/${PROJECT_NAME})

install(FILES readme.md DESTINATION share/${PROJECT_NAME})
