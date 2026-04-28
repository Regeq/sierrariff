cmake_minimum_required(VERSION 3.20)
project(SierraRiff
        VERSION 0.0.1
        DESCRIPTION "A minimal, modular music player"
        LANGUAGES C CXX
)

set(CMAKE_CXX_STANDARD 17)

add_executable(sr
        src/main.cpp
        src/player/Player.cpp
        src/queue/Queue.cpp
        src/tags/TagEditor.cpp
        src/tui/TUI.cpp
        src/tui/TUI.cpp
        src/tags/TagEditor.cpp
        src/queue/Queue.cpp
        src/player/Player.cpp
        src/main.cpp
)

target_include_directories(sr PRIVATE
        include/
        src/
)