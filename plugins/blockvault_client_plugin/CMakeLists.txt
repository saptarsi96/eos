
add_library( blockvault_client_plugin
             blockvault_client_plugin.cpp)
target_link_libraries( blockvault_client_plugin appbase eosio_chain)
target_include_directories( blockvault_client_plugin PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include" )

find_package(PostgreSQL REQUIRED)
find_package(libpqxx CONFIG REQUIRED)

string(REPLACE "libpq.a" "libpgcommon.a" LIBPGCOMMON "${PostgreSQL_LIBRARIES}")
string(REPLACE "libpq.a" "libpgport.a" LIBPGPORT "${PostgreSQL_LIBRARIES}")
target_link_libraries(PostgreSQL::PostgreSQL INTERFACE ${LIBPGCOMMON} ${LIBPGPORT})
target_link_libraries(libpqxx::pqxx INTERFACE PostgreSQL::PostgreSQL)

add_library(blockvault postgres_backend.cpp)
target_include_directories(blockvault PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_link_libraries(blockvault PUBLIC libpqxx::pqxx eosio_chain)

target_link_libraries( blockvault_client_plugin blockvault)
target_compile_definitions(blockvault_client_plugin PRIVATE HAS_PQXX)

add_subdirectory(tests)
