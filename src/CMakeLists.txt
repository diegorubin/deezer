find_package(PkgConfig)

pkg_check_modules(GTK REQUIRED gtk+-3.0)
pkg_check_modules(GLIB_REQUIRED glib-2.0)

add_definitions(${GLIB_CFLAGS} ${GLIB_CFLAGS_OTHER})
add_definitions(${GTK_CFLAGS} ${GTK_CFLAGS_OTHER})

link_libraries(${GLIB_LIBRARIES})
link_libraries(${GTK_LIBRARIES})

link_directories(${GLIB_LIBRARY_DIRS})
link_directories(${GTK_LIBRARY_DIRS})

vala_precompile(VALA_C
  deezer.vala
PACKAGES
  gtk+-3.0
  posix
GENERATE_VAPI
  deezer
GENERATE_HEADER
  deezer
)

add_executable(deezer ${VALA_C})
add_library(deezer-clib ${VALA_C})

