cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

rosbuild_init()

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()

# Eigen is not being found in the pcl includes, so we explicitly
# require it here
find_package(Eigen REQUIRED)
include_directories(${EIGEN_INCLUDE_DIRS})
include_directories(/usr/include/vtk-5.8)

#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
#rosbuild_add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})

rosbuild_add_executable(recognition_node src/recognition_node.cpp)
rosbuild_add_executable(recognition_service src/recognition_service.cpp)
rosbuild_add_executable(training_node src/generate_training_data.cpp)
rosbuild_add_executable(data_processing_node src/process_faro_data.cpp)
target_link_libraries(recognition_node ${PCL_LIBRARIES} libvtkCommon.so libvtkFiltering.so libvtkRendering.so libvtkIO.so libvtkGraphics.so)
target_link_libraries(recognition_service ${PCL_LIBRARIES} libvtkCommon.so libvtkFiltering.so libvtkRendering.so libvtkIO.so)
target_link_libraries(training_node ${PCL_LIBRARIES} libvtkCommon.so libvtkFiltering.so libvtkRendering.so libvtkIO.so libvtkGraphics.so)

