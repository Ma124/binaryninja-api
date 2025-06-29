cmake_minimum_required(VERSION 3.24 FATAL_ERROR)

project(lang_forkedc CXX)

file(GLOB SOURCES
	*.cpp
	*.h)

add_library(lang_forkedc SHARED ${SOURCES})

target_include_directories(lang_forkedc
	PRIVATE ${PROJECT_SOURCE_DIR})

if(WIN32)
	target_link_directories(lang_forkedc PRIVATE ${BN_INSTALL_DIR})
	target_link_libraries(lang_forkedc binaryninjaapi binaryninjacore)
else()
	target_link_libraries(lang_forkedc binaryninjaapi)
endif()

set_target_properties(lang_forkedc PROPERTIES
    CXX_STANDARD 20
	CXX_VISIBILITY_PRESET hidden
	CXX_STANDARD_REQUIRED ON
	C_STANDARD 99
	C_STANDARD_REQUIRED ON
    C_VISIBILITY_PRESET hidden
    VISIBILITY_INLINES_HIDDEN ON
	POSITION_INDEPENDENT_CODE ON
	HEADLESS 1
	CMAKE_CXX_FLAGS_DEBUG_INIT "-g")

find_path(
    BN_API_PATH
    NAMES binaryninjaapi.h
    # List of paths to search for the clone of the api
    HINTS ../.. binaryninjaapi $ENV{BN_API_PATH}
    REQUIRED
)
add_subdirectory(${BN_API_PATH} api)
