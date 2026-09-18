cmake_minimum_required(VERSION 4.0)

project(cgltf
   VERSION         "1.0"
   DESCRIPTION     "💠 Single-file glTF 2.0 loader and writer written in C99"
   HOMEPAGE_URL    "https://github.com/jkuhlmann/cgltf"
   LANGUAGES       "C"
)

include(GNUInstallDirs)
set(CGLTF_LIB_NAME cgltf)

set(CGLTF_PUBLIC_HEADERS cgltf.h cgltf_write.h)
list(TRANSFORM CGLTF_PUBLIC_HEADERS PREPEND include/)

add_library(cgltf
   source/cgltf.c
   source/cgltf_write.c
)

target_include_directories(cgltf
   PUBLIC include
)

configure_file(
   ${CMAKE_CURRENT_SOURCE_DIR}/cgltf.pc.in
   ${CMAKE_CURRENT_BINARY_DIR}/cgltf.pc @ONLY
)

install(
   FILES ${CGLTF_PUBLIC_HEADERS}
   DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/cgltf
)


install(
   FILES ${CMAKE_CURRENT_BINARY_DIR}/cgltf.pc
   DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig
)

install(
   TARGETS cgltf
   EXPORT cgltf_targets
   LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
   ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
   RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
