cmake_minimum_required(VERSION 4.0)

project(cgltf
   VERSION         "1.0"
   DESCRIPTION     "💠 Single-file glTF 2.0 loader and writer written in C99"
   HOMEPAGE_URL    "https://github.com/jkuhlmann/cgltf"
   LANGUAGES       "C"
)

include(GNUInstallDirs)
set(CGLTF_PUBLIC_HEADERS cgltf.h cgltf_write.h)
set(CGLTF_SOURCE_FILES cgltf.c cgltf_write.c)

list(TRANSFORM CGLTF_PUBLIC_HEADERS PREPEND include/)
list(TRANSFORM CGLTF_SOURCE_FILES PREPEND source/)

add_library(cgltf "${CGLTF_SOURCE_FILES}")
add_library(vendor::cgltf ALIAS cgltf)

target_include_directories(cgltf PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/include> $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/cgltf.pc.in ${CMAKE_CURRENT_BINARY_DIR}/cgltf.pc @ONLY)
install(FILES ${CGLTF_PUBLIC_HEADERS} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/cgltf)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/cgltf.pc DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig)

install(
   TARGETS cgltf
   EXPORT cgltfConfig
   LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
   ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
   RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

install(
   EXPORT cgltfConfig
   NAMESPACE vendor::
   DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/cgltf
)
