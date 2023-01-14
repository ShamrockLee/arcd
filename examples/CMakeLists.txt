cmake_minimum_required(VERSION 3.2)

set(EXAMPLE_HEADERS_DIR ${CMAKE_CURRENT_SOURCE_DIR})

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99 -Wall -Wextra -Werror -pedantic-errors")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall -Wextra -Werror -pedantic-errors")

add_library(adaptive_model adaptive_model.c adaptive_model.h)
target_include_directories(adaptive_model PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>)
target_link_libraries(adaptive_model arcd)

add_executable(arcd_stream arcd_stream.c)
target_link_libraries(arcd_stream arcd adaptive_model)

if(ARCD_EXAMPLES_INSTALL)
	install(TARGETS adaptive_model EXPORT arcd_adaptive_model
		INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
		ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
		RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
	install(TARGETS arcd_stream EXPORT arcd_stream
		INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
		ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})
	install(DIRECTORY ${EXAMPLE_HEADERS_DIR}/
		DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
		FILES_MATCHING PATTERN "*.h")
endif()
