cmake_minimum_required(VERSION 3.10.2)
project(moveit_planners_ompl)

find_package(Boost REQUIRED system filesystem date_time thread serialization)
find_package(moveit_common REQUIRED)
find_package(moveit_core REQUIRED)
find_package(moveit_ros_planning REQUIRED)
find_package(rclcpp REQUIRED)
find_package(pluginlib REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(ompl REQUIRED)

moveit_package()

include_directories(ompl_interface/include)
include_directories(SYSTEM
                    ${Boost_INCLUDE_DIRS}
                    ${OMPL_INCLUDE_DIRS})

add_subdirectory(ompl_interface)

ament_export_include_directories(include)
ament_export_libraries(moveit_ompl_interface moveit_ompl_planner_plugin)
ament_export_dependencies(moveit_core ompl)

pluginlib_export_plugin_description_file(moveit_core ompl_interface_plugin_description.xml)

ament_package()
