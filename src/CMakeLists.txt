find_package(PkgConfig)

pkg_check_modules(GTK REQUIRED gtk+-3.0)
pkg_check_modules(GLIB_REQUIRED glib-2.0)
pkg_check_modules(WEBKITGTK_REQUIRED webkitgtk-3.0)

add_definitions(${GLIB_CFLAGS} ${GLIB_CFLAGS_OTHER})
add_definitions(${GTK_CFLAGS} ${GTK_CFLAGS_OTHER})
add_definitions(${WEBKITGTK_CFLAGS} ${WEBKITGTK_CFLAGS_OTHER})

link_libraries(${GLIB_LIBRARIES})
link_libraries(${GTK_LIBRARIES})
link_libraries(${WEBKITGTK_LIBRARIES})

link_directories(${GLIB_LIBRARY_DIRS})
link_directories(${GTK_LIBRARY_DIRS})
link_directories(${WEBKITGTK_LIBRARY_DIRS})

set(VALA_SRC
  views/window_main.vala
  deezer.vala
)

vala_precompile(VALA_C
  ${VALA_SRC}
PACKAGES
  gtk+-3.0
  webkitgtk-3.0
  posix
GENERATE_VAPI
  deezer
GENERATE_HEADER
  deezer
)

add_executable(deezer ${VALA_C})
add_library(deezer-clib ${VALA_C})

