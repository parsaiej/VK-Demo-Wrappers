# Library Name
# -----------------------

set(WRAPPERS_NAME VulkanWrappers)

# Misc. Packages.
# -----------------------

find_package(glfw3 REQUIRED)
find_package(Vulkan REQUIRED)

# Add VMA
# -----------------------

add_subdirectory(VulkanMemoryAllocator)

# OS-specific needs.
# -----------------------

if (APPLE)
    find_library(COCOA_LIBRARY Cocoa REQUIRED)
    find_library(QUARTZ_CORE QuartzCore REQUIRED)

    add_library(MetalUtility SHARED "MetalUtility/MetalUtility.m")

    # Link
    target_link_libraries(MetalUtility glfw)
    target_link_libraries(MetalUtility ${QUARTZ_CORE})
    target_link_libraries(MetalUtility ${COCOA_LIBRARY})
endif ()

# Library
# -----------------------

add_library(${WRAPPERS_NAME} SHARED 
    "VmaUsage.cpp" 
    "Device.cpp" 
    "Window.cpp"
    "Image.cpp"
)

# Include
# -----------------------

target_include_directories(${WRAPPERS_NAME} PRIVATE ${GLFW_INCLUDE_DIRS})
target_include_directories(${WRAPPERS_NAME} PRIVATE ${Vulkan_INCLUDE_DIRS})
target_include_directories(${WRAPPERS_NAME} PRIVATE "MetalUtility/")
target_include_directories(${WRAPPERS_NAME} PRIVATE "Include/")

# Link
# -----------------------

target_link_libraries(${WRAPPERS_NAME} PRIVATE ${Vulkan_LIBRARIES})
target_link_libraries(${WRAPPERS_NAME} PRIVATE glfw)
target_link_libraries(${WRAPPERS_NAME} PRIVATE MetalUtility)
target_link_libraries(${WRAPPERS_NAME} PRIVATE GPUOpen::VulkanMemoryAllocator)