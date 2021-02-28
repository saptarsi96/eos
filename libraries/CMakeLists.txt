
find_package(RocksDB CONFIG REQUIRED)

add_subdirectory( fc )
add_subdirectory( builtins )

# Suppress warnings on 3rdParty Library
add_definitions( -w )
add_subdirectory( softfloat )
remove_definitions( -w )

add_subdirectory( chainbase )
add_subdirectory( wasm-jit )
add_subdirectory( appbase )
add_subdirectory( chain )
add_subdirectory( testing )
add_subdirectory( version )
add_subdirectory( state_history )
add_subdirectory( abieos )

add_subdirectory( chain_kv )
add_subdirectory( se-helpers )

set(USE_EXISTING_SOFTFLOAT ON CACHE BOOL "use pre-exisiting softfloat lib")
set(ENABLE_TOOLS OFF CACHE BOOL "Build tools")
set(ENABLE_TESTS OFF CACHE BOOL "Build tests")
set(ENABLE_ADDRESS_SANITIZER OFF CACHE BOOL "Use address sanitizer")
set(ENABLE_UNDEFINED_BEHAVIOR_SANITIZER OFF CACHE BOOL "Use UB sanitizer")
set(ENABLE_PROFILE OFF CACHE BOOL "Enable for profile builds")
if(eos-vm IN_LIST EOSIO_WASM_RUNTIMES OR eos-vm-jit IN_LIST EOSIO_WASM_RUNTIMES)
add_subdirectory( eos-vm )
endif()
