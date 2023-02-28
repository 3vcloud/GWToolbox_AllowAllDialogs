cmake_minimum_required(VERSION 3.16)

set(PROJECT_NAME GWToolbox_AllowAllDialogs)

project(${PROJECT_NAME})

if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    message(FATAL_ERROR "You are configuring a 64bit build, this is not supported. Run cmake with `-A Win32`")
endif()

set_property(GLOBAL PROPERTY USE_FOLDERS ON)
 


set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED OFF)
set(MSVC_CRT_VERSION "VC142")
set(GWTOOLBOXPP_SOURCE_DIR "${CMAKE_CURRENT_LIST_DIR}/../../GWToolboxpp2")

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/../cmake")

# Outputs dll, exe, and pdb into a /bin/config folder
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_LIST_DIR}/bin/")

include(gwca)
include(imgui)
include(imgui_fonts)

add_compile_definitions(
    "NOMINMAX"
    "_USE_MATH_DEFINES"
    "WIN32_LEAN_AND_MEAN"
    "SETTINGS_FOLDER=\"${CMAKE_CURRENT_LIST_DIR}\""
    )

add_library(${PROJECT_NAME} SHARED)
file(GLOB SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/*.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp")

target_sources(${PROJECT_NAME} PRIVATE ${SOURCES})
target_compile_definitions(${PROJECT_NAME} PRIVATE "_USRDLL")
target_compile_definitions(${PROJECT_NAME} PRIVATE BUILD_DLL)
target_compile_options(${PROJECT_NAME} PRIVATE /W4 /WX /ZI)

target_include_directories(${PROJECT_NAME} PRIVATE 
    "${CMAKE_CURRENT_SOURCE_DIR}"
    )
target_link_libraries(${PROJECT_NAME} PRIVATE gwca)
