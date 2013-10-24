# determine source and header files
file(GLOB_RECURSE GUACAMOLE_OCULUS_SRC RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
    *.cpp
    ../include/*.h
    ../include/*.hpp
)

ADD_LIBRARY( guacamole-oculus SHARED
    ${GUACAMOLE_OCULUS_SRC}
)

TARGET_LINK_LIBRARIES( guacamole-oculus  ${LIBS} )
