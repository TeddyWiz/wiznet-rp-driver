# ioLibrary_Driver
set(WIZNET_CHIP W5100S)

if(${WIZNET_CHIP} STREQUAL W5100S)
    add_definitions(-D_WIZCHIP_=W5100S)
elseif(${WIZNET_CHIP} STREQUAL W5500)
    add_definitions(-D_WIZCHIP_=W5500)
else()
    message(FATAL_ERROR "WIZNET_CHIP is wrong = ${WIZNET_CHIP}")
endif()

# Set the project root directory if it's not already defined, as may happen if
# the tests folder is included directly by a parent project, without including
# the top level CMakeLists.txt.
if(NOT DEFINED WIZNET_DIR)
    set(WIZNET_DIR ${PICO_SDK_PATH}/lib/ioLibrary_Driver)
    message(STATUS "WIZNET_DIR = ${WIZNET_DIR}")
endif()

if(NOT DEFINED DRIVER_DIR)
    set(DRIVER_DIR ${CMAKE_CURRENT_LIST_DIR})
    message(STATUS "DRIVER_DIR = ${DRIVER_DIR}")
endif()

add_library(IOLIBRARY_FILES STATIC)

target_sources(IOLIBRARY_FILES PUBLIC
        ${DRIVER_DIR}/w5x00_spi.c
        ${DRIVER_DIR}/w5x00_gpio_irq.c
        )

target_include_directories(IOLIBRARY_FILES PUBLIC
        ${WIZNET_DIR}/Ethernet
        ${DRIVER_DIR}/include
        )

target_link_libraries(IOLIBRARY_FILES PRIVATE
        pico_stdlib
        hardware_spi
        hardware_dma
        hardware_clocks
        )

# lwip
add_library(LWIP_FILES STATIC)

target_sources(LWIP_FILES PUBLIC
        ${DRIVER_DIR}/w5x00_lwip.c
        )

target_include_directories(LWIP_FILES PUBLIC
        ${WIZNET_DIR}/Ethernet
        ${DRIVER_DIR}
        )

target_link_libraries(LWIP_FILES PUBLIC
        ETHERNET_FILES
        pico_lwip
        pico_lwip_nosys
		pico_time
		hardware_irq
        )

# timer
add_library(TIMER_FILES STATIC)

target_sources(TIMER_FILES PUBLIC
        ${DRIVER_DIR}/timer.c
        )

target_include_directories(TIMER_FILES PUBLIC
        ${DRIVER_DIR}/include/
        )

target_link_libraries(TIMER_FILES PRIVATE
        pico_stdlib      
        )
