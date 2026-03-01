cmake_minimum_required(VERSION 3.8)
project(microstrain_comm)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(sensor_msgs REQUIRED)

LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_modules)

find_package(GLIB2 REQUIRED)
find_package(GTHREAD2 REQUIRED)

include_directories(
  include
  ${GLIB2_MAIN_INCLUDE_DIR}
  ${GLIB2_INTERNAL_INCLUDE_DIR}
  ${GTHREAD2_MAIN_INCLUDE_DIR}
)

add_executable(${PROJECT_NAME}_node
  src/micro/microstrain_comm.cpp
  src/libbot/ringbuf.c
  src/libbot/rotations.c
  src/libbot/serial.c
  src/libbot/timestamp.c
)

ament_target_dependencies(${PROJECT_NAME}_node
  rclcpp
  sensor_msgs
)

target_link_libraries(${PROJECT_NAME}_node
  ${GLIB2_LIBRARY}
  ${GTHREAD2_LIBRARY}
)

# Install rules
install(TARGETS ${PROJECT_NAME}_node
  DESTINATION lib/${PROJECT_NAME}
)

install(DIRECTORY launch
  DESTINATION share/${PROJECT_NAME}
)

ament_package()