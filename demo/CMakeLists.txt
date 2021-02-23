# 3.10 introduces C++ 17 support
cmake_minimum_required (VERSION 3.10)
project (mpt_demos)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(Eigen3 3.3 REQUIRED NO_MODULE)
find_package(OpenMP REQUIRED)
#find_package(FCL REQUIRED)
find_package(Assimp REQUIRED)

find_package(PNG REQUIRED)

######### Find CCD the hard way
find_package(PkgConfig)
if (PKGCONFIG_FOUND)
    pkg_check_modules(FCL fcl>=0.6)
    pkg_check_modules(CCD ccd>=2.0)
else()
    find_package(FCL REQUIRED)
    find_package(CCD CONFIG)
endif()
#########

link_directories(${FCL_LIBRARY_DIRS} ${ASSIMP_LIBRARY_DIRS} ${CCD_LIBRARY_DIRS})
include_directories(../src ../../nigh/src ${FCL_INCLUDE_DIRS} ${CCD_LIBRARY_DIRS} ${ASSIMP_INCLUDE_DIRS} ${PNG_INCLUDE_DIR})

if (OPENMP_FOUND)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

set(trgt holonomic_2d_point_planning)
add_executable(${trgt} holonomic_2d_point_planning.cpp)
target_link_libraries(${trgt} Eigen3::Eigen)

set(trgt png_2d_planning)
add_executable(${trgt} png_2d_planning.cpp)
target_link_libraries(${trgt} Eigen3::Eigen ${PNG_LIBRARY})

set(trgt link_maipulator_planning)
add_executable(${trgt} link_manipulator_planning.cpp)
target_link_libraries(${trgt} Eigen3::Eigen)
