# Minimum cmake verison 3.1 (required for C standard)
cmake_minimum_required (VERSION 3.1)

#########
# setup #
#########

cmake_policy( SET CMP0011 NEW )
cmake_policy( SET CMP0012 NEW ) # how if-statements work
cmake_policy( SET CMP0042 NEW ) # rpath on mac os xcmake_policy( SET CMP0048 NEW ) # version in project()
project( DL_Min_C VERSION 0.0.0 )

if (CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    set (CMAKE_INSTALL_PREFIX ${PROJECT_BINARY_DIR} CACHE PATH "Install prefix" FORCE )
endif()

# Get the dripline version
file( STRINGS "dripline_shield.json" JSON_STRINGS )
list( FILTER JSON_STRINGS INCLUDE REGEX "message" )
list( GET JSON_STRINGS 0 JSON_STRING )
string( REGEX MATCH "[0-9]+\\.[0-9]+\\.[0-9]+" DRIPLINE_PROTOCOL_VERSION ${JSON_STRING})
message( STATUS "Dripline protocol version: ${DRIPLINE_PROTOCOL_VERSION}" )

# standard install directories
include( GNUInstallDirs )

# build shared libraries
set( BUILD_SHARED_LIBS ON )

# turn on RPATH for Mac OSX
set( CMAKE_MACOSX_RPATH ON )

# add the library install directory to the rpath
SET(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_LIBDIR}" )

# add the automatically determined parts of the RPATH
# which point to directories outside the build tree to the install RPATH
set( CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE )


####################
# dripline options #
####################

set( DL_Min_C_Backend "rabbitmqc" )

option( DL_Min_C_BUILD_EXAMPLES "Flag to enable building the examples" TRUE )

option( DL_Min_C_ENABLE_TESTING "Enable build of tests" FALSE )

set( DL_Min_C_MAX_PAYLOAD_SIZE "10000" CACHE STRING "Maximum payload size (bytes)" )

# always use C11
set( CMAKE_C_STANDARD 11 )


# Process options

if( DL_Min_C_OFFLINE )
    add_definitions( -DDL_OFFLINE )
else()
    remove_definitions( -DDL_OFFLINE )
endif()

add_definitions( -DDL_MAX_PAYLOAD_SIZE=${DL_Min_C_MAX_PAYLOAD_SIZE} )


#########################
# External dependencies #
#########################

set( PUBLIC_EXT_LIBS )
set( PRIVATE_EXT_LIBS )

set( CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake )

##############
# rabbitmq-c #
##############


find_package( Rabbitmqc REQUIRED )

INCLUDE_DIRECTORIES(SYSTEM ${Rabbitmqc_INCLUDE_DIRS})


#####################
# Prepare for Build #
#####################




##################
# Build Dripline #
##################

add_subdirectory( library )

#if( DL_Min_C_ENABLE_EXECUTABLES )
#    add_subdirectory( executables )
#endif()

############
# Examples #
############

if( DL_Min_C_BUILD_EXAMPLES )
    add_subdirectory( examples )
endif()

#########
# Tests #
#########

if( DL_Min_C_ENABLE_TESTING )
    add_subdirectory( testing )
endif()

##################
# Package Config #
##################

#configure_file( ${PROJECT_SOURCE_DIR}/DriplineConfig.cmake.in ${CMAKE_CURRENT_BINARY_DIR}/DriplineConfig.cmake @ONLY )

#pbuilder_do_package_config()
