cmake_minimum_required(VERSION 3.0.2)
project(costmap_converter)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++14)


find_package(catkin REQUIRED COMPONENTS
  dynamic_reconfigure roscpp costmap_2d 
  grid_map_core grid_map_ros grid_map_msgs rqt_gui rqt_gui_py
  rviz pcl_ros
)

set(EIGEN3_INCLUDE_DIR "/home/eureka/Softwares/eigen-3.3.4/eigen_install/include/eigen3")
include_directories(${EIGEN3_INCLUDE_DIR})

catkin_package(

)

###########
## Build ##
###########

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

## Declare a C++ library
add_library(pointMap_layer
  src/pointMap_layer.cpp
)
target_link_libraries(pointMap_layer ${catkin_LIBRARIES})
target_compile_options(pointMap_layer PRIVATE -std=c++14)


add_library(elevationMap_layer 
  src/elevationMap_layer.cpp)
target_link_libraries(elevationMap_layer ${catkin_LIBRARIES})
target_compile_options(elevationMap_layer PRIVATE -std=c++14)

#############
## Install ##
#############
install(FILES
  costmap_plugins.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

install(
  TARGETS
  pointMap_layer  
  elevationMap_layer  
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)
