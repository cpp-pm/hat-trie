cmake_minimum_required(VERSION 3.1)

project(tsl_hat_trie LANGUAGES CXX VERSION 0.6.0)

# Introduce variables:
# * CMAKE_INSTALL_LIBDIR
# * CMAKE_INSTALL_BINDIR
# * CMAKE_INSTALL_INCLUDEDIR
include(GNUInstallDirs)

add_library(tsl_hat_trie INTERFACE)
# Use tsl::hat_trie as target, more consistent with other libraries conventions (Boost, Qt, ...)
add_library(tsl::hat_trie ALIAS tsl_hat_trie)

target_include_directories(tsl_hat_trie INTERFACE
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
    "$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>")
set(TSL_HAT_TRIE_HDR_LIST
    "${CMAKE_CURRENT_SOURCE_DIR}/include/tsl/array-hash/array_growth_policy.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/include/tsl/array-hash/array_hash.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/include/tsl/array-hash/array_map.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/include/tsl/array-hash/array_set.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/include/tsl/htrie_hash.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/include/tsl/htrie_map.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/include/tsl/htrie_set.h"
)
target_sources(tsl_hat_trie INTERFACE)


#if(${CMAKE_VERSION} VERSION_GREATER "3.7")
    # Only available since version 3.8
    #target_compile_features(tsl_hat_trie INTERFACE cxx_std_11)
#endif()

set(GENERATED_DIR "${CMAKE_CURRENT_BINARY_DIR}/generated")
set(VERSION_CONFIG "${GENERATED_DIR}/${PROJECT_NAME}ConfigVersion.cmake")
set(PROJECT_CONFIG "${GENERATED_DIR}/${PROJECT_NAME}Config.cmake")

set(CONFIG_INSTALL_DIR "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}")

set(TARGETS_EXPORT_NAME "${PROJECT_NAME}Targets")
set(NAMESPACE "${PROJECT_NAME}::")

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${VERSION_CONFIG}" COMPATIBILITY ExactVersion
)

configure_package_config_file(
    "cmake/Config.cmake.in"
    "${PROJECT_CONFIG}"
    INSTALL_DESTINATION "${CONFIG_INSTALL_DIR}"
)

install(
    TARGETS tsl_hat_trie
    EXPORT "${TARGETS_EXPORT_NAME}"
    PUBLIC_HEADER DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}"
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
    LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
    ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
)

install(
    DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/include/tsl"
    DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}"
)

install(
    FILES "${PROJECT_CONFIG}" "${VERSION_CONFIG}"
    DESTINATION "${CONFIG_INSTALL_DIR}"
)

install(
    EXPORT "${TARGETS_EXPORT_NAME}"
    NAMESPACE "${NAMESPACE}"
    DESTINATION "${CONFIG_INSTALL_DIR}"
)
