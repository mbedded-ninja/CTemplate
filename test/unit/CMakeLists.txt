#
# \file 	        CMakeLists.txt
# \author 		Geoffrey Hunter <gbmhunter@gmail.com> (www.mbedded.ninja)
# \edited               n/a
# \created		2017-11-24
# \last-modified	2018-04-08
# \brief 		Contains instructions for building the unit tests.
# \details
#               See README.md in root dir for more info.

enable_testing()
find_package (Threads)
#find_package(GTest REQUIRED)
#message("gtest libraries found at ${GTEST_BOTH_LIBRARIES}")

file(GLOB_RECURSE CTemplateTests_SRC
        "*.cpp"
        "*.hpp")

add_executable(CTemplateTests ${CTemplateTests_SRC})

if(COVERAGE)
#    set_target_properties(CppTemplateTests PROPERTIES COMPILE_FLAGS "-g -O0 -fprofile-arcs -ftest-coverage")
    set_target_properties(CTemplateTests PROPERTIES LINK_FLAGS "-coverage -lgcov")
endif ()

target_link_libraries(CTemplateTests LINK_PUBLIC CTemplate gtest_main ${CMAKE_THREAD_LIBS_INIT})

# The custom target and custom command below allow the unit tests
# to be run.
# If you want them to run automatically by CMake, uncomment #ALL
add_custom_target(
        run_unit_tests #ALL
        DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/CTemplateTests.touch CTemplateTests)

add_custom_command(
        OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/CTemplateTests.touch
        COMMAND ${CMAKE_CURRENT_BINARY_DIR}/CTemplateTests)

# Target name, executable that runs unit tests, output directory
if(COVERAGE)
        setup_target_for_coverage(${PROJECT_NAME}_coverage CTemplateTests coverage)
endif()