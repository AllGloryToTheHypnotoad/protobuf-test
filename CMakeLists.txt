cmake_minimum_required(VERSION 3.4)

set(PROJECT_NAME test_proto)
project(${PROJECT_NAME})

find_package(Protobuf REQUIRED)

# protobuf headers
set(PROTO_FILES proto/msg.proto)

# c++ sources
set(SRC_FILES src/main.cpp)

# set(HDR_FILES)

PROTOBUF_GENERATE_CPP(PROTO_SRC PROTO_HDR ${PROTO_FILES})
protobuf_generate_python(PROTO_PY ${PROTO_FILES})

set(PY_SOURCES python/client.py python/server.py)
# add_custom_target(myTarget ALL DEPENDS ${PY_SOURCES})
add_custom_target(myTarget ALL DEPENDS ${PROTO_PY} ${PY_SOURCES})

add_executable(${PROJECT_NAME} ${SRC_FILES} ${HDR_FILES} ${PROTO_SRC} ${PROTO_HDR} ${PROTO_FILES})
target_include_directories(${PROJECT_NAME}
   PUBLIC ${CMAKE_CURRENT_BINARY_DIR}
   PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}
   PUBLIC ${PROTOBUF_INCLUDE_DIRS}
   )
target_link_libraries(${PROJECT_NAME} ${PROTOBUF_LIBRARIES})

# set(bob hi)
# file(WRITE __init__.py ${bob})
# file(MAKE_DIRECTORY test)

# install(myTarget ../python)
