cmake_minimum_required(VERSION 3.16)

project(boost-trim-test
    LANGUAGES CXX
)

add_subdirectory(boost-trim)

if(WIN32)
    set(OPENSSL_ROOT_DIR C:/tools/OpenSSL-Win64)
endif()
find_package(OpenSSL REQUIRED)

macro(add_exe tgt std)
    add_executable(${tgt} ${tgt}.cpp)
    target_link_libraries(${tgt}
        Boost::boost
        OpenSSL::SSL
        OpenSSL::Crypto
    )
    set_target_properties(${tgt} PROPERTIES
        CXX_STANDARD ${std}
        CXX_STANDARD_REQUIRED ON
        CXX_EXTENSIONS OFF
    )
    if(MSVC)
        get_source_file_property(props ${tgt}.cpp COMPILE_FLAGS)
        if(${props} STREQUAL "NOTFOUND")
            set(props)
        endif()
        set_source_files_properties(${tgt}.cpp PROPERTIES COMPILE_FLAGS "${props} /bigobj")
    endif()
endmacro()

add_exe(websockect_server_async_ssl 17)
#add_exe(websocket_server_coro_ssl 20)
