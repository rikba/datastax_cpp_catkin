cmake_minimum_required(VERSION 2.8.3)
project(datastax_cpp_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(datastax_cpp_src
  GIT_REPOSITORY git@github.com:datastax/cpp-driver.git
  GIT_TAG 2.5.0
  UPDATE_COMMAND ""
  CMAKE_ARGS
    -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX}
    -DCMAKE_INSTALL_LIBDIR:PATH=${CATKIN_DEVEL_PREFIX}/lib/
    -DCMAKE_BUILD_TYPE=Release
    -DBUILD_SHARED_LIBS=ON
    -DCASS_BUILD_SHARED=ON
)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} datastax_cpp_src)
target_link_libraries(${PROJECT_NAME} ${CATKIN_DEVEL_PREFIX}/lib/libcassandra${CMAKE_SHARED_LIBRARY_SUFFIX})

cs_install()

cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
          CFG_EXTRAS datastax_cpp-extras.cmake)
