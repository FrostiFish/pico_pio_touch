# == DO NOT EDIT THE FOLLOWING LINES for the Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.1.1)
set(toolchainVersion 14_2_Rel1)
set(picotoolVersion 2.1.1)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================
set(PICO_BOARD pico CACHE STRING "Board type")

# What CMake to start at
cmake_minimum_required(VERSION 3.12)
 
# Include the subsidiary .cmake file to get the SDK
if (EXISTS ${USERHOME}/.pico-sdk/examples/pico_sdk_import.cmake)
    include(${USERHOME}/.pico-sdk/examples/pico_sdk_import.cmake)
endif()

# Set the name and version of the project
project(pico_pio_touch C CXX ASM)

# Initialise the SDK
pico_sdk_init()

# Link the Project to a source file (step 4.6)
add_executable(${PROJECT_NAME} source.c)

# Create C header file with the name <pio program>.pio.h
pico_generate_pio_header(${PROJECT_NAME} ${CMAKE_CURRENT_LIST_DIR}/touch.pio)
 
# Link to pico_stdlib (gpio, time, etc. functions) and other libraries required by the executable
target_link_libraries(${PROJECT_NAME} 
    pico_stdlib
    hardware_pio
)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
 
# Enable USB, UART output
pico_enable_stdio_usb(${PROJECT_NAME} 1)
pico_enable_stdio_uart(${PROJECT_NAME} 0)

# Enable extra outputs (SWD?)
pico_add_extra_outputs(${PROJECT_NAME})