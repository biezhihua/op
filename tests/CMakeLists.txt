cmake_minimum_required(VERSION 3.0)

ADD_DEFINITIONS(-D _UNICODE)
ADD_DEFINITIONS(-D UNICODE)

ADD_DEFINITIONS(-D _CMAKE_BUILD)

SET(SRC_FILES "main.cpp"
"../libop/com/op_i.c")

IF(CMAKE_CL_64)
set(CMAKE_LIBRARY_PATH ${CMAKE_LIBRARY_PATH} "${PROJECT_SOURCE_DIR}/out/build/x64-Debug/libop/")
ELSE(CMAKE_CL_64)
set(CMAKE_LIBRARY_PATH ${CMAKE_LIBRARY_PATH} "${PROJECT_SOURCE_DIR}/out/build/x86-Debug/libop/")
ENDIF(CMAKE_CL_64)

include_directories( 
  ./
  ../libop/3rd_party/include/3rd_party
) 

add_executable(op_test ${SRC_FILES}  )

SET(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/../libop/)

target_link_libraries(op_test ${op_com})

IF(CMAKE_CL_64)
  install(TARGETS op_test  DESTINATION "${PROJECT_SOURCE_DIR}/bin/x64")
ELSE(CMAKE_CL_64)
  install(TARGETS op_test  DESTINATION "${PROJECT_SOURCE_DIR}/bin/x86")
ENDIF(CMAKE_CL_64)