cmake_minimum_required(VERSION 2.8.3)
project(beaglebone_io_ros)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
)
find_package(PythonLibs REQUIRED)

catkin_package(
   INCLUDE_DIRS source
   LIBRARIES beaglebone_io_ros beaglebone_io_ros_python
   CATKIN_DEPENDS roscpp rospy
)

###########
## Build ##
###########

include_directories(
  ${catkin_INCLUDE_DIRS}
  ${PYTHON_INCLUDE_DIRS}
)

add_library(beaglebone_io_ros
  source/c_adc.c
  source/c_pwm.c
  source/c_uart.c
  source/common.c
  source/constants.c
  source/event_gpio.c
  source/spimodule.c
)

add_library(beaglebone_io_ros_python
  source/py_adc.c
  source/py_gpio.c
  source/py_pwm.c
  source/py_uart.c
)
target_link_libraries(beaglebone_io_ros_python
  beaglebone_io_ros
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

# Mark executables and/or libraries for installation
install(TARGETS beaglebone_io_ros beaglebone_io_ros_python
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

## Mark cpp header files for installation
install(DIRECTORY source
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
)

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_beaglebone_io_ros.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
