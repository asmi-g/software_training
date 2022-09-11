cmake_minimum_required(VERSION 3.5)
project(software_training)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(turtlesim REQUIRED)
find_package(rclcpp_components REQUIRED)
find_package(std_msgs REQUIRED)
find_package(rcutils REQUIRED)
find_package(rcl REQUIRED)
find_package(rclcpp_action REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(builtin_interfaces REQUIRED)
find_package(rosidl_default_generators REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)


#include the 'include' directory
include_directories(include)

#add spawn_turtle_nodelets as plugin
add_library(turtle_spawn SHARED
	src/spawn_turtle_nodelet.cpp)
target_compile_definitions(turtle_spawn
	PRIVATE "SOFTWARE_TRAINING_DLL")
ament_target_dependencies(turtle_spawn
	"rclcpp"
	"rclcpp_components"
	"turtlesim"
	"std_msgs"
	"geometry_msgs")
rclcpp_components_register_node(turtle_spawn PLUGIN "composition::spawn_turtle_nodelet" EXECUTABLE turtle_spawner)
# set(node_plugins PLUGIN "${node_plugins}composition::spawn_turtle_nodelet;$<TARGET_FILE:turtle_spawn\n>" EXECUTABLE turtle_spawner)





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



#tell where to put binaries
install(TARGETS
	turtle_spawn

	ARCHIVE DESTINATION lib
	LIBRARY DESTINATION lib 
	RUNTIME DESTINATION bin)


#install launch file
#install(DIRECTORY launch
#	DESTINATION share/${PROJECT_NAME}
#	)

#install configuration file (yaml files) for param server
#install(DIRECTORY
#	config
#	DESTINATION share/${PROJECT_NAME}
#	)


ament_package()
