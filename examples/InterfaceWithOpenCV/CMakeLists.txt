cmake_minimum_required(VERSION 3.17)

project(InterfaceWithOpenCV)

set(CMAKE_CXX_STANDARD 17)

set(NUMCPP_INCLUDES ../../include)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

add_executable(${PROJECT_NAME} InterfaceWithOpenCV.cpp)

set_target_properties(${PROJECT_NAME}
  PROPERTIES
  RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin/$<0:>
)

set(Boost_USE_STATIC_LIBS TRUE) 
find_package(Boost 1.68.0)

if (NOT (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC"))
find_package (OpenCV REQUIRED)
endif ()

target_include_directories(${PROJECT_NAME} PRIVATE 
  ${NUMCPP_INCLUDES}
  ${Boost_INCLUDE_DIRS} 
  ${OpenCV_INCLUDE_DIRS}
)

if (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  target_link_directories(${PROJECT_NAME} PRIVATE ${OpenCV_LIBRARY_DIR})
  target_link_libraries(${PROJECT_NAME} PRIVATE opencv_world343.lib)
else()
  target_link_libraries(${PROJECT_NAME} PRIVATE ${OpenCV_LIBS})
endif()

target_compile_options(${PROJECT_NAME} PRIVATE
  $<$<OR:$<CXX_COMPILER_ID:Clang>,$<CXX_COMPILER_ID:GNU>>:-Wall>
  $<$<OR:$<CXX_COMPILER_ID:Clang>,$<CXX_COMPILER_ID:GNU>>:-Wextra>
  $<$<OR:$<CXX_COMPILER_ID:Clang>,$<CXX_COMPILER_ID:GNU>>:-pedantic>
  $<$<OR:$<CXX_COMPILER_ID:Clang>,$<CXX_COMPILER_ID:GNU>>:-Wdouble-promotion>
  $<$<OR:$<CXX_COMPILER_ID:Clang>,$<CXX_COMPILER_ID:GNU>>:-Wunused>
  $<$<CXX_COMPILER_ID:MSVC>:/W4>
  $<$<CXX_COMPILER_ID:MSVC>:/Zi>
  $<$<CXX_COMPILER_ID:MSVC>:/sdl>
  $<$<CXX_COMPILER_ID:MSVC>:/MP>
  $<$<CXX_COMPILER_ID:MSVC>:/Gy>
  $<$<AND:$<CXX_COMPILER_ID:MSVC>,$<CONFIG:Release>>:/Oi>
  $<$<AND:$<CXX_COMPILER_ID:MSVC>,$<CONFIG:Release>>:/Ot>
  $<$<AND:$<CXX_COMPILER_ID:MSVC>,$<CONFIG:Release>>:/GL>
)

target_link_options(${PROJECT_NAME} PRIVATE
  $<$<AND:$<CXX_COMPILER_ID:MSVC>,$<CONFIG:Release>>:/LTCG>
)

target_compile_definitions(${PROJECT_NAME} PRIVATE 
  $<$<CXX_COMPILER_ID:MSVC>:_SILENCE_CXX17_RESULT_OF_DEPRECATION_WARNING>
)
