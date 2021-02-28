find_package(PkgConfig REQUIRED)

add_library( blockvault_client_plugin
             blockvault_client_plugin.cpp)
target_link_libraries( blockvault_client_plugin appbase eosio_chain)
target_include_directories( blockvault_client_plugin PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include" )

find_package(PostgreSQL REQUIRED)
find_package(libpqxx CONFIG REQUIRED)

add_library(blockvault postgres_backend.cpp)
target_include_directories(blockvault PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_link_libraries(blockvault PUBLIC PostgreSQL::PostgreSQL  
                                        ${PostgreSQL_LIBRARY_DIRS}/libpgcommon.a 
                                        ${PostgreSQL_LIBRARY_DIRS}/libpgport.a 
                                        libpqxx::pqxx 
                                        eosio_chain)

target_link_libraries( blockvault_client_plugin blockvault)
target_compile_definitions(blockvault_client_plugin PRIVATE HAS_PQXX)

add_subdirectory(tests)
