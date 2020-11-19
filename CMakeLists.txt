cmake_minimum_required(VERSION 3.4)

add_subdirectory(pybind11)
project(juce-python VERSION 0.1.0)

add_executable(juce-python main.cpp)

target_link_libraries(juce-python PRIVATE pybind11::embed)