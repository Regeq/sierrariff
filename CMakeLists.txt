cmake_minimum_required(VERSION 4.1)
project(sierrariff)

set(CMAKE_CXX_STANDARD 17)

include_directories(src)

add_executable(sierrariff
        src/Player.cpp
        src/Player.h)

# vcpkg packages
find_package(TagLib REQUIRED)
find_package(ftxui REQUIRED)
find_package(CLI11 REQUIRED)

target_link_libraries(mplayer PRIVATE
        TagLib::TagLib
        ftxui::screen
        ftxui::dom
        ftxui::component
        CLI11::CLI11
)