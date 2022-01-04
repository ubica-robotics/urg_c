cmake_minimum_required(VERSION 3.5)
project(urg_c)

find_package(ament_cmake REQUIRED)

if(WIN32)
  message(WARNING "${PROJECT_NAME} is currently not supported on Windows")
  ament_package()
  return()
endif()

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# Set library locations to better handle if upstream locations change
set(LIBRARY_DIR ${CMAKE_CURRENT_SOURCE_DIR}/urg_library)
set(LIBRARY_INCLUDE_DIR ${LIBRARY_DIR}/include)
set(LIBRARY_SRC_DIR ${LIBRARY_DIR}/src)

###################################################
## Declare things to be passed to other projects ##
###################################################

###########
## Build ##
###########

## Declare libraries
add_library(urg_c SHARED
  ${LIBRARY_SRC_DIR}/urg_sensor.c
  ${LIBRARY_SRC_DIR}/urg_utils.c
  ${LIBRARY_SRC_DIR}/urg_debug.c
  ${LIBRARY_SRC_DIR}/urg_connection.c
  ${LIBRARY_SRC_DIR}/urg_ring_buffer.c
  ${LIBRARY_SRC_DIR}/urg_serial.c
  ${LIBRARY_SRC_DIR}/urg_serial_utils.c
  ${LIBRARY_SRC_DIR}/urg_tcpclient.c
  ${LIBRARY_SRC_DIR}/urg_time.c
)
target_include_directories(urg_c
  PUBLIC
  $<BUILD_INTERFACE:${LIBRARY_INCLUDE_DIR}>
  $<INSTALL_INTERFACE:include>
)
if(CMAKE_COMPILER_IS_GNUCXX)
  target_link_libraries(urg_c -lrt -lm)
endif(CMAKE_COMPILER_IS_GNUCXX)



#############
## Install ##
#############

## Mark executables and/or libraries for installation
install(TARGETS urg_c
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

## Mark cpp header files for installation
install(DIRECTORY ${LIBRARY_INCLUDE_DIR}/
  DESTINATION include
)

ament_export_include_directories(include)
ament_export_libraries(urg_c)
ament_package()
