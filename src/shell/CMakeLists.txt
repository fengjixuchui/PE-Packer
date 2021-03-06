enable_language(ASM_MASM)

add_library(shell "")

target_compile_definitions(shell PRIVATE -D_UNICODE -DUNICODE)

set(HEADER_PATH "${CMAKE_SOURCE_DIR}/include/shell")

target_include_directories(shell PUBLIC ${HEADER_PATH})

target_sources(shell
    PRIVATE
        ${CMAKE_CURRENT_LIST_DIR}/install_shell.c
        ${CMAKE_CURRENT_LIST_DIR}/entry_export.h
    PUBLIC
        ${HEADER_PATH}/install_shell.h
)

if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    message(FATAL_ERROR "[!] Must configuring on/for x86 platform")
else()
    target_sources(shell
        PRIVATE
            ${CMAKE_CURRENT_LIST_DIR}/entry_x86.asm
            ${CMAKE_CURRENT_LIST_DIR}/include_x86/windows.inc
            ${CMAKE_CURRENT_LIST_DIR}/include_x86/winextra.inc
    )

    set_source_files_properties(entry_x86.asm PROPERTIES COMPILE_FLAGS "/c /coff")
    set_source_files_properties(entry_x86.asm PROPERTIES LINKER_FLAGS "/SAFESEH:NO")
endif()