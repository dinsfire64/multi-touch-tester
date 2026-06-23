cmake_minimum_required(VERSION 3.13)

set(CMAKE_SYSTEM_NAME Windows)
set(CMAKE_SYSTEM_VERSION 10)

set(CMAKE_C_COMPILER x86_64-w64-mingw32-gcc)
set(CMAKE_CXX_COMPILER x86_64-w64-mingw32-g++)
set(CMAKE_RC_COMPILER x86_64-w64-mingw32-windres)

set(CMAKE_C_FLAGS "-O2")
set(CMAKE_CXX_FLAGS "-O2")

project(MultiTouchTester)

set(VERSION_MAJOR 1)
set(VERSION_MINOR 0)
set(VERSION_REVISION 0)
execute_process(
    COMMAND
        git rev-parse --short=4 HEAD
    OUTPUT_VARIABLE
        VERSION_BUILD
    OUTPUT_STRIP_TRAILING_WHITESPACE
)

configure_file(
  ${CMAKE_CURRENT_SOURCE_DIR}/version.src
  ${CMAKE_CURRENT_SOURCE_DIR}/version.h
  @ONLY)

include_directories(
    "./imgui"
)

add_executable(
    MultiTouchTester WIN32
        "./main.cpp"
        "./version.rc"
)

target_sources(
    MultiTouchTester
    PUBLIC
        "./imgui/backends/imgui_impl_dx11.cpp"
        "./imgui/backends/imgui_impl_win32.cpp"
        "./imgui/imgui.cpp"
        "./imgui/imgui_demo.cpp"
        "./imgui/imgui_draw.cpp"
        "./imgui/imgui_tables.cpp"
        "./imgui/imgui_widgets.cpp"
)

target_link_libraries(
    MultiTouchTester
        setupapi
        shcore
        d3d11
        d3dcompiler
        xinput
)

target_compile_definitions(
    MultiTouchTester
    PRIVATE
        -D_UNICODE
        -DUNICODE
)

target_link_options(MultiTouchTester PRIVATE
    -static
    -static-libgcc
    -static-libstdc++
)

set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /MT")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MTd")
#set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /MANIFEST:NO /ManifestFile:Manifest.xml")