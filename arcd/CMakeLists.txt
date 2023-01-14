cmake_minimum_required(VERSION 3.2)

# arcd target (required)
set(HEADERS_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(HEADERS arcd.h)
set(SOURCES arcd.c)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99 -Wall -Wextra -Werror -pedantic-errors")

add_library(arcd ${HEADERS} ${SOURCES})
target_include_directories(arcd PUBLIC $<BUILD_INTERFACE:${HEADERS_DIR}>)

# install (optional)

include(GNUInstallDirs)

if(ARCD_CONFIGURE_INSTALL)
	install(TARGETS arcd EXPORT arcd
		INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
		ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})
	install(DIRECTORY ${HEADERS_DIR}/
		DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
		FILES_MATCHING PATTERN "*.h")
endif()
