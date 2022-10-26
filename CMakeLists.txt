cmake_minimum_required(VERSION 3.9)
project(MyProject)
set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_BINARY_DIR ${CMAKE_SOURCE_DIR}/bin)
# set(CMAKE_CXX_STANDARD 11)
target_compile_features(${PROJECT_NAME} PUBLIC cxx_std_11)


# Create library
include(cmake/SourcesAndHeaders.cmake)


#
# Prevent building in the source directory
#

if(PROJECT_SOURCE_DIR STREQUAL PROJECT_BINARY_DIR)
  message(FATAL_ERROR "In-source builds not allowed. Please make a new directory (called a build directory) and run CMake from there.\n")
endif()


if(${PROJECT_NAME}_ENABLE_UNIT_TESTING)
    add_library(${PROJECT_NAME}_lib ${headers} ${sources})
endif

set_target_properties(
  ${PROJECT_NAME}
  PROPERTIES
  ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib/${CMAKE_BUILD_TYPE}"
  LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib/${CMAKE_BUILD_TYPE}"
  RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}"
)



target_include_directories(
    ${PROJECT_NAME}
    PUBLIC
    $<INSTALL_INTERFACE:include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/src

# create executable
add_executable(${PROJECT_NAME} ${exe_sources})







# add_subdirectory(src)
# add_subdirectory(tests)

# target_link_libraries(${CMAKE_PROJECT_NAME}_run ${CMAKE_PROJECT_NAME}_lib)

# # Doxygen Build
# option(BUILD_DOC "Build Documentation" ON)

# find_package(Doxygen)
# if(DOXYGEN_FOUND)
#     set(BUILD_DOC_DIR ${CMAKE_SOURCE_DIR}/build/docs)
#     if(NOT EXISTS ${BUILD_DOC_DIR})
#         file(MAKE_DIRECTORY ${BUILD_DOC_DIR})
#     endif()

#     set(DOXYGEN_IN ${CMAKE_CURRENT_SOURCE_DIR}/docs/Doxyfile)
#     set(DOXYGEN_OUT ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile)
#     configure_file(${DOXYGEN_IN} ${DOXYGEN_OUT} @ONLY)

#     message("Doxygen buld started")
#     add_custom_target(Doxygen ALL
#             COMMAND ${DOXYGEN_EXECUTABLE} ${DOXYGEN_OUT}
#             WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
#             COMMENT "Generating API documentation with Doxygen"
#             VERBATIM)
# else(DOXYGEN_FOUND)
#     message("Doxygen needs to be installed to generate the documentation.")
# endif(DOXYGEN_FOUND)