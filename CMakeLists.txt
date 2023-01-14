cmake_minimum_required(VERSION 3.2)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_SOURCE_DIR}/cmake)
if(NOT DEFINED CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type")
endif()

project(arcd)

include(GNUInstallDirs)

if(NOT DEFINED INSTALL_CMAKE_DIR)
	set(INSTALL_CMAKE_DIR ${CMAKE_INSTALL_LIBDIR}/cmake/arcd CACHE PATH
		"Installation directory for CMake files")
endif()

option(ARCD_CONFIGURE_INSTALL "Generate install target" ON)
option(ARCD_EXAMPLES "Build examples" OFF)
option(ARCD_EXAMPLES_INSTALL "Install example executables and libraries when enabling ARCD_EXAMPLES" OFF)
option(ARCD_TESTS "Build tests" OFF)
option(ARCD_TESTS_INSTALL "Install test executables when enabling ARCD_TESTS" OFF)

add_subdirectory(arcd)

if(ARCD_EXAMPLES)
	add_subdirectory(examples)
endif()
if(ARCD_TESTS)
	enable_testing()
	add_subdirectory(tests)
endif()

if(ARCD_CONFIGURE_INSTALL)
	export(EXPORT arcd)
	install(EXPORT arcd
		DESTINATION ${INSTALL_CMAKE_DIR})
	configure_file(arcd-config.cmake.in arcd-config.cmake @ONLY)
	install(FILES ${CMAKE_CURRENT_BINARY_DIR}/arcd-config.cmake
		DESTINATION ${INSTALL_CMAKE_DIR})
endif()
