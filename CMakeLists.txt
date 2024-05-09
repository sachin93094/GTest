cmake_minimum_required(VERSION 3.0)

# Define your project
project(ProjectName)

# Locate GTest
find_package(GTest REQUIRED)
include_directories(${GTEST_INCLUDE_DIRS})

# Link runTests with what we want to test and the GTest and pthread library
add_executable(runTests tests.cpp)
target_link_libraries(runTests ${GTEST_LIBRARIES} pthread)

# Set the output directory for the executable
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR}/bin)

# Enable XML output for Google Test
target_compile_definitions(runTests PRIVATE GTEST_OUTPUT_XML)

# Optionally, you can set the output directory for the XML file
# set(XML_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/xml)

# Add a custom command to run the tests and generate XML output
add_custom_command(
    TARGET runTests
    POST_BUILD
    COMMAND ${CMAKE_BINARY_DIR}/bin/runTests --gtest_output=xml:${XML_OUTPUT_DIRECTORY}/test_results.xml
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/bin
    COMMENT "Running tests and generating XML output"
)
