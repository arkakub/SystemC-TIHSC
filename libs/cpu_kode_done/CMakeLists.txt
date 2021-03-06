cmake_minimum_required(VERSION 2.8)
project(DPC13exam)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/../cmake)
set(CMAKE_INSTALL_PREFIX ${CMAKE_SOURCE_DIR}/..)

if (WIN32)
ADD_DEFINITIONS(-DWIN32 -D_WIN32 -D_WINDOWS)
ADD_DEFINITIONS(-DUNICODE -D_UNICODE)
ADD_DEFINITIONS(-D_CRT_SECURE_NO_WARNINGS)
SET (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /EHsc")
SET (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W3")
endif (WIN32)

find_package(CUDA 4.1 REQUIRED)

# Compile kernels for shader models 1.0 and 2.0 as default
set(CUDA_NVCC_FLAGS "-gencode=arch=compute_10,code=\"sm_10,compute_10\" -gencode=arch=compute_20,code=\"sm_20,compute_20\" -gencode=arch=compute_30,code=\"sm_30,compute_30\" -gencode=arch=compute_35,code=\"sm_35,compute_35\"")

find_package(GLEW)

include_directories(
  ${CMAKE_SOURCE_DIR}/../include
  ${CMAKE_SOURCE_DIR}/utils
  )

link_directories(
  ${CMAKE_SOURCE_DIR}/../lib
  )

cuda_add_executable(kode main.cpp filtering.cu debugging.c k_means.cu)

target_link_libraries(kode 
  common
  cuda_utils
  ${Boost_LIBRARIES}
  ${CUDA_LIBRARIES}
  ${CUDA_CUFFT_LIBRARIES}
  )

install(TARGETS kode DESTINATION bin)
