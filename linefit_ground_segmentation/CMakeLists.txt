cmake_minimum_required(VERSION 3.10)
project(linefit_ground_segmentation)

find_package(PCL REQUIRED)
find_package(Eigen3 REQUIRED)

# Specify include directories
include_directories(
    include
    ${EIGEN3_INCLUDE_DIR}
    ${PCL_INCLUDE_DIRS}
)

# Define the library
add_library(${PROJECT_NAME} SHARED
    src/ground_segmentation.cc
    src/segment.cc
    src/bin.cc
    src/viewer.cc
)

# Link libraries
target_link_libraries(${PROJECT_NAME} ${PCL_LIBRARIES})

# Specify the C++ standard
target_compile_features(${PROJECT_NAME} PUBLIC cxx_std_14)

# Add PCL definitions
add_definitions(${PCL_DEFINITIONS})

# Install rules for the library
install(TARGETS ${PROJECT_NAME}
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

# Install header files
install(DIRECTORY include/
    DESTINATION include
    FILES_MATCHING PATTERN "*.h"
    PATTERN ".svn" EXCLUDE
)

# QtCreator compatibility: add headers to a custom target
FILE(GLOB_RECURSE LibFiles "include/*")
add_custom_target(headers SOURCES ${LibFiles})
