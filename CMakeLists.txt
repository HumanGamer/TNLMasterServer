cmake_minimum_required(VERSION 3.18)

if (MSVC)
    #Use solution folders. 
    set_property(GLOBAL PROPERTY USE_FOLDERS ON)
endif()

# Remove MinSizeRel and RelWithDebInfo configurations
set(CMAKE_CONFIGURATION_TYPES "Debug;Release" CACHE STRING "" FORCE)

set(ENABLE_LOGGING true CACHE BOOL "Enable Logging")

project(TorqueMasterServer)

add_subdirectory(src)
