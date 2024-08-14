cmake_minimum_required(VERSION 3.15)

# Set the project name and version
project(SimpleEQ VERSION 1.0)

# Set the JUCE directory
set(JUCE_DIR 'home/will/JUCE')

# Add the JUCE modules
add_subdirectory(${JUCE_DIR} JUCE)

# Create the executable
juce_add_plugin(SimpleEQ
    COMPANY_NAME "YourCompany"
    PLUGIN_MANUFACTURER_CODE JucePlugin_ManufacturerCode
    PLUGIN_CODE JucePlugin_PluginCode
    FORMATS AU VST3 Standalone
    PRODUCT_NAME "SimpleEQ"
)

# Link the JUCE modules
target_link_libraries(SimpleEQ PRIVATE
    juce::juce_audio_utils
    juce::juce_dsp
)