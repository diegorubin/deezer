find_package(PkgConfig)
pkg_check_modules(GTK REQUIRED gtk+-3.0)
pkg_check_modules(GTHREAD REQUIRED gthread-3.0)

set(CFLAGS
  ${GTK_CFLAGS} ${GTK_CFLAGS_OTHER}
  ${GTHREAD_CFLAGS} ${GTHREAD_CFLAGS_OTHER}
)
add_definitions(${CFLAGS})

set(LIBS
  ${GTK_LIBRARIES}
  ${GTHREAD_LIBRARIES}
)
link_libraries(${LIBS})

set(LIB_PATHS
  ${GTK_LIBRARY_DIRS}
  ${GTHREAD_LIBRARY_DIRS}
)
link_directories(${LIB_PATHS})

set(VALA_SRC
  deezer.vala
)

# Vala segfaults if its output dirs are not existant. Therefore make sure they
# are there.
macro(make_compile_directory directory)
  if (NOT IS_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/${directory}")
    make_directory("${CMAKE_CURRENT_BINARY_DIR}/${directory}")
  endif (NOT IS_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/${directory}")
endmacro(make_compile_directory)

vala_precompile(VALA_C
  ${VALA_SRC}
PACKAGES 
  gtk+-2.0
  posix
OPTIONS 
  --thread
  --debug
GENERATE_HEADER
  deezer
GENERATE_VAPI
  deezer
)

add_executable(deezer
  ${VALA_C}
)

target_link_libraries(-lm)

install(TARGETS 
  deezer
RUNTIME
DESTINATION
  bin
)

add_library(
  SHARED
  EXCLUDE_FROM_ALL
  ${VALA_C} deezer.h deezer.vapi
)

