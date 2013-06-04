cmake_minimum_required(VERSION 2.8)
project(bzip2)

if (NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

if(MSVC OR MSVC90 OR MSVC10)
  set(MSVC ON)
endif (MSVC OR MSVC90 OR MSVC10)

file(GLOB sources *.c)
set(bzip2_STAT_SRCS
  ${sources}
)

include_directories(
  ${CMAKE_CURRENT_SOURCE_DIR}
)

add_library(bz2 ${bzip2_STAT_SRCS})

target_link_libraries(bz2)

install(TARGETS bz2
  DESTINATION lib/)

install(FILES bzlib.h
  DESTINATION include)

set(PKG_NAME ${PROJECT_NAME})
set(PKG_LIBRARIES bz2)
set(cmake_conf_file "${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT_NAME}-config.cmake")
configure_file("${cmake_conf_file}.in" "${cmake_conf_file}" @ONLY)
install(FILES ${cmake_conf_file} DESTINATION share/${PROJECT_NAME}/cmake/ COMPONENT cmake)

if (NOT MSVC)
  set(PKG_DESC "bzip2")
  set(PKG_CB_LIBS "-lbz2")
  set(pkg_conf_file "${CMAKE_CURRENT_SOURCE_DIR}/bzip2.pc")
  configure_file("${pkg_conf_file}.in" "${pkg_conf_file}" @ONLY)
  install(FILES ${pkg_conf_file} DESTINATION lib/pkgconfig/ COMPONENT pkgconfig)
endif()

