cmake_minimum_required(VERSION 3.10.2)
project(moveit_ros_robot_interaction)

find_package(Boost REQUIRED thread)
find_package(moveit_common REQUIRED)
find_package(moveit_ros_planning REQUIRED)
find_package(interactive_markers REQUIRED)
find_package(tf2 REQUIRED)
find_package(tf2_eigen REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(rclcpp REQUIRED)

moveit_package()

set(MOVEIT_LIB_NAME moveit_robot_interaction)

include_directories(include)

add_library(${MOVEIT_LIB_NAME} SHARED
  src/interactive_marker_helpers.cpp
  src/kinematic_options.cpp
  src/kinematic_options_map.cpp
  src/locked_robot_state.cpp
  src/interaction_handler.cpp
  src/robot_interaction.cpp
)
set_target_properties(${MOVEIT_LIB_NAME} PROPERTIES VERSION "${${PROJECT_NAME}_VERSION}")

# Causes the visibility macros to use dllexport rather than dllimport,
# which is appropriate when building the dll but not consuming it.
target_compile_definitions(${MOVEIT_LIB_NAME}
  PRIVATE "ROBOT_INTERACTION_BUILDING_LIBRARY")

ament_target_dependencies(${MOVEIT_LIB_NAME}
  Boost
  moveit_ros_planning
  interactive_markers
  tf2_geometry_msgs
)

if(BUILD_TESTING)
  find_package(ament_cmake_gtest REQUIRED)
  ament_add_gtest(locked_robot_state_test test/locked_robot_state_test.cpp)
  target_link_libraries(locked_robot_state_test ${MOVEIT_LIB_NAME})
  ament_target_dependencies(locked_robot_state_test
    Boost)
endif()

install(TARGETS ${MOVEIT_LIB_NAME}
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin)

install(DIRECTORY include/ DESTINATION include)

install(DIRECTORY resources DESTINATION share/${PROJECT_NAME})

ament_export_include_directories(include)
ament_export_libraries(${MOVEIT_LIB_NAME})
ament_package()
