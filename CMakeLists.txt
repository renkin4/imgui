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
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/backends>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/imgui>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/imgui/backends>
)

target_link_libraries(imgui PUBLIC glfw)

if(YENGINE_WITH_VULKAN)
    target_link_libraries(imgui PUBLIC Vulkan::Vulkan)
endif()

include(GNUInstallDirs)

install(TARGETS imgui
    ARCHIVE DESTINATION lib/$<CONFIG>
)

install(FILES
    imgui.h
    imgui_internal.h
    imconfig.h
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/imgui
)

install(FILES
    backends/imgui_impl_glfw.h
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/imgui/backends
)

if(YENGINE_WITH_DX12)
    install(FILES backends/imgui_impl_dx12.h
        DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/imgui/backends
    )
endif()

if(YENGINE_WITH_VULKAN)
    install(FILES backends/imgui_impl_vulkan.h
        DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/imgui/backends
    )
endif()

