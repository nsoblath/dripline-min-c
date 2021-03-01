# CMakeLists.txt for dripline-min-c/examples
# Author: N. Oblath
# Created: Mar 1, 2021

###########
# Library #
###########

#include_directories( BEFORE
#    ${CMAKE_CURRENT_SOURCE_DIR}
#)

#set( dripline_examples_HEADERS
#    oscillator.hh
#    oscillator_service_endpoints.hh
#    oscillator_service_hub.hh
#    simple_service.hh
#)

#set( dripline_examples_SOURCES
#    oscillator.cc
#    oscillator_service_endpoints.cc
#    oscillator_service_hub.cc
#    simple_service.cc
#)

#set ( dripline_examples_LIBS
#    Dripline
#)

##########

#pbuilder_library( 
#    TARGET DriplineExamples 
 #   SOURCES ${dripline_examples_SOURCES}
#    PROJECT_LIBRARIES ${dripline_examples_LIBS}
#    #PUBLIC_EXTERNAL_LIBRARIES ${PUBLIC_EXT_LIBS}
#    #PRIVATE_EXTERNAL_LIBRARIES ${PRIVATE_EXT_LIBS}
#)

#pbuilder_install_headers( ${dripline_examples_HEADERS} )

###############
# Executables #
###############

set( dlminc_examples_exe_LIBS 
    DLMinC
    #DriplineExamples
)
set( dlminc_examples_exe_SOURCES
    say_hello.c
)

add_executable( say_hello say_hello.c )

target_link_libraries( say_hello
    PUBLIC ${dlminc_examples_exe_LIBS}
)

install( TARGETS say_hello
    EXPORT DLMinC_Examples_Targets
    COMPONENT Examples
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

