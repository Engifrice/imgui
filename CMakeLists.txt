cmake_minimum_required(VERSION 3.21)
project(ImGUI)

add_library(libimgui
    "imgui_knobs.cpp"
    "misc/cpp/imgui_stdlib.cpp"
    "imgui.cpp"
    "imgui_draw.cpp"
    "imgui_demo.cpp"
    "imgui_tables.cpp"
    "imgui_widgets.cpp"
)
set_target_properties(libimgui PROPERTIES PREFIX "")
target_include_directories(libimgui
    PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}"
    PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/backends"
)

if(WIN32)
    set(IMGUI_EXPORT_DEF "IMGUI_API=__declspec(dllexport)")
    set(IMGUI_IMPORT_DEF "IMGUI_API=__declspec(dllimport)")
else()
    set(IMGUI_EXPORT_DEF "IMGUI_API=__attribute__((visibility(\"default\")))")
    set(IMGUI_IMPORT_DEF "IMGUI_API=")
endif()

target_compile_definitions(libimgui PRIVATE ${IMGUI_EXPORT_DEF})

add_library(imgui INTERFACE)
target_compile_definitions(imgui INTERFACE ${IMGUI_IMPORT_DEF})
target_link_libraries(imgui INTERFACE libimgui)