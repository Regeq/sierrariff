cmake_minimum_required(VERSION 3.20)

project(SierraRiff
        VERSION 0.0.1
        DESCRIPTION "A minimal, modular music player"
        LANGUAGES C CXX
)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(ftxui  CONFIG REQUIRED)
find_package(CLI11  CONFIG REQUIRED)
find_package(TagLib CONFIG REQUIRED)
find_package(CURL   CONFIG REQUIRED)
find_package(miniz  CONFIG REQUIRED)

set(SR_CORE_SOURCES
        include/miniaudio.c

        src/player/Player.cpp
        src/queue/Queue.cpp
        src/tags/TagEditor.cpp

        src/core/config.cpp
        src/core/package_manager.cpp

        src/modules/module_registry.cpp
        src/modules/lyrics/lyrics_module.cpp
        src/modules/lyrics/providers/lrc_provider.cpp
        src/modules/lyrics/providers/txt_provider.cpp
        src/modules/metadata/meta_module.cpp
)

set(SR_INCLUDE_DIRS
        ${CMAKE_CURRENT_SOURCE_DIR}/include
        ${CMAKE_CURRENT_SOURCE_DIR}/src
)

set(SR_COMPILE_DEFS
        SR_VERSION="${PROJECT_VERSION}"
)


set(SR_CORE_LIBS
        CLI11::CLI11
        TagLib::TagLib
        CURL::libcurl
        miniz::miniz
)

if(WIN32)
    list(APPEND SR_CORE_LIBS winmm)
endif()

add_executable(sr
        src/main.cpp

        src/tui/TUI.cpp

        src/ui/app.cpp
        src/ui/keybinds.cpp
        src/ui/layouts/layout_full.cpp
        src/ui/layouts/layout_minimal.cpp
        src/ui/layouts/layout_focus.cpp
        src/ui/components/seekbar.cpp
        src/ui/components/tracklist.cpp
        src/ui/components/statusbar.cpp
        src/ui/components/metapanel.cpp

        ${SR_CORE_SOURCES}
)

target_include_directories(sr PRIVATE ${SR_INCLUDE_DIRS})
target_compile_definitions(sr PRIVATE ${SR_COMPILE_DEFS})

target_link_libraries(sr PRIVATE
        ${SR_CORE_LIBS}
        ftxui::screen
        ftxui::dom
        ftxui::component
)

find_package(SDL3  CONFIG REQUIRED)
find_package(imgui CONFIG REQUIRED)

add_executable(sr-gui
        src/gui/main_gui.cpp

        src/gui/app_gui.cpp
        src/gui/skin_engine.cpp
        src/gui/srss_parser.cpp
        src/gui/layouts/layout_full.cpp
        src/gui/layouts/layout_minimal.cpp
        src/gui/layouts/layout_focus.cpp

        ${SR_CORE_SOURCES}
)

target_include_directories(sr-gui PRIVATE ${SR_INCLUDE_DIRS})
target_compile_definitions(sr-gui PRIVATE ${SR_COMPILE_DEFS})

target_link_libraries(sr-gui PRIVATE
        ${SR_CORE_LIBS}
        imgui::imgui
        SDL3::SDL3
)

set_target_properties(sr sr-gui PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}"
)

message(STATUS "")
message(STATUS "  SierraRiff v${PROJECT_VERSION}")
message(STATUS "  ───────────────────────────────")
message(STATUS "  Audio   : miniaudio ma_engine (high level)")
message(STATUS "  TUI     : FTXUI")
message(STATUS "  GUI     : ImGui + SDL3")
message(STATUS "  Tags    : TagLib")
message(STATUS "")