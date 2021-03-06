
# Copyright 2010 Dean Michael Berris.
# Distributed under the Boost Software License, Version 1.0.
# (See accompanying file LICENSE_1_0.txt or copy at
# http://www.boost.org/LICENSE_1_0.txt)

include(sugar_add_executable)
include(sugar_add_gtest)
include(sugar_target_link_libraries)

if (Boost_FOUND)
    set ( TESTS
        request_incremental_parser_test
	request_linearize_test
        )
    foreach ( test ${TESTS} )
        if (${CMAKE_CXX_COMPILER_ID} MATCHES GNU)
            set_source_files_properties(${test}.cpp
                PROPERTIES COMPILE_FLAGS "-Wall")
        endif()
        sugar_add_executable(cpp-netlib-http-${test} ${test}.cpp)
        add_dependencies(cpp-netlib-http-${test}
            cppnetlib-uri)
        sugar_target_link_libraries(cpp-netlib-http-${test}
            ${Boost_LIBRARIES} ${CMAKE_THREAD_LIBS_INIT}
            cppnetlib-uri)
	if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
	  sugar_target_link_libraries(cpp-netlib-http-${test} rt)
	endif()
        sugar_add_gtest(cpp-netlib-http-${test} cpp-netlib-http-${test})
    endforeach (test)
    set ( TESTS
        client_constructor_test
        client_get_test
        client_get_different_port_test
        client_get_timeout_test
        client_get_streaming_test
        )
    foreach ( test ${TESTS} )
        if (${CMAKE_CXX_COMPILER_ID} MATCHES GNU)
            set_source_files_properties(${test}.cpp
                PROPERTIES COMPILE_FLAGS "-Wall")
        endif()
        if(MSVC)
          set_source_files_properties(
              ${test}.cpp PROPERTIES COMPILE_FLAGS "/bigobj"
          )
        endif()
        sugar_add_executable(cpp-netlib-http-${test} ${test}.cpp)
        add_dependencies(cpp-netlib-http-${test}
            cppnetlib-uri
            cppnetlib-client-connections)
        sugar_target_link_libraries(cpp-netlib-http-${test}
            ${Boost_LIBRARIES} ${CMAKE_THREAD_LIBS_INIT}
            cppnetlib-uri
            cppnetlib-client-connections)
        if (OPENSSL_FOUND)
            sugar_target_link_libraries(cpp-netlib-http-${test} ${OPENSSL_LIBRARIES})
        endif()
	if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
	  sugar_target_link_libraries(cpp-netlib-http-${test} rt)
	endif()
        sugar_add_gtest(cpp-netlib-http-${test} cpp-netlib-http-${test})
    endforeach (test)

    set ( SERVER_API_TESTS
        server_constructor_test
        server_async_run_stop_concurrency
        )
    foreach ( test ${SERVER_API_TESTS} )
        if (${CMAKE_CXX_COMPILER_ID} MATCHES GNU)
            set_source_files_properties(${test}.cpp
                PROPERTIES COMPILE_FLAGS "-Wall")
        endif()
        sugar_add_executable(cpp-netlib-http-${test} ${test}.cpp)
        add_dependencies(cpp-netlib-http-${test} cppnetlib-server-parsers)
        sugar_target_link_libraries(cpp-netlib-http-${test} ${Boost_LIBRARIES} ${CMAKE_THREAD_LIBS_INIT} cppnetlib-server-parsers)
	if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
	  sugar_target_link_libraries(cpp-netlib-http-${test} rt)
	endif()
        sugar_add_gtest(cpp-netlib-http-${test} cpp-netlib-http-${test})
    endforeach (test)

endif()
