# CMakeLists.txt for dripline-min-c/library
# Author: N. Oblath
# Created: Mar 1, 2021

##########

include_directories( BEFORE
    ${CMAKE_CURRENT_SOURCE_DIR}
)

set( dlminc_HEADERS
    hello_world.h
)

set( dlminc_SOURCES
    hello_world.c
)

##########

add_library( DLMinC ${dlminc_SOURCES} )
set_target_properties( DLMinC PROPERTIES VERSION ${${PROJECT_NAME}_VERSION} )
set_target_properties( DLMinC PROPERTIES PUBLIC_HEADER ${dlminc_HEADERS} )
get_target_property( SOURCE_TREE_INCLUDE_DIRS DLMinC INCLUDE_DIRECTORIES )
target_link_libraries( DLMinC 
    PUBLIC ${PUBLIC_EXT_LIBS}
    PRIVATE ${PRIVATE_EXT_LIBS} 
)
target_include_directories( DLMinC 
    INTERFACE 
        "$<BUILD_INTERFACE:${SOURCE_TREE_INCLUDE_DIRS}>"
        "$<INSTALL_INTERFACE:$<INSTALL_PREFIX>/include>"
)

# install libraries and add to export for installation
install( TARGETS DLMinC
    EXPORT DLMinC_Library_Targets
    COMPONENT Library
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

# make targets available at build time
export( TARGETS DLMinC
    FILE ${CMAKE_INSTALL_BINDIR}/DLMinC_Library_Targets.cmake
)

