cmake_minimum_required(VERSION 3.15)

option(MSDFGEN_CORE_ONLY "Only build the core library with no dependencies" ON)

project(msdfgen VERSION 1.9 LANGUAGES CXX)


add_subdirectory(freetype/)
include_directories(freetype/include)

file(GLOB_RECURSE MSDFGEN_CORE_HEADERS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} "core/*.h" "core/*.hpp")
file(GLOB_RECURSE MSDFGEN_CORE_SOURCES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} "core/*.cpp")
file(GLOB_RECURSE MSDFGEN_EXT_HEADERS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} "ext/*.h" "ext/*.hpp")
file(GLOB_RECURSE MSDFGEN_EXT_SOURCES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} "ext/*.cpp" "lib/*.cpp")

# Core library
add_library(msdfgen-core 
"${CMAKE_CURRENT_SOURCE_DIR}/msdfgen.h" ${MSDFGEN_CORE_HEADERS} ${MSDFGEN_CORE_SOURCES} 
"${CMAKE_CURRENT_SOURCE_DIR}/msdfgen-ext.h" ${MSDFGEN_EXT_HEADERS} ${MSDFGEN_EXT_SOURCES} 
${CMAKE_CURRENT_SOURCE_DIR}/lib/lodepng.cpp ${CMAKE_CURRENT_SOURCE_DIR}/lib/tinyxml2.cpp 
${CMAKE_CURRENT_SOURCE_DIR}/include/lodepng.h ${CMAKE_CURRENT_SOURCE_DIR}/include/tinyxml2.h )

target_link_libraries(msdfgen-core PUBLIC freetype)
add_library(msdfgen::msdfgen-core ALIAS msdfgen-core)

set_target_properties(msdfgen-core PROPERTIES PUBLIC_HEADER "${MSDFGEN_CORE_HEADERS}")
#set_property(TARGET msdfgen-core PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
target_include_directories(msdfgen-core INTERFACE
    $<INSTALL_INTERFACE:include/msdfgen>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/>
)




