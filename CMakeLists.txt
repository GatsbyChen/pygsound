cmake_minimum_required(VERSION 3.13.1)
project(gsound)

# Set a default build type if none was specified
if( NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES )
	message( STATUS "Setting build type to 'debug' as none was specified." )
	set( CMAKE_BUILD_TYPE debug CACHE STRING "Choose the type of build." FORCE )
	# Set the possible values of build type for cmake-gui
	set_property( CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "debug" "release" ) 
endif()

#set build directory
set(BUILD_DIRECTORY ${CMAKE_BUILD_TYPE})
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wno-deprecated")
set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${BUILD_DIRECTORY}/lib )
set( CMAKE_CXX_FLAGS_DEBUG "-g -O0 -fPIC" )
set( CMAKE_CXX_FLAGS_RELEASE "-O3 -Ofast -DNDEBUG -fPIC" )
#set( CMAKE_CXX_FLAGS_RELEASE "-O2 -march=native -DNDEBUG -fPIC" )

set( THREADS_PREFER_PTHREAD_FLAG ON )

find_package( Threads REQUIRED )
find_package( PkgConfig REQUIRED )
find_package( ZLIB REQUIRED )
pkg_check_modules( FFTW3 REQUIRED fftw3 )
pkg_check_modules( OGG REQUIRED ogg )
pkg_check_modules( VORBIS REQUIRED vorbis )

# Add include directories
include_directories(
	${CMAKE_SOURCE_DIR}/src/Om/Om\ Framework
	${CMAKE_SOURCE_DIR}/src/Om/Om\ BVH
	${CMAKE_SOURCE_DIR}/src/Om/Om\ Compression
	${CMAKE_SOURCE_DIR}/src/Om/Om\ Resources
	${CMAKE_SOURCE_DIR}/src/Om/Om\ Sound
	${CMAKE_SOURCE_DIR}/src/GSound
)

enable_testing()
add_subdirectory(src/Om)
add_subdirectory(src/GSound)
add_subdirectory(src/pygsound)
