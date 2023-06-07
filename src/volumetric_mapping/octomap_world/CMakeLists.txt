cmake_minimum_required(VERSION 2.8.3)
project(octomap_world)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(CMAKE_BUILD_TYPE Release)

find_package(catkin_simple REQUIRED)
catkin_simple(ALL_DEPS_REQUIRED)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  "${PROJECT_SOURCE_DIR}/include"
  or-tools/include
)

link_directories(
  or-tools/lib
)

# ############
# LIBRARIES #
# ############
cs_add_library(${PROJECT_NAME}
  src/octomap_world.cc
  src/octomap_manager.cc
)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES} ${CMAKE_CURRENT_SOURCE_DIR}/or-tools/lib/libglog.so ${PROJECT_SOURCE_DIR}/or-tools/lib/libgflags.so)

# ###########
# BINARIES #
# ###########
cs_add_executable(octomap_manager
  src/octomap_manager_node.cc
)
target_link_libraries(octomap_manager ${PROJECT_NAME} ${catkin_LIBRARIES} ${CMAKE_CURRENT_SOURCE_DIR}/or-tools/lib/libglog.so ${PROJECT_SOURCE_DIR}/or-tools/lib/libgflags.so)

# #########
# EXPORT #
# #########
cs_install()
cs_export()
