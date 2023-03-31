PROJECT(rf2o_laser_odometry)

CMAKE_MINIMUM_REQUIRED(VERSION 3.3)
# Require C++17

if(${CMAKE_VERSION} VERSION_LESS "3.8.0")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")
else()
	set(CMAKE_CXX_STANDARD 17)
endif()

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS  
  roscpp
  rospy
  nav_msgs
  sensor_msgs
  std_msgs
  tf
  cmake_modules
)

## System dependencies are found with CMake's conventions
find_package(Boost REQUIRED COMPONENTS system)
find_package(Eigen3 REQUIRED)

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
 INCLUDE_DIRS include ${EIGEN3_INCLUDE_DIRS}
 LIBRARIES ${PROJECT_NAME}
 CATKIN_DEPENDS nav_msgs roscpp sensor_msgs std_msgs tf
 DEPENDS #Eigen3
)

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(include)

include_directories(
  SYSTEM
  ${catkin_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIRS}
)

## Declare a cpp library
add_library(${PROJECT_NAME} src/CLaserOdometry2D.cpp)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})

## Declare a cpp executable
add_executable(rf2o_laser_odometry_node src/CLaserOdometry2DNode.cpp)
target_link_libraries(rf2o_laser_odometry_node ${PROJECT_NAME} ${catkin_LIBRARIES})

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executables for installation
## See http://docs.ros.org/melodic/api/catkin/html/howto/format1/building_executables.html
install(TARGETS
  rf2o_laser_odometry_node
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

## Mark libraries for installation
## See http://docs.ros.org/melodic/api/catkin/html/howto/format1/building_libraries.html
install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
)

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
install(DIRECTORY
launch
DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
