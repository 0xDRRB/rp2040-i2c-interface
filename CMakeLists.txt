cmake_minimum_required(VERSION 3.12)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
include($ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake)
include($ENV{PICO_EXTRAS_PATH}/external/pico_extras_import.cmake)

# Project name
set(NAME i2c_adapter)

# Board type
set(PICO_BOARD pimoroni_tiny2040)

project(${NAME} C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

pico_sdk_init()

add_executable(${NAME}
    main.c
    usb_descriptors.c
)

target_include_directories(${NAME} PUBLIC
        ${CMAKE_CURRENT_LIST_DIR})

target_link_libraries(${NAME}
    pico_stdlib
    pico_unique_id
    hardware_watchdog
    hardware_flash
    hardware_uart
    hardware_pio
    hardware_pwm
    hardware_adc
    hardware_i2c
    tinyusb_device
    tinyusb_board
    cmsis_core
)

pico_add_extra_outputs(${NAME})

add_custom_target(upload
    COMMAND picotool load -v -x ${CMAKE_PROJECT_NAME}.bin
)

set_directory_properties(PROPERTIES
   ADDITIONAL_MAKE_CLEAN_FILES
   "${CMAKE_PROJECT_NAME}.bin;${CMAKE_PROJECT_NAME}.dis;\
${CMAKE_PROJECT_NAME}.elf.map;${CMAKE_PROJECT_NAME}.hex;\
${CMAKE_PROJECT_NAME}.uf2"
)
