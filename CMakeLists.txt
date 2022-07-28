# ioLibrary_Driver

set(ENV{WIZNET_CHIP} W5100S)
set(WIZNET_CHIP $ENV{WIZNET_CHIP})
#set(env{WIZNET_CHIP W5100S PARENT_SCOPE)
message(STATUS "WIZNET_CHIP is = ${WIZNET_CHIP} ")
#if(${WIZNET_CHIP} STREQUAL W5100S)
#    add_definitions(-D_WIZCHIP_=W5100S)
#elseif(${WIZNET_CHIP} STREQUAL W5500)
#    add_definitions(-D_WIZCHIP_=W5500)
#else()
#    message(FATAL_ERROR "WIZNET_CHIP is wrong = ${WIZNET_CHIP}")
#endif()


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

# Ethernet
add_library(ETHERNET_FILES STATIC)

target_sources(ETHERNET_FILES PUBLIC
        ${WIZNET_DIR}/Ethernet/socket.c
        ${WIZNET_DIR}/Ethernet/wizchip_conf.c
        )

if(${WIZNET_CHIP} STREQUAL W5100S)
target_include_directories(ETHERNET_FILES INTERFACE
        ${WIZNET_DIR}/Ethernet
        ${WIZNET_DIR}/Ethernet/W5100S
        )

target_link_libraries(ETHERNET_FILES PUBLIC
        W5100S_FILES
        )
elseif(${WIZNET_CHIP} STREQUAL W5500)
target_include_directories(ETHERNET_FILES INTERFACE
        ${WIZNET_DIR}/Ethernet
        ${WIZNET_DIR}/Ethernet/W5500
        )

target_link_libraries(ETHERNET_FILES PUBLIC
        W5500_FILES
        )
endif()

if(${WIZNET_CHIP} STREQUAL W5100S)
add_library(W5100S_FILES STATIC)

target_sources(W5100S_FILES PUBLIC
        ${WIZNET_DIR}/Ethernet/W5100S/w5100s.c
        )

target_include_directories(W5100S_FILES INTERFACE
        ${WIZNET_DIR}/Ethernet
        ${WIZNET_DIR}/Ethernet/W5100S
        )

target_link_libraries(W5100S_FILES PRIVATE
        ETHERNET_FILES
        )
elseif(${WIZNET_CHIP} STREQUAL W5500)
add_library(W5500_FILES STATIC)

target_sources(W5500_FILES PUBLIC
        ${WIZNET_DIR}/Ethernet/W5500/w5500.c
        )

target_include_directories(W5500_FILES INTERFACE
        ${WIZNET_DIR}/Ethernet
        ${WIZNET_DIR}/Ethernet/W5500
        )

target_link_libraries(W5500_FILES PRIVATE
        ETHERNET_FILES
        )
endif()

# Loopback
add_library(LOOPBACK_FILES STATIC)

target_sources(LOOPBACK_FILES PUBLIC
        ${WIZNET_DIR}/Application/loopback/loopback.c
        )

target_include_directories(LOOPBACK_FILES PUBLIC
        ${WIZNET_DIR}/Ethernet
        ${WIZNET_DIR}/Application/loopback
        )

# DHCP
add_library(DHCP_FILES STATIC)

target_sources(DHCP_FILES PUBLIC
        ${WIZNET_DIR}/Internet/DHCP/dhcp.c
        )

target_include_directories(DHCP_FILES PUBLIC
        ${WIZNET_DIR}/Ethernet
        ${WIZNET_DIR}/Internet/DHCP
        )

# DNS
add_library(DNS_FILES STATIC)

target_sources(DNS_FILES PUBLIC
        ${WIZNET_DIR}/Internet/DNS/dns.c
        )

target_include_directories(DNS_FILES PUBLIC
        ${WIZNET_DIR}/Ethernet
        ${WIZNET_DIR}/Internet/DNS
        )

# FTP Client
add_library(FTPCLIENT_FILES STATIC)

target_sources(FTPCLIENT_FILES PUBLIC
        ${WIZNET_DIR}/Internet/FTPClient/ftpc.c
        )

target_include_directories(FTPCLIENT_FILES PUBLIC
        ${WIZNET_DIR}/Ethernet
        ${WIZNET_DIR}/Internet/FTPClient
        )

# FTP Server
add_library(FTPSERVER_FILES STATIC)

target_sources(FTPSERVER_FILES PUBLIC
        ${WIZNET_DIR}/Internet/FTPServer/ftpd.c
        )

target_include_directories(FTPSERVER_FILES PUBLIC
        ${WIZNET_DIR}/Ethernet
        ${WIZNET_DIR}/Internet/FTPServer
        )

# HTTP Server
add_library(HTTPSERVER_FILES STATIC)

target_sources(HTTPSERVER_FILES PUBLIC
        ${WIZNET_DIR}/Internet/httpServer/httpParser.c
        ${WIZNET_DIR}/Internet/httpServer/httpServer.c
        ${WIZNET_DIR}/Internet/httpServer/httpUtil.c
        )

target_include_directories(HTTPSERVER_FILES PUBLIC
        ${WIZNET_DIR}/Ethernet
        ${WIZNET_DIR}/Internet/httpServer
        )

# MQTT
add_library(MQTT_FILES STATIC)

target_sources(MQTT_FILES PUBLIC
        ${WIZNET_DIR}/Internet/MQTT/mqtt_interface.c
        ${WIZNET_DIR}/Internet/MQTT/MQTTClient.c
        ${WIZNET_DIR}/Internet/MQTT/MQTTPacket/src/MQTTConnectClient.c
        ${WIZNET_DIR}/Internet/MQTT/MQTTPacket/src/MQTTConnectServer.c
        ${WIZNET_DIR}/Internet/MQTT/MQTTPacket/src/MQTTDeserializePublish.c
        ${WIZNET_DIR}/Internet/MQTT/MQTTPacket/src/MQTTFormat.c
        ${WIZNET_DIR}/Internet/MQTT/MQTTPacket/src/MQTTPacket.c
        ${WIZNET_DIR}/Internet/MQTT/MQTTPacket/src/MQTTSerializePublish.c
        ${WIZNET_DIR}/Internet/MQTT/MQTTPacket/src/MQTTSubscribeClient.c
        ${WIZNET_DIR}/Internet/MQTT/MQTTPacket/src/MQTTSubscribeServer.c
        ${WIZNET_DIR}/Internet/MQTT/MQTTPacket/src/MQTTUnsubscribeClient.c
        ${WIZNET_DIR}/Internet/MQTT/MQTTPacket/src/MQTTUnsubscribeServer.c
        )

target_include_directories(MQTT_FILES PUBLIC
        ${WIZNET_DIR}/Ethernet
        ${WIZNET_DIR}/Internet/MQTT
        ${WIZNET_DIR}/Internet/MQTTPacket/src
        ) 

# SNTP
add_library(SNTP_FILES STATIC)

target_sources(SNTP_FILES PUBLIC
        ${WIZNET_DIR}/Internet/SNTP/sntp.c
        )

target_include_directories(SNTP_FILES PUBLIC
        ${WIZNET_DIR}/Ethernet
        ${WIZNET_DIR}/Internet/SNTP
        )

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
        ${DRIVER_DIR}/include
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
        ${DRIVER_DIR}/include
        )

target_link_libraries(TIMER_FILES PRIVATE
        pico_stdlib      
        )
