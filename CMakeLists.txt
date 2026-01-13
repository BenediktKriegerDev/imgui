cmake_minimum_required(VERSION 3.7...4.0)
project(imgui VERSION 1.0.0)

set(IMGUI_SOURCES
    ${PROJECT_SOURCE_DIR}/imgui.cpp
    ${PROJECT_SOURCE_DIR}/imgui_draw.cpp
    ${PROJECT_SOURCE_DIR}/imgui_demo.cpp
    ${PROJECT_SOURCE_DIR}/imgui_tables.cpp
    ${PROJECT_SOURCE_DIR}/imgui_widgets.cpp
)

if(USE_BACKEND_VULKAN)
    set(IMGUI_SOURCES ${IMGUI_SOURCES}
        ${PROJECT_SOURCE_DIR}/backends/imgui_impl_vulkan.h
        ${PROJECT_SOURCE_DIR}/backends/imgui_impl_vulkan.cpp
    )
endif()

if(USE_BACKEND_WGPU)
    set(IMGUI_SOURCES ${IMGUI_SOURCES}
        ${PROJECT_SOURCE_DIR}/backends/imgui_impl_wgpu.h
        ${PROJECT_SOURCE_DIR}/backends/imgui_impl_wgpu.cpp
    )
endif()

if(USE_BACKEND_GLFW)
    set(IMGUI_SOURCES ${IMGUI_SOURCES}
        ${PROJECT_SOURCE_DIR}/backends/imgui_impl_glfw.h
        ${PROJECT_SOURCE_DIR}/backends/imgui_impl_glfw.cpp
    )
endif()

if(USE_BACKEND_SDL3)
    set(IMGUI_SOURCES ${IMGUI_SOURCES}
        ${PROJECT_SOURCE_DIR}/backends/imgui_impl_sdl3.h
        ${PROJECT_SOURCE_DIR}/backends/imgui_impl_sdl3.cpp
    )
endif()

add_library(imgui STATIC ${IMGUI_SOURCES})
target_include_directories(imgui PRIVATE ${PROJECT_SOURCE_DIR} ${PROJECT_SOURCE_DIR}/backends)

if(USE_BACKEND_GLFW)
    target_link_libraries(imgui PRIVATE glfw)
endif()
if(USE_BACKEND_SDL3)
    target_link_libraries(imgui PRIVATE SDL3::SDL3)
endif()

if(USE_BACKEND_VULKAN)
    find_package(Vulkan REQUIRED)
    if(Vulkan_FOUND)
        target_link_libraries(imgui PRIVATE ${Vulkan_LIBRARIES})
        target_include_directories(imgui PUBLIC ${Vulkan_INCLUDE_DIR})
    else()
        message(ERROR "Vulkan SDK has to be installed")
    endif()
endif()
