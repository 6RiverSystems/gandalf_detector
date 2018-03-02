cmake_minimum_required(VERSION 2.8.3)
project(gandalf_detector)

ADD_DEFINITIONS(-std=c++0x)

## Find catkin macros and libraries
find_package(catkin REQUIRED COMPONENTS
  roscpp
  roslib
  sensor_msgs
  std_msgs
)

find_package(Eigen3 REQUIRED)

## System dependencies are found with CMake's conventions
Find_package(Boost REQUIRED COMPONENTS system)

## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/api/catkin/html/user_guide/setup_dot_py.html
# catkin_python_setup()

CATKIN_PACKAGE(INCLUDE_DIRS external/include
                            components/GDIFDetector/include
                            components/AdaBoostTreeClassifier/include
                            components/LaserBasedObjectDetection/include
               CATKIN_DEPENDS roscpp
                              roslib
                              sensor_msgs
                              std_msgs
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
# include_directories(include)
include_directories(
  include
  external/include
  components/GDIFDetector/include
  components/AdaBoostTreeClassifier/include
  components/LaserBasedObjectDetection/include
  ${catkin_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIR}
)

## Declare a cpp library
add_library(gandalf_detector
  components/LaserBasedObjectDetection/src/LaserRangeSegment.C
  components/LaserBasedObjectDetection/src/Segmentation.C
  components/AdaBoostTreeClassifier/src/AdaboostClassifier.C
  components/AdaBoostTreeClassifier/src/AdaboostClassifierNode.C
  components/GDIFDetector/src/GDIFeatures.C
  components/GDIFDetector/src/GDIFDetectorTree.C
)

## Declare a cpp executable
add_executable(gandalf_detector_node src/gandalf_detector_node.cpp)

## Add cmake target dependencies of the executable/library
## as an example, message headers may need to be generated before nodes
add_dependencies(gandalf_detector_node 
	gandalf_detector_generate_messages_cpp,
	opencv_ml, opencv_core
)
		
## Specify libraries to link a library or executable target against
target_link_libraries(gandalf_detector_node
  ${catkin_LIBRARIES}
  ${Boost_LIBRARIES}
  gandalf_detector
  opencv_ml
  opencv_core
)

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables and/or libraries for installation
# install(TARGETS gandalf_detector gandalf_detector_node
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
install(DIRECTORY launch/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
)

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_gandalf_detector.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
