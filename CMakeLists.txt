# ESP32 DOOM (prboom port)
cmake_minimum_required(VERSION 3.16)

# Build options
option(DOOM_STREAMING "Enable WiFi streaming of display" OFF)
option(DOOM_WIFI_AP "Use WiFi AP mode instead of station mode" OFF)

# Set WiFi credentials for streaming (change these!)
set(WIFI_SSID "YourWiFiSSID" CACHE STRING "WiFi SSID for streaming")
set(WIFI_PASS "YourWiFiPassword" CACHE STRING "WiFi password for streaming")

set(EXTRA_COMPONENT_DIRS 
    ${CMAKE_CURRENT_SOURCE_DIR}/prboom
    ${CMAKE_CURRENT_SOURCE_DIR}/prboom-esp32-compat
    ${CMAKE_CURRENT_SOURCE_DIR}/prboom-wad-tables
)

include($ENV{IDF_PATH}/tools/cmake/project.cmake)
project(esp32-doom)

# Add streaming compile definitions if enabled
if(DOOM_STREAMING)
    message(STATUS "DOOM Streaming ENABLED - WiFi SSID: ${WIFI_SSID}")
    add_compile_definitions(DOOM_STREAMING)
    add_compile_definitions(WIFI_SSID="${WIFI_SSID}")
    add_compile_definitions(WIFI_PASS="${WIFI_PASS}")
    if(DOOM_WIFI_AP)
        message(STATUS "DOOM WiFi AP mode ENABLED")
        add_compile_definitions(DOOM_WIFI_AP=1)
    endif()
endif()
