cmake_minimum_required(VERSION 3.10)

include_directories(../libtomcrypt/headers)

set(TNL_SRC
	# Sources
	assert.cpp
    asymmetricKey.cpp
    bitStream.cpp
    byteBuffer.cpp
    certificate.cpp
    clientPuzzle.cpp
    connectionStringTable.cpp
    dataChunker.cpp
    eventConnection.cpp
    ghostConnection.cpp
    huffmanStringProcessor.cpp
    journal.cpp
    log.cpp
    netBase.cpp
    netConnection.cpp
    netInterface.cpp
    netObject.cpp
    netStringTable.cpp
    platform.cpp
    random.cpp
    rpc.cpp
    symmetricCipher.cpp
    thread.cpp
    tnlMethodDispatch.cpp
    udp.cpp
    vector.cpp
	
	# Headers
    tnl.h
    tnlAssert.h
    tnlAsymmetricKey.h
    tnlBitSet.h
    tnlBitStream.h
    tnlByteBuffer.h
    tnlCertificate.h
    tnlClientPuzzle.h
    tnlConnectionStringTable.h
    tnlDataChunker.h
    tnlEndian.h
    tnlEventConnection.h
    tnlGhostConnection.h
    tnlHuffmanStringProcessor.h
    tnlJournal.h
    tnlLog.h
    tnlMethodDispatch.h
    tnlNetBase.h
    tnlNetConnection.h
    tnlNetEvent.h
    tnlNetInterface.h
    tnlNetObject.h
    tnlNetStringTable.h
    tnlNonce.h
    tnlPlatform.h
    tnlRandom.h
    tnlRPC.h
    tnlSymmetricCipher.h
    tnlThread.h
    tnlTypes.h
    tnlUDP.h
    tnlVector.h
)

# Filter sources
source_group("Source" FILES ${TNL_SRC})

set(SOURCE_FILES
	${TNL_SRC}
)

add_library(tnl STATIC ${SOURCE_FILES})
add_dependencies(tnl libtomcrypt)
target_link_libraries(tnl libtomcrypt)
target_compile_definitions(tnl PUBLIC "$<$<CONFIG:DEBUG>:TNL_DEBUG>")

if (${ENABLE_LOGGING})
	add_definitions(-DTNL_ENABLE_LOGGING)
endif()

if (WIN32)
	target_link_libraries(tnl wsock32)
    add_definitions(-D_WIN32_WINNT=0x0A00 -DWINVER=0x0A00)
endif()

set_target_properties(tnl
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/run"
    RUNTIME_OUTPUT_DIRECTORY_DEBUG "${CMAKE_SOURCE_DIR}/run"
    RUNTIME_OUTPUT_DIRECTORY_RELEASE "${CMAKE_SOURCE_DIR}/run"
    RUNTIME_OUTPUT_NAME "tnl"
    RUNTIME_OUTPUT_NAME_DEBUG "tnld"
    RUNTIME_OUTPUT_NAME_RELEASE "tnl"
)
