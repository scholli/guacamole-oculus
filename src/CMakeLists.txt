# determine source and header files
file(GLOB_RECURSE GUACAMOLE_OCULUS_SRC RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
    *.cpp
    ../include/*.h
    ../include/*.hpp
)

LINK_DIRECTORIES(${LIB_PATHS})

ADD_LIBRARY( guacamole-oculus SHARED
    ${GUACAMOLE_OCULUS_SRC}
)

IF (MSVC)
	set_target_properties(guacamole-oculus PROPERTIES COMPILE_FLAGS "-D GUA_OCULUS_LIBRARY")
ENDIF (MSVC)

LIST(LENGTH _LIB_COUNT LIBS)
IF (_LIB_COUNT GREATER 0)
    TARGET_LINK_LIBRARIES( guacamole-oculus debug ${LIBS} optimized ${LIBS})
ENDIF (_LIB_COUNT GREATER 0)
