cmake_minimum_required(VERSION 3.18)

include_directories(../tnl)

set(MASTER_SRC
	# Sources
	main.cpp
	masterInterface.cpp
	config.cpp
	
	# Headers
	masterInterface.h
)

# Filter sources
source_group("Source" FILES ${MASTER_SRC})

set(SOURCE_FILES
	${MASTER_SRC}
)

add_executable(master ${SOURCE_FILES})
add_dependencies(master tnl libtomcrypt)
target_link_libraries(master tnl libtomcrypt)
target_compile_definitions(master PUBLIC "$<$<CONFIG:DEBUG>:TNL_DEBUG>")

if (${ENABLE_LOGGING})
	add_definitions(-DTNL_ENABLE_LOGGING)
endif()

if (WIN32)
	target_link_libraries(master wsock32)
    add_definitions(-D_WIN32_WINNT=0x0A00 -DWINVER=0x0A00)
endif()

set_target_properties(master
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/run"
    RUNTIME_OUTPUT_DIRECTORY_DEBUG "${CMAKE_SOURCE_DIR}/run"
    RUNTIME_OUTPUT_DIRECTORY_RELEASE "${CMAKE_SOURCE_DIR}/run"
    RUNTIME_OUTPUT_NAME "master"
    RUNTIME_OUTPUT_NAME_DEBUG "masterd"
    RUNTIME_OUTPUT_NAME_RELEASE "master"
)
