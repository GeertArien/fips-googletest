#
# project: fips-googletest
#
cmake_minimum_required(VERSION 2.8)

# include the fips main cmake file
get_filename_component(FIPS_ROOT_DIR "../fips" ABSOLUTE)
include("${FIPS_ROOT_DIR}/cmake/fips.cmake")


fips_setup(PROJECT fips-googletest)

fips_begin_lib(googletest)
fips_include_directories(googletest/googletest)
fips_dir(googletest/googletest/src)
fips_files(
        gtest.cc
        gtest-death-test.cc
        gtest-filepath.cc
        gtest-port.cc
        gtest-printers.cc
        gtest-test-part.cc
        gtest-typed-test.cc
        gtest_main.cc
)
fips_end_lib()

if (NOT FIPS_EXCEPTIONS)
    set(GOOGLETEST_NO_EXCEPTIONS "-fno-exceptions")
endif()

if (NOT FIPS_RTTI)
    add_definitions(-DGTEST_HAS_RTTI=0)
    set(GOOGLETEST_NO_RTTI "-fno-rtti")
else()
    add_definitions(-DGTEST_HAS_RTTI=1)
endif()

if (FIPS_CLANG)
    set_target_properties(googletest PROPERTIES COMPILE_FLAGS "${GOOGLETEST_NO_EXCEPTIONS} ${GOOGLETEST_NO_RTTI}")
elseif(FIPS_GCC)
    set_target_properties(googletest PROPERTIES COMPILE_FLAGS "${GOOGLETEST_NO_EXCEPTIONS} ${GOOGLETEST_NO_RTTI}")
elseif(FIPS_MSVC)
    if (NOT FIPS_EXCEPTIONS)
        string(REPLACE "/EHsc" "" CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS}) # -fno-exceptions
    endif()

    if (NOT FIPS_RTTI)
        string(REPLACE "/GR" "" CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS}) # -fno-rtti
    endif()
endif()

fips_finish()
