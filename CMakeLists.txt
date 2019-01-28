cmake_minimum_required(VERSION 3.2 FATAL_ERROR)
cmake_policy(VERSION 3.2)

set(MINIZ_VERSION "2.0.8")
project(miniz VERSION ${MINIZ_VERSION} LANGUAGES C)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

include_directories("${PROJECT_SOURCE_DIR}")

option(MINIZ_INSTALL_ENABLE "MINIZ install" ON)

set(MINIZ_INSTALL_CONFIGDIR "${CMAKE_INSTALL_LIBDIR}/cmake/miniz")
if (MINIZ_INSTALL_ENABLE)
	install(DIRECTORY miniz DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
endif()

write_basic_package_version_file("${CMAKE_CURRENT_BINARY_DIR}/minizConfigVersion.cmake" VERSION ${MINIZ_VERSION} COMPATIBILITY AnyNewerVersion)

# build tree package config
set(MINIZ_SOURCES miniz/miniz_zip.c)
configure_file(cmake/minizBuildConfig.cmake.in minizConfig.cmake @ONLY)

# install tree package config
configure_package_config_file(
	cmake/minizConfig.cmake.in
	${MINIZ_INSTALL_CONFIGDIR}/minizConfig.cmake
	INSTALL_DESTINATION ${MINIZ_INSTALL_CONFIGDIR}
	PATH_VARS CMAKE_INSTALL_INCLUDEDIR
	NO_CHECK_REQUIRED_COMPONENTS_MACRO)

if(MINIZ_INSTALL_ENABLE)
	install(FILES
		"${CMAKE_CURRENT_BINARY_DIR}/${MINIZ_INSTALL_CONFIGDIR}/minizConfig.cmake"
		"${CMAKE_CURRENT_BINARY_DIR}/minizConfigVersion.cmake"
		DESTINATION ${MINIZ_INSTALL_CONFIGDIR})
endif()

export(PACKAGE miniz)
