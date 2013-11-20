################################################################
# Project GUACAMOLE_OCULUS
################################################################

PROJECT(GUACAMOLE_OCULUS CXX)

# version number
set(GUACAMOLE_OCULUS_MAJOR 0)
set(GUACAMOLE_OCULUS_MINOR 1)
set(GUACAMOLE_OCULUS_PATCH 0)
set(GUACAMOLE_OCULUS_VERSION ${GUACAMOLE_OCULUS_MAJOR}.${GUACAMOLE_OCULUS_MINOR}.${GUACAMOLE_OCULUS_PATCH})
set(GUACAMOLE_OCULUS_DESCRIPTION "GUACAMOLE_OCULUS - an astonishing virtual reality engine")
set(GUACAMOLE_OCULUS_HOMEPAGE "http://www.GUACAMOLE_OCULUS.org")
set(GUACAMOLE_OCULUS_EXENAME "GUACAMOLE_OCULUS")
set(GUACAMOLE_OCULUS_PACKAGENAME "GUACAMOLE_OCULUS")

# We require at least version 2.8.0
cmake_minimum_required(VERSION 2.8.0)

find_package(PkgConfig)

# Location where cmake first looks for modules.
list(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/modules)

################################################################
# Configure and find libraries
################################################################

pkg_check_modules(ASSIMP REQUIRED assimp)
pkg_check_modules(JSON REQUIRED jsoncpp)
pkg_check_modules(GL REQUIRED gl)
pkg_check_modules(GLEW REQUIRED glew)

include(find_guacamole)
include(find_schism)
include(find_boost)
include(find_ev)
include(find_ovr)
include(find_bullet)

set(LIBS
    ${SCHISM_LIBRARIES}
    ${BOOST_LIBRARIES}
    ${ASSIMP_LIBRARIES}
    ${JSON_LIBRARIES}
    ${GL_LIBRARIES}
    ${GLEW_LIBRARIES}
    ${EV_LIBRARIES}
    ${BULLET_LIBRARIES}
    ${GUACAMOLE_LIBRARIES}
    ${OVR_LIBRARIES}
    udev
    Xinerama
)
link_libraries(${LIBS})

set(LIB_PATHS
    ${ASSIMP_LIBRARY_DIRS}
    ${JSON_LIBRARY_DIRS}
    ${GL_LIBRARY_DIRS}
    ${GLEW_LIBRARY_DIRS}
    ${GUACAMOLE_LIBRARY_DIRS}
)
link_directories(${LIB_PATHS})

set(INCLUDE_PATHS
    ${CMAKE_CURRENT_SOURCE_DIR}/build
    ${CMAKE_CURRENT_SOURCE_DIR}/include
    ${SCHISM_INCLUDE_DIRS}
    ${BOOST_INCLUDE_DIRS}
    ${ASSIMP_INCLUDE_DIRS}
    ${JSON_INCLUDE_DIRS}
    ${GL_INCLUDE_DIRS}
    ${GLEW_INCLUDE_DIRS}
    ${EV_INCLUDE_DIRS}
    ${BULLET_INCLUDE_DIRS}
    ${GUACAMOLE_INCLUDE_DIRS}
    ${OVR_INCLUDE_DIRS}
)
include_directories(${INCLUDE_PATHS})

if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release")
endif()

set(CMAKE_CXX_FLAGS_RELEASE "-s -O4 --std=c++0x -fpermissive")
set(CMAKE_CXX_FLAGS_DEBUG "-g -Wall --std=c++0x -fpermissive")

################################################################
# Create libraries
################################################################

file(MAKE_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/lib)
set(LIBRARY_OUTPUT_PATH ${CMAKE_CURRENT_SOURCE_DIR}/lib)
add_subdirectory(src)

################################################################
# Create example
################################################################

add_subdirectory(example)

################################################################
# Summary
################################################################

message(STATUS "" )
message(STATUS "Summary:" )
message(STATUS " build type: ${CMAKE_BUILD_TYPE}" )
message(STATUS "" )
message(STATUS " schism:" )
message(STATUS "   library: ${SCHISM_LIBRARIES}" )
message(STATUS "   include: ${SCHISM_INCLUDE_DIRS}" )
message(STATUS "" )
message(STATUS " boost:" )
message(STATUS "   library: ${BOOST_LIBRARIES}" )
message(STATUS "   include: ${BOOST_INCLUDE_DIRS}" )
message(STATUS "" )
message(STATUS " assimp:" )
message(STATUS "   library: ${ASSIMP_LIBRARIES}" )
message(STATUS "   include: ${ASSIMP_INCLUDE_DIRS}" )
message(STATUS "" )
message(STATUS " gl:" )
message(STATUS "   library: ${GL_LIBRARIES}" )
message(STATUS "   include: ${GL_INCLUDE_DIRS}" )
message(STATUS "" )
message(STATUS " glew:" )
message(STATUS "   library: ${GLEW_LIBRARIES}" )
message(STATUS "   include: ${GLEW_INCLUDE_DIRS}" )
message(STATUS "" )
message(STATUS " ev:" )
message(STATUS "   library: ${EV_LIBRARIES}" )
message(STATUS "   include: ${EV_INCLUDE_DIRS}" )
message(STATUS "" )
message(STATUS " bullet:" )
message(STATUS "   library: ${BULLET_LIBRARIES}" )
message(STATUS "   include: ${BULLET_INCLUDE_DIRS}" )
message(STATUS "" )
message(STATUS " ovr:" )
message(STATUS "   library: ${OVR_LIBRARIES}" )
message(STATUS "   include: ${OVR_INCLUDE_DIRS}" )
message(STATUS "" )
message(STATUS " guacamole:" )
message(STATUS "   library: ${GUACAMOLE_LIBRARIES}" )
message(STATUS "   library path: ${GUACAMOLE_LIBRARY_DIRS}" )
message(STATUS "   include: ${GUACAMOLE_INCLUDE_DIRS}" )
message(STATUS "" )
