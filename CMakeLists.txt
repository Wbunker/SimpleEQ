cmake_minimum_required(VERSION 3.15)

# Set the project name and version
project(SimpleEQ VERSION 1.0)

# Set the JUCE directory
set(JUCE_DIR /home/will/JUCE)

# Find GTK3 package
find_package(PkgConfig REQUIRED)
pkg_check_modules(GTK3 REQUIRED gtk+-3.0)
pkg_check_modules(WEBKIT2GTK REQUIRED webkit2gtk-4.0)
pkg_check_modules(SOUP REQUIRED libsoup-2.4)
pkg_check_modules(JAVASCRIPTCOREGTK REQUIRED javascriptcoregtk-4.0)
find_package(CURL REQUIRED)

# Add the JUCE modules
add_subdirectory(${JUCE_DIR} ${CMAKE_BINARY_DIR}/JUCE)

# Include GTK3 directories
include_directories(${GTK3_INCLUDE_DIRS})
include_directories(${WEBKIT2GTK_INCLUDE_DIRS})
include_directories(${SOUP_INCLUDE_DIRS})
include_directories(${JAVASCRIPTCOREGTK_INCLUDE_DIRS})
include_directories(${CURL_INCLUDE_DIRS})

# Create the executable
juce_add_plugin(SimpleEQ
    COMPANY_NAME "Bunker_Consulting"
    PLUGIN_MANUFACTURER_CODE Bnkr
    PLUGIN_CODE EQ01
    FORMATS AU VST3 Standalone
    PRODUCT_NAME "SimpleEQ"
)

target_sources(SimpleEQ PRIVATE
    Source/PluginProcessor.cpp
    Source/PluginEditor.cpp
    # Add other source files here
)

# Include JuceLibraryCode directory
target_include_directories(SimpleEQ PRIVATE ${CMAKE_SOURCE_DIR}/JuceLibraryCode)

# Define JUCE_VST3_CAN_REPLACE_VST2
add_definitions(-DJUCE_VST3_CAN_REPLACE_VST2=0)

# Link the JUCE modules
target_link_libraries(SimpleEQ PRIVATE
    juce::juce_audio_utils
    juce::juce_dsp
    ${GTK3_LIBRARIES}
    ${WEBKIT2GTK_LIBRARIES}
    ${SOUP_LIBRARIES}
    ${JAVASCRIPTCOREGTK_LIBRARIES}
    ${CURL_LIBRARIES}
)

# Set the output directory for the standalone executable
set_target_properties(SimpleEQ_Standalone PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/Standalone
)