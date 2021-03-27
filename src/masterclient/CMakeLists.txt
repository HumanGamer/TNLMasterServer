cmake_minimum_required(VERSION 3.10)

include_directories(../tnl)

set(MASTERCLIENT_SRC
	# Sources
	main.cpp
)

set(MASTER_SRC
	# Sources
	../master/masterInterface.cpp
	
	# Headers
	../master/masterInterface.h
)

# Filter sources
source_group("Source" FILES ${MASTERCLIENT_SRC})
source_group("Includes" FILES ${MASTER_SRC})

set(SOURCE_FILES
	${MASTERCLIENT_SRC}
	${MASTER_SRC}
)

add_executable(masterclient ${SOURCE_FILES})
add_dependencies(masterclient tnl libtomcrypt)
target_link_libraries(masterclient tnl libtomcrypt)
target_compile_definitions(masterclient PUBLIC "$<$<CONFIG:DEBUG>:TNL_DEBUG>")

if (${ENABLE_LOGGING})
	add_definitions(-DTNL_ENABLE_LOGGING)
endif()

if (WIN32)
	target_link_libraries(masterclient wsock32)
    add_definitions(-D_WIN32_WINNT=0x0A00 -DWINVER=0x0A00)
endif()

set_target_properties(masterclient
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/run"
    RUNTIME_OUTPUT_DIRECTORY_DEBUG "${CMAKE_SOURCE_DIR}/run"
    RUNTIME_OUTPUT_DIRECTORY_RELEASE "${CMAKE_SOURCE_DIR}/run"
    RUNTIME_OUTPUT_NAME "masterclient"
    RUNTIME_OUTPUT_NAME_DEBUG "masterclientd"
    RUNTIME_OUTPUT_NAME_RELEASE "masterclient"
)
