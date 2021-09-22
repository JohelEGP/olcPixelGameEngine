cmake_minimum_required(VERSION 3.21.0)
project(olcPixelGameEngine
        VERSION 2.16.0
        DESCRIPTION "An unofficial distribution of olcPixelGameEngine, with a CMake buildsystem"
        HOMEPAGE_URL "https://github.com/JohelEGP/olcPixelGameEngine/"
        LANGUAGES CXX)

set(PGE_LINK_LIBRARIES X11 GL pthread png CACHE STRING "List of linked libraries required to build PGE.")

add_library(olcPixelGameEngineHeader INTERFACE)
target_compile_features(olcPixelGameEngineHeader INTERFACE cxx_std_17)
target_include_directories(olcPixelGameEngineHeader SYSTEM INTERFACE $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>)
target_link_libraries(olcPixelGameEngineHeader INTERFACE ${PGE_LINK_LIBRARIES})
set_target_properties(olcPixelGameEngineHeader PROPERTIES EXPORT_NAME PixelGameEngineHeader)
add_library(olc::PixelGameEngineHeader ALIAS olcPixelGameEngineHeader)

add_library(olcPixelGameEngine EXCLUDE_FROM_ALL "PGE.cpp")
target_link_libraries(olcPixelGameEngine PUBLIC olc::PixelGameEngineHeader)
set_target_properties(olcPixelGameEngine PROPERTIES EXPORT_NAME PixelGameEngine)
add_library(olc::PixelGameEngine ALIAS olcPixelGameEngine)

install(TARGETS olcPixelGameEngineHeader olcPixelGameEngine EXPORT PGETargets)

export(EXPORT PGETargets NAMESPACE olc::)
configure_file("olcPixelGameEngineConfig.cmake" "." COPYONLY)
include(CMakePackageConfigHelpers)
write_basic_package_version_file(olcPixelGameEngineConfigVersion.cmake VERSION ${olcPixelGameEngine_VERSION}
                                 COMPATIBILITY SameMajorVersion)
