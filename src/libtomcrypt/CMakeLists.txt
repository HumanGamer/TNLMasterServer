cmake_minimum_required(VERSION 3.18)

include_directories(../libtomcrypt/headers)

set(LIBTOMCRYPT_SRC
	# Sources
	ciphers/aes/aes.c
    hashes/helper/hash_memory.c
    hashes/helper/hash_memory_multi.c
    hashes/md2.c
    hashes/md4.c
    hashes/md5.c
    hashes/sha1.c
    hashes/sha2/sha256.c
    hashes/sha2/sha512.c
    misc/base64/base64_decode.c
    misc/base64/base64_encode.c
    misc/burn_stack.c
    misc/crypt/crypt.c
    misc/crypt/crypt_argchk.c
    misc/crypt/crypt_cipher_descriptor.c
    misc/crypt/crypt_cipher_is_valid.c
    misc/crypt/crypt_find_cipher.c
    misc/crypt/crypt_find_cipher_any.c
    misc/crypt/crypt_find_cipher_id.c
    misc/crypt/crypt_find_hash.c
    misc/crypt/crypt_find_hash_any.c
    misc/crypt/crypt_find_hash_id.c
    misc/crypt/crypt_find_prng.c
    misc/crypt/crypt_hash_descriptor.c
    misc/crypt/crypt_hash_is_valid.c
    misc/crypt/crypt_prng_descriptor.c
    misc/crypt/crypt_prng_is_valid.c
    misc/crypt/crypt_register_cipher.c
    misc/crypt/crypt_register_hash.c
    misc/crypt/crypt_register_prng.c
    misc/crypt/crypt_unregister_cipher.c
    misc/crypt/crypt_unregister_hash.c
    misc/crypt/crypt_unregister_prng.c
    misc/error_to_string.c
    misc/mpi/is_prime.c
    misc/mpi/mpi.c
    misc/mpi/mpi_to_ltc_error.c
    misc/mpi/rand_prime.c
    misc/zeromem.c
    modes/ctr/ctr_decrypt.c
    modes/ctr/ctr_done.c
    modes/ctr/ctr_encrypt.c
    modes/ctr/ctr_getiv.c
    modes/ctr/ctr_setiv.c
    modes/ctr/ctr_start.c
    pk/ecc/ecc.c
    pk/packet_store_header.c
    pk/packet_valid_header.c
    prngs/yarrow.c
	
	# Headers
    headers/ltc_tommath.h
    headers/tomcrypt.h
    headers/tomcrypt_argchk.h
    headers/tomcrypt_cfg.h
    headers/tomcrypt_cipher.h
    headers/tomcrypt_custom.h
    headers/tomcrypt_hash.h
    headers/tomcrypt_mac.h
    headers/tomcrypt_macros.h
    headers/tomcrypt_misc.h
    headers/tomcrypt_pk.h
    headers/tomcrypt_pkcs.h
    headers/tomcrypt_prng.h
    headers/tommath_class.h
    headers/tommath_superclass.h
)

# Filter sources
source_group("Source" FILES ${LIBTOMCRYPT_SRC})

set(SOURCE_FILES
	${LIBTOMCRYPT_SRC}
)

add_library(libtomcrypt STATIC ${SOURCE_FILES})
target_compile_definitions(libtomcrypt PUBLIC "$<$<CONFIG:DEBUG>:TNL_DEBUG>")

if (${ENABLE_LOGGING})
	add_definitions(-DTNL_ENABLE_LOGGING)
endif()

if (WIN32)
    add_definitions(-D_WIN32_WINNT=0x0A00 -DWINVER=0x0A00)
endif()

set_target_properties(libtomcrypt
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/run"
    RUNTIME_OUTPUT_DIRECTORY_DEBUG "${CMAKE_SOURCE_DIR}/run"
    RUNTIME_OUTPUT_DIRECTORY_RELEASE "${CMAKE_SOURCE_DIR}/run"
    RUNTIME_OUTPUT_NAME "libtomcrypt"
    RUNTIME_OUTPUT_NAME_DEBUG "libtomcryptd"
    RUNTIME_OUTPUT_NAME_RELEASE "libtomcrypt"
)
