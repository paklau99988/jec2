project(LoadingSVG VERSION 0.0.1)

set (TargetName ${PROJECT_NAME})

if(NOT Torch_FOUND)
    list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../../cmake_find")
    find_package(Torch REQUIRED PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../../../libtorch")
endif()

juce_add_gui_app(${TargetName} PRODUCT_NAME "LoadingSVG")
juce_generate_juce_header(${TargetName})

target_sources(${TargetName} PRIVATE
        Source/Main.cpp)

target_compile_definitions(${TargetName} PRIVATE
        JUCE_WEB_BROWSER=0
        JUCE_USE_CURL=0
        JUCE_APPLICATION_NAME_STRING="$<TARGET_PROPERTY:${TargetName},JUCE_PROJECT_NAME>"
        JUCE_APPLICATION_VERSION_STRING="$<TARGET_PROPERTY:${TargetName},JUCE_VERSION>")

target_link_libraries(${TargetName} PRIVATE juce::juce_gui_extra ${TORCH_LIBRARIES})


set_target_properties(${TargetName} PROPERTIES
  CXX_STANDARD 14
  CXX_STANDARD_REQUIRED YES
)

if(MSVC)
    set_target_properties(${TargetName} PROPERTIES RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_CURRENT_BINARY_DIR})
    set_target_properties(${TargetName} PROPERTIES RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_CURRENT_BINARY_DIR})
    include(copy_torch_dlls)
    copy_torch_dlls(${TargetName})
endif(MSVC)
