cmake_minimum_required(VERSION 2.6)
project(kaggle-digit-recognizer)

option(DEBUG "DEBUG" OFF)

set(MODEL_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)
include_directories("${CMAKE_CURRENT_SOURCE_DIR}/../../")

set(SOURCES ${MODEL_SOURCE_DIR}/DigitRecognizer.cpp)

if(DEBUG)
  message("Compilation with debug info (with ggdb3 flag)")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -ggdb3")
else()
  message("Compilation without debug info (without ggdb3 flag)")
endif()

add_executable(DigitRecognizer ${SOURCES})
target_link_libraries(DigitRecognizer
  ${MLPACK_LIBRARIES}
  ${ARMADILLO_LIBRARIES})

if (NOT EXISTS ${PROJECT_BINARY_DIR}/../data)
  add_custom_command(TARGET DigitRecognizer POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E tar xvzf ${CMAKE_CURRENT_SOURCE_DIR}/../kaggle_train_test_dataset.zip
    WORKING_DIRECTORY ${PROJECT_BINARY_DIR}/../)
endif()
