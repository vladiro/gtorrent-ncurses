###############
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

PROJECT(gtorrent-ncurses)

SET(gtorrent-ncurses_VERSION_MAJOR 0)
SET(gtorrent-ncurses_VERSION_MINOR 0)
SET(gtorrent-ncurses_VERSION_PATCH 1)
###############

# Configure version into Version.hpp
SET (VERSION ${gtorrent-ncurses_VERSION_MAJOR}.${gtorrent-ncurses_VERSION_MINOR}.${gtorrent-ncurses_VERSION_PATCH})
CONFIGURE_FILE (src/utils/Version.hpp.in Version.hpp @ONLY)

# Set compiler options
SET (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -g -Wall")

# Set Cmake to build runtime in cwd
SET (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR})

# Find Boost
SET (Boost_USE_STATIC_LIBS ON)
SET (Boost_USE_MULTITHREADED ON)
SET (Boost_USE_STATIC_RUNTIME OFF)
FIND_PACKAGE(Boost COMPONENTS system REQUIRED)

# Find libtorrent
INCLUDE (FindPkgConfig)
PKG_SEARCH_MODULE (LIBTORRENT REQUIRED libtorrent-rasterbar)

# Find ncurses
INCLUDE(CheckLibraryExists)
PKG_SEARCH_MODULE (NCURSESPP REQUIRED ncurses++w)
PKG_SEARCH_MODULE (NCURSES REQUIRED ncursesw)
PKG_SEARCH_MODULE (NCURSESPANEL REQUIRED panelw)

# To find version
INCLUDE_DIRECTORIES(${PROJECT_BINARY_DIR})


ADD_DEFINITIONS (
  ${LIBTORRENT_CFLAGS}
  )

ADD_SUBDIRECTORY(gtorrent-core)
ADD_SUBDIRECTORY(src)
