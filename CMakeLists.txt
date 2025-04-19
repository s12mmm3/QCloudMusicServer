cmake_minimum_required(VERSION 3.14)

project(QCloudMusicServerProject VERSION 1.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_POLICY_VERSION_MINIMUM 3.5)

if(MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /Zi /EHa")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /DEBUG")
    set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} /DEBUG")
    set_property(DIRECTORY PROPERTY VS_STARTUP_PROJECT ${PROJECT_NAME})
endif()

# 设置第三方仓库
include_directories(3rdparty)

set(BUILD_SHARED_LIBS OFF)
set(MUSICAPI_TEST OFF)
add_subdirectory(3rdparty/QCloudMusicApi)
include_directories(3rdparty/QCloudMusicApi)

add_subdirectory(QCloudMusicServer)