cmake_minimum_required(VERSION 3.0)

project(imagequant C)

if(${CMAKE_SYSTEM_PROCESSOR} STREQUAL ARM64)
  option(BUILD_WITH_SSE "Use SSE" OFF)
else()
  option(BUILD_WITH_SSE "Use SSE" ON)
endif()

if(BUILD_WITH_SSE)
  add_definitions(-DUSE_SSE=0)
endif()

find_package(OpenMP)
if(OPENMP_FOUND)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

if(CMAKE_COMPILER_IS_GNUCC)
  add_compile_options("-std=c99")
endif()

add_library(${PROJECT_NAME} STATIC
  libimagequant.c
  blur.c
  mediancut.c
  mempool.c
  nearest.c
  pam.c
  kmeans.c
)
