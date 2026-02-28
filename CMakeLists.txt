project(imgui)

set(IMGUI_SOURCES
    imgui.cpp
    imgui_draw.cpp
    imgui_tables.cpp
    imgui_widgets.cpp
    imgui_demo.cpp
    backends/imgui_impl_glfw.cpp
)

if(YENGINE_WITH_DX12)
    list(APPEND IMGUI_SOURCES backends/imgui_impl_dx12.cpp)
endif()

if(YENGINE_WITH_VULKAN)
    list(APPEND IMGUI_SOURCES backends/imgui_impl_vulkan.cpp)
endif()

add_library(imgui STATIC ${IMGUI_SOURCES})
add_library(imgui::imgui ALIAS imgui)

target_include_directories(imgui
    PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}/backends
)

target_link_libraries(imgui PUBLIC glfw)

if(YENGINE_WITH_VULKAN)
    target_link_libraries(imgui PUBLIC Vulkan::Vulkan)
endif()
