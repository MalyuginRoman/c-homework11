cmake_minimum_required(VERSION 3.7.2)

set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VERSION 0.0.${PATCH_VERSION})

project(join_server VERSION ${PROJECT_VERSION})
set(CMAKE_CXX_STANDARD 17)

message(STATUS "CMAKE_TOOLCHAIN_FILE: ${DCMAKE_TOOLCHAIN_FILE}")

configure_file(version.h.in version.h)

find_package(Boost REQUIRED)

if (Boost_FOUND)
    include_directories (${Boost_INCLUDE_DIRS})
    add_executable(join_server
    main.cpp
    command_parser.cpp
    table.cpp
    database.cpp)
    target_link_libraries(join_server ${Boost_LIBRARIES})
endif ()

set_target_properties(join_server PROPERTIES
        CXX_STANDARD 17
        CXX_STANDARD_REQUIRED ON
        )

if (MSVC)
    target_compile_options(join_server PRIVATE
        /W4
    )
else ()
    target_compile_options(join_server PRIVATE
        -Wall -Wextra -pedantic -Werror 
    )
endif()

install(TARGETS join_server RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT example@example.com)

include(CPack)
