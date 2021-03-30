cmake_minimum_required(VERSION 3.4)

add_subdirectory(pybind11)
add_subdirectory(JUCE)

project(juce-python VERSION 0.1.0)

juce_add_plugin(JucePythonExamplePlugin
    # VERSION ...                               # Set this if the plugin version is different to the project version
    # ICON_BIG ...                              # ICON_* arguments specify a path to an image file to use as an icon for the Standalone
    # ICON_SMALL ...
    # COMPANY_NAME ...                          # Specify the name of the plugin's author
    # IS_SYNTH TRUE/FALSE                       # Is this a synth or an effect?
    # NEEDS_MIDI_INPUT TRUE/FALSE               # Does the plugin need midi input?
    # NEEDS_MIDI_OUTPUT TRUE/FALSE              # Does the plugin need midi output?
    # IS_MIDI_EFFECT TRUE/FALSE                 # Is this plugin a MIDI effect?
    # EDITOR_WANTS_KEYBOARD_FOCUS TRUE/FALSE    # Does the editor need keyboard focus?
    # COPY_PLUGIN_AFTER_BUILD TRUE/FALSE        # Should the plugin be installed to a default location after building?
    PLUGIN_MANUFACTURER_CODE Juce               # A four-character manufacturer id with at least one upper-case character
    PLUGIN_CODE Dem0                            # A unique four-character plugin id with at least one upper-case character
    FORMATS AU VST3 Standalone                  # The formats to build. Other valid formats are: AAX Unity VST AU AUv3
    PRODUCT_NAME "JUCE Python Example")        # The name of the final executable, which can differ from the target name

   
set(JUCE_MODULE_LIBS 
juce::juce_audio_basics
juce::juce_audio_devices
juce::juce_audio_formats
juce::juce_audio_plugin_client
juce::juce_audio_processors
juce::juce_audio_utils
juce::juce_core
juce::juce_cryptography
juce::juce_data_structures
juce::juce_dsp
juce::juce_events
juce::juce_graphics
juce::juce_gui_basics
juce::juce_gui_extra
juce::juce_opengl
juce::juce_product_unlocking)

juce_generate_juce_header(JucePythonExamplePlugin)

target_sources(JucePythonExamplePlugin PRIVATE
    PluginEditor.cpp
    PluginProcessor.cpp)

target_compile_definitions(JucePythonExamplePlugin
PUBLIC
# JUCE_WEB_BROWSER and JUCE_USE_CURL would be on by default, but you might not need them.
JUCE_WEB_BROWSER=0  # If you remove this, add `NEEDS_WEB_BROWSER TRUE` to the `juce_add_plugin` call
JUCE_USE_CURL=0     # If you remove this, add `NEEDS_CURL TRUE` to the `juce_add_plugin` call
JUCE_VST3_CAN_REPLACE_VST2=0)

target_link_libraries(JucePythonExamplePlugin PRIVATE
    # AudioPluginData           # If we'd created a binary data target, we'd link to it here
    juce::juce_audio_utils
    pybind11::embed
    ${JUCE_MODULE_LIBS}
    )

    # for clang-tidy(this enable to find system header files).
if(CMAKE_EXPORT_COMPILE_COMMANDS)
set(CMAKE_CXX_STANDARD_INCLUDE_DIRECTORIES ${CMAKE_CXX_IMPLICIT_INCLUDE_DIRECTORIES})
endif()