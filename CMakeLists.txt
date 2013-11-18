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
include(find_compiler)

IF (UNIX)
	pkg_check_modules(ASSIMP REQUIRED assimp)
	pkg_check_modules(JSON REQUIRED jsoncpp)
	pkg_check_modules(GL REQUIRED gl)
	pkg_check_modules(GLEW REQUIRED glew)
ENDIF (UNIX)

include(find_guacamole)
include(find_schism)
include(find_assimp)
include(find_boost)
include(find_json)
include(find_ovr)
include(find_bullet)

set(LIBS
    ${SCHISM_LIBRARIES}
    ${BOOST_LIBRARIES}
    ${ASSIMP_LIBRARIES}
    ${JSON_LIBRARIES}
    ${GL_LIBRARIES}
    ${GLEW_LIBRARIES}
    ${BULLET_LIBRARIES}
    ${GUACAMOLE_LIBRARIES}
    ${OVR_LIBRARIES}
)

IF (UNIX)
    LIST(APPEND LIBS udev)
    LIST(APPEND LIBS Xinerama)
ELSEIF (MSVC)
    LIST(APPEND LIBS winmm.lib)
ENDIF(UNIX)

link_libraries(${LIBS})

set(LIB_PATHS
    ${OVR_LIBRARY_DIRS}
    ${SCHISM_LIBRARY_DIRS}
    ${GUACAMOLE_LIBRARY_DIRS}
    ${ASSIMP_LIBRARY_DIRS}
    ${JSON_LIBRARY_DIRS}
    ${GL_LIBRARY_DIRS}
    ${GLEW_LIBRARY_DIRS}
)

set(INCLUDE_PATHS
    ${CMAKE_CURRENT_SOURCE_DIR}/build
    ${CMAKE_CURRENT_SOURCE_DIR}/include
    ${SCHISM_INCLUDE_DIRS}
    ${BOOST_INCLUDE_DIRS}
    ${ASSIMP_INCLUDE_DIRS}
    ${JSON_INCLUDE_DIRS}
    ${GL_INCLUDE_DIRS}
    ${GLEW_INCLUDE_DIRS}
    ${BULLET_INCLUDE_DIRS}
    ${GUACAMOLE_INCLUDE_DIRS}
    ${OVR_INCLUDE_DIRS}
)
include_directories(${INCLUDE_PATHS})

if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release")
endif()

IF (UNIX)
    set(CMAKE_CXX_FLAGS_RELEASE "-s -O4 --std=c++0x -fpermissive")
    set(CMAKE_CXX_FLAGS_DEBUG "-g -Wall --std=c++0x -fpermissive")
ELSEIF (MSVC)
    set(CMAKE_CXX_FLAGS_RELEASE "-D NDEBUG -D _SECURE_SCL=0 -D _SCL_SECURE_NO_WARNINGS -D _CRT_SECURE_NO_DEPRECATE /MD /MP")
	set(CMAKE_CXX_FLAGS_DEBUG "-D_DEBUG /MDd /Zi")
ENDIF(UNIX)

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

message( "" )
message( "Summary:" )
message( " build type: ${CMAKE_BUILD_TYPE}" )
message( "" )
message( " schism:" )
message( "   library: ${SCHISM_LIBRARIES}" )
message( "   library paths: ${SCHISM_LIBRARY_DIRS}" )
message( "   include: ${SCHISM_INCLUDE_DIRS}" )
message( "" )
message( " boost:" )
message( "   library: ${BOOST_LIBRARIES}" )
message( "   library paths: ${BOOST_LIBRARY_DIRS}" )
message( "   include: ${BOOST_INCLUDE_DIRS}" )
message( "" )
message( " assimp:" )
message( "   library: ${ASSIMP_LIBRARIES}" )
message( "   library paths: ${ASSIMP_LIBRARY_DIRS}" )
message( "   include: ${ASSIMP_INCLUDE_DIRS}" )
message( "" )
message( " gl:" )
message( "   library: ${GL_LIBRARIES}" )
message( "   include: ${GL_INCLUDE_DIRS}" )
message( "" )
message( " glew:" )
message( "   library: ${GLEW_LIBRARIES}" )
message( "   include: ${GLEW_INCLUDE_DIRS}" )
message( "" )
message( " ev:" )
message( "   library: ${EV_LIBRARIES}" )
message( "   include: ${EV_INCLUDE_DIRS}" )
message( "" )
message( " bullet:" )
message( "   library: ${BULLET_LIBRARIES}" )
message( "   library paths: ${BULLET_LIBRARY_DIRS}" )
message( "   include: ${BULLET_INCLUDE_DIRS}" )
message( "" )
message( " guacamole:" )
message( "   library: ${GUA_LIBRARIES}" )
message( "   library paths: ${GUA_LIBRARY_DIRS}" )
message( "   include: ${GUA_INCLUDE_DIRS}" )
message( "" )
message( " ovr:" )
message( "   library: ${OVR_LIBRARIES}" )
message( "   library path: ${OVR_LIBRARY_DIRS}" )
message( "   include: ${OVR_INCLUDE_DIRS}" )
message( "" )
