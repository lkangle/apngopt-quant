cmake_minimum_required(VERSION 3.0)
project(apngopt)

set(CMAKE_CXX_STANDARD 17)

if (CMAKE_COMPILER_IS_GNUCC)
    set(CMAKE_C_FLAGS "-lm -std=c99 ${CMAKE_C_FLAGS}")
endif ()

add_definitions(-DPNG_ARM_NEON_OPT=0)

include_directories(${CMAKE_SOURCE_DIR})
include_directories(${CMAKE_SOURCE_DIR}/7z)
include_directories(${CMAKE_SOURCE_DIR}/zlib)
include_directories(${CMAKE_SOURCE_DIR}/libpng)
include_directories(${CMAKE_SOURCE_DIR}/zopfli)
include_directories(${CMAKE_SOURCE_DIR}/libimagequant)

add_subdirectory(libimagequant)

aux_source_directory(zlib ZLIB_SOURCE)
aux_source_directory(libpng PNG_SOURCE)
aux_source_directory(7z 7Z_SOURCE)
aux_source_directory(zopfli ZOPFLI_SOURCE)

add_library(zlib SHARED ${ZLIB_SOURCE})
add_library(libpng SHARED ${PNG_SOURCE} ${ZLIB_SOURCE})
add_library(7z SHARED ${7Z_SOURCE} ${ZLIB_SOURCE})
add_library(zopfli SHARED ${ZOPFLI_SOURCE})

add_executable(${PROJECT_NAME} apngopt.cpp)
target_link_libraries(${PROJECT_NAME} imagequant 7z zopfli libpng zlib)
