cmake_minimum_required (VERSION 3.11)
project(chronicle_sth_decoder VERSION 0.1 LANGUAGES CXX)

set(default_build_type "Release")
 
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "Setting build type to '${default_build_type}' as none was specified.")
  set(CMAKE_BUILD_TYPE "${default_build_type}" CACHE
      STRING "Choose the type of build." FORCE)
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
    "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

FIND_PACKAGE(Boost 1.58 REQUIRED COMPONENTS date_time system iostreams program_options)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)


add_subdirectory(external/abieos)
add_subdirectory(external/chainbase)

add_executable(chronicle_sth_decoder src/chronicle_sth_decoder.cpp)

target_include_directories(chronicle_sth_decoder
  PUBLIC external/abieos/src external/abieos/external/rapidjson/include external/chainbase/include
  PRIVATE ${Boost_INCLUDE_DIR})

target_link_libraries(chronicle_sth_decoder
  PRIVATE abieos chainbase
  PUBLIC Boost::date_time Boost::system Boost::iostreams Boost::program_options -lpthread)


if (CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    target_compile_options(chronicle_sth_decoder PRIVATE -Wall -Wextra -Wno-unused-parameter -fcolor-diagnostics)
endif()
