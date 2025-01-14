cmake_minimum_required(VERSION 3.13)

project(mqtt-codec)

option(STAIC_LIB "This is a default option for build static lib" ON)
option(SHARE_LIB "This is a default option for build dynamic lib" ON)
option(DEBUG "This is a default option for DEBUG" OFF)
option(EXAMPLE "This is a
default option for EXAMPLE" OFF)

if(DEBUG)
  set(CMAKE_BUILD_TYPE "Debug")
  if(APPLE)
    set(CMAKE_C_FLAGS
      "${CMAKE_C_FLAGS} -Wall -g -fsanitize=address")
  else()
    set(CMAKE_C_FLAGS
      "${CMAKE_C_FLAGS} -Wall -g -fsanitize=address -fsanitize=leak")
  endif()
  add_definitions(-DDEBUG)
endif(DEBUG)

set(SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/src/mqtt_basic.c
            ${CMAKE_CURRENT_SOURCE_DIR}/src/mqtt_codec.c)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

if(STAIC_LIB)
  add_library(mqtt-codec SHARED ${SOURCES})
endif(STAIC_LIB)

if(SHARE_LIB)
  add_library(mqtt-codec-static STATIC ${SOURCES})
endif()

if(EXAMPLE)
  add_executable(mqtt-codec-test test/example.c)

  target_link_libraries(mqtt-codec-test mqtt-codec)
  target_link_libraries(mqtt-codec-test m)
endif(EXAMPLE)
