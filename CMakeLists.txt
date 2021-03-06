project(cairo)
cmake_minimum_required(VERSION 2.8)

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

find_package(Pixman REQUIRED)
find_package(PNG REQUIRED)
find_package(ZLIB REQUIRED)
find_package(Freetype)
find_package(Threads)
find_package(Fontconfig)


include(Configure_config.cmake)
include(Configure_features.cmake)
include_directories(${CMAKE_BINARY_DIR})

if(CAIRO_HAS_PTHREAD)
	list(APPEND CAIRO_LIBS -lpthread)
endif()

if (WIN32)
    set(CAIRO_LIBS gdi32 msimg32 user32 winmm)
endif()

include_directories(${PIXMAN_INCLUDE_DIR} ${PNG_INCLUDE_DIR} ${ZLIB_INCLUDE_DIR})
list(APPEND CAIRO_LIBS  ${PIXMAN_LIBRARY} ${PNG_LIBRARY} ${ZLIB_LIBRARY})

if(FONTCONFIG_FOUND)
    list(APPEND CAIRO_LIBS ${FONTCONFIG_LIBRARY})
endif()

if(FREETYPE_FOUND)
    find_package(BZip2)
    include_directories(${FREETYPE_INCLUDE_DIRS})
    include_directories(${BZIP2_INCLUDE_DIR})
    list(APPEND CAIRO_LIBS ${FREETYPE_LIBRARIES} ${BZIP2_LIBRARIES})
endif()

include_directories(src)
add_subdirectory(src)