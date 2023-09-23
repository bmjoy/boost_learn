if(WIN32)
  set(BOOST_DEBUG ON)
  if(DEFINED ENV{BOOST_ROOT})
    set(BOOST_ROOT $ENV{BOOST_ROOT})
    list(APPEND BOOST_LIBRARYDIR
      ${BOOST_ROOT}/lib${PLATFORM}-msvc-14.2 )
  endif()

  set(Boost_USE_STATIC_LIBS        ON)
  set(Boost_USE_MULTITHREADED      ON)
  set(Boost_USE_STATIC_RUNTIME     OFF)
endif()

if (CMAKE_VERSION VERSION_LESS "3.16.0")
  # boost's own cmake file adds extra compile definitions that cotire fails to pick up
  set(Boost_NO_BOOST_CMAKE ON)
endif()

include (CheckCXXSourceCompiles)

if (WIN32)
  # On windows the requirements are higher according to the wiki.
  set(BOOST_REQUIRED_VERSION 1.73)
else()
  set(BOOST_REQUIRED_VERSION 1.71)
endif()

find_package(Boost ${BOOST_REQUIRED_VERSION} REQUIRED system filesystem program_options iostreams regex thread)

if(NOT Boost_FOUND)
  if(NOT DEFINED ENV{BOOST_ROOT} AND NOT DEFINED Boost_DIR AND NOT DEFINED BOOST_ROOT AND NOT DEFINED BOOSTROOT)
    message(FATAL_ERROR "No BOOST_ROOT environment variable could be found! Please make sure it is set and the points to your Boost installation.")
  endif()
endif()

add_executable (Daytime1 "Daytime1.cpp" "Daytime1.h")

target_link_libraries(Daytime1 PUBLIC ${Boost_LIBRARIES})

target_include_directories(Daytime1 PUBLIC ${Boost_INCLUDE_DIRS})
