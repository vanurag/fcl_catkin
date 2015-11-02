cmake_minimum_required(VERSION 2.8.3)
project(fcl_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/fcl/)
ExternalProject_Add(fcl_src
  GIT_REPOSITORY https://github.com/vanurag/fcl/
  UPDATE_COMMAND ""
  CMAKE_ARGS -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX} -DBUILD_SHARED_LIBS:BOOL=true
)


cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} fcl_src)
target_link_libraries(${PROJECT_NAME} 
  ${CATKIN_DEVEL_PREFIX}/lib/libfcl${CMAKE_SHARED_LIBRARY_SUFFIX})


cs_install()

cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include/fcl/
  CFG_EXTRAS fcl-extras.cmake)
