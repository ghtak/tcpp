project(tcpp.test)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../tcpp/include)

file(GLOB_RECURSE SRCS ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp )

add_executable(tcpp.test ${SRCS})


target_link_libraries(tcpp.test tcpp)
if(WIN32)
    target_link_libraries(tcpp.test ws2_32)
else()
    target_link_libraries(tcpp.test dl)
endif()

enable_testing()
add_test(tcpp.test tcpp.test)
add_custom_command(TARGET tcpp.test POST_BUILD COMMAND ctest -C $<CONFIGURATION> --verbose --output-on-failure )
