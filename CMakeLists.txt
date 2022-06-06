cmake_minimum_required(VERSION 3.13)

project(test VERSION 1.0)

set(CMAKE_MESSAGE_LOG_LEVEL "WARNING")
set_property(GLOBAL PROPERTY RULE_MESSAGES OFF)

# Tell IDF build to link against this target.
set(IDF_EXECUTABLE_SRCS "${CMAKE_CURRENT_LIST_DIR}/src/main.c" "${CMAKE_CURRENT_LIST_DIR}/src/aws_dev_mode_key_provisioning.c")
set(IDF_PROJECT_EXECUTABLE test)

# Override the configurations for FreeRTOS.
include_directories(BEFORE amazon-freertos-configs)

# Add FreeRTOS as a subdirectory. AFR_BOARD tells which board to target.
set(AFR_BOARD espressif.esp32_devkitc CACHE INTERNAL "")
add_subdirectory(amazon-freertos)

# Link against the mqtt library so that we can use it. Dependencies are transitively
# linked.
target_link_libraries(test PRIVATE
    AFR::logging
    AFR::backoff_algorithm
    AFR::core_mqtt
    AFR::transport_interface_secure_sockets
    AFR::secure_sockets
    AFR::pkcs11_helpers
    AFR::pkcs11
    AFR::utils
    3rdparty::mbedtls
    AFR::core_mqtt_demo_dependencies
)
