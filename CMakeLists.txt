
##################### Settings #######################
cmake_minimum_required(VERSION 3.16.0)

project(libtess2 C)

file(
    GLOB_RECURSE
    SOURCES

    # Sources
    ${CMAKE_CURRENT_LIST_DIR}/Source/*.c
)

add_library(libtess2 STATIC ${SOURCES})
add_library(libtess2::libtess2 ALIAS libtess2)

target_include_directories(
    libtess2

    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Include>
        $<INSTALL_INTERFACE:include>
)

set_target_properties(
    libtess2

    PROPERTIES
        POSITION_INDEPENDENT_CODE ON
)

if (MSVC)
    target_compile_options(libtess2 PRIVATE /W0)
else()
    target_compile_options(libtess2 PRIVATE -w)
endif()

install(
    TARGETS
        libtess2
    EXPORT
        libtess2Targets
    ARCHIVE
        DESTINATION lib
)

install(
    DIRECTORY
        Include/
    DESTINATION
        include
)

install(
    EXPORT
        libtess2Targets
    FILE
        libtess2Targets.cmake
    NAMESPACE
        libtess2::
    DESTINATION
        lib/cmake/libtess2
)