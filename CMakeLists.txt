cmake_minimum_required(VERSION 3.20.4)

project(tcpp VERSION 0.1.0)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

if(MSVC)
  add_compile_options("/wd4819")
  add_compile_options("/wd4127")
  add_compile_options("/MP")
  foreach (flag_var
      CMAKE_CXX_FLAGS
      CMAKE_CXX_FLAGS_DEBUG
      CMAKE_CXX_FLAGS_RELEASE
      CMAKE_CXX_FLAGS_MINSIZEREL
      CMAKE_CXX_FLAGS_RELWITHDEBINFO
      CMAKE_C_FLAGS
      CMAKE_C_FLAGS_DEBUG
      CMAKE_C_FLAGS_RELEASE)
    if (CMAKE_BUILD_TYPE STREQUAL "Debug")
      string(REPLACE "/MDd" "-MTd" ${flag_var} "${${flag_var}}")
    else()
      string(REPLACE "/MD" "-MT" ${flag_var} "${${flag_var}}")
    endif()
    string(REPLACE "/W3" "/W4" ${flag_var} "${${flag_var}}")
  endforeach()

  string(REGEX REPLACE "/O2" "Od"
    CMAKE_CXX_FLAGS_DEBUG
    "${CMAKE_CXX_FLAGS_DEBUG}")

  #add_compile_options( /std:c++latest )
  add_compile_options( /std:c++20 )
  #add_compile_options( -std=c++2a )
  #set(CMAKE_CXX_STANDARD 20)
elseif(APPLE)
  add_definitions( -Wall -o2 -g -ggdb -DSOCKLEN_T=socklen_t -Wc++17-extensions -stdlib=libc++ )
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++1z")
else()
  add_compile_options( 
    -g -ggdb 
    -Wall -W 
    -std=c++2a -fconcepts 
    )
endif()


add_subdirectory(tcpp)
add_subdirectory(test)