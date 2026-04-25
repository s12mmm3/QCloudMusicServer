cmake_minimum_required(VERSION 3.14)

project(QCloudMusicServer VERSION 1.0.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(QT_NO_MISSING_CATALOG_LANGUAGE_WARNING ON)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_POLICY_VERSION_MINIMUM 3.5)

find_package(Qt6 REQUIRED COMPONENTS Core HttpServer Network Concurrent)

if (WIN32)
    if (MSVC)
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /Zi /EHa")
        set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /DEBUG")
        set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} /DEBUG")
        set_property(DIRECTORY PROPERTY VS_STARTUP_PROJECT ${PROJECT_NAME})
    endif()
    set(VERSIONINFO_RC "${CMAKE_BINARY_DIR}/VersionInfo.rc")
    configure_file("${CMAKE_SOURCE_DIR}/VersionInfo.rc.in" "${VERSIONINFO_RC}")
elseif (APPLE)
    if (IOS)
        # Building for iOS
    elseif (UNIX)
        # Building for macOS
        set(CMAKE_OSX_ARCHITECTURES "x86_64;arm64")
    else()
    endif()
elseif (EMSCRIPTEN)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -pthread -s USE_PTHREADS=1 -s SHARED_MEMORY=1")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread -s USE_PTHREADS=1 -s SHARED_MEMORY=1")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -pthread -s USE_PTHREADS=1 -s SHARED_MEMORY=1")
endif()

# 设置第三方仓库
include_directories(3rdparty)

set(QCLOUDMUSICAPI_BUILD_SHARED OFF CACHE BOOL "Build QCloudMusicApi as a shared library" FORCE)
set(QCLOUDMUSICAPI_BUILD_TEST OFF CACHE BOOL "Build QCloudMusicApi tests" FORCE)
add_subdirectory(3rdparty/QCloudMusicApi)
include_directories(3rdparty/QCloudMusicApi)

qt_standard_project_setup(
    REQUIRES 6.8
    # I18N_TRANSLATED_LANGUAGES zh_CN zh_TW en_US de_DE fr_FR ar_SA ja_JP ko_KR pl_PL pt_BR pt_PT ru_RU es_ES uk_UA
)

FILE(GLOB SOURCES "./*.h" "./*.cpp" "./*.qrc" "manager/*" "task/*" "app/*")
qt_add_executable(${PROJECT_NAME} #[[WIN32]] MACOSX_BUNDLE
    ${SOURCES}
)

target_compile_definitions(${PROJECT_NAME} PRIVATE
    PROJECT_NAME="${PROJECT_NAME}"
)

target_link_libraries(${PROJECT_NAME} PRIVATE
    Qt6::Core
    Qt6::HttpServer
    Qt6::Network
    Qt6::Concurrent
    QCloudMusicApi
)

# Qt for iOS sets MACOSX_BUNDLE_GUI_IDENTIFIER automatically since Qt 6.1.
# If you are developing for iOS or macOS you should consider setting an
# explicit, fixed bundle identifier manually though.
set_target_properties(${PROJECT_NAME} PROPERTIES
    MACOSX_BUNDLE_GUI_IDENTIFIER com.overwriter.${PROJECT_NAME}
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    # MACOSX_BUNDLE_ICON_FILE "logo.icns"
    MACOSX_BUNDLE TRUE
    # WIN32_EXECUTABLE TRUE
)

include(GNUInstallDirs)
install(TARGETS ${PROJECT_NAME}
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

qt_generate_deploy_app_script(
    TARGET ${PROJECT_NAME}
    OUTPUT_SCRIPT deploy_script
    NO_UNSUPPORTED_PLATFORM_ERROR
)
install(SCRIPT ${deploy_script})
