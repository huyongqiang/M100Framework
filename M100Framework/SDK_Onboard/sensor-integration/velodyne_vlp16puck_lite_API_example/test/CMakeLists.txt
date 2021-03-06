cmake_minimum_required(VERSION 2.8)
project(VLP16LiDAR_Test)

if (COVERAGE)
  set(GCC_COVERAGE_COMPILE_FLAGS "-fprofile-arcs -ftest-coverage -fPIC")
  set(GCC_COVERAGE_LINK_FLAGS    "-lgcov")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${GCC_COVERAGE_COMPILE_FLAGS} ${COMMON_CXX_FLAGS} ${CMAKE_CXX_FLAGS_DEBUG}")
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${GCC_COVERAGE_LINK_FLAGS}")
endif ()

# Set compiler flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -pthread -g -O0")

include_directories(../vlp16lidar-driver/inc)

# Test with GTest framework
if(CMAKE_SYSTEM_NAME MATCHES Linux)
  include_directories(test ${Boost_INCLUDE_DIRS} ${LIBLAS_INCLUDE_DIR})
  FILE(GLOB TEST_SOURCE_FILES *.cpp)
  add_executable(${PROJECT_NAME} ${TEST_SOURCE_FILES})
  add_dependencies(${PROJECT_NAME} googletest) 
  target_link_libraries(${PROJECT_NAME}
    boost_thread
    boost_system
    boost_iostreams
    boost_filesystem
    vlp16lidar-driver
    ${binary_dir}/googlemock/gtest/libgtest.a
    ${binary_dir}/googlemock/gtest/libgtest_main.a
    pthread)
  add_test(${EXECUTABLE_OUTPUT_PATH}/${PROJECT_NAME} ${EXECUTABLE_OUTPUT_PATH}/${PROJECT_NAME})
endif()

