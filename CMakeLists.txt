cmake_minimum_required(VERSION 3.7.1)

project(opengloves_protocol)

find_package(protobuf CONFIG REQUIRED)
find_package(gRPC CONFIG REQUIRED)
find_package(Threads)

set(PROTO_FILES
    protos/server/server_output.proto
)

add_library(opengloves_protos ${PROTO_FILES})
target_link_libraries(opengloves_protos
    PUBLIC
        protobuf::libprotobuf
        gRPC::grpc
        gRPC::grpc++
)
target_include_directories(opengloves_protos PUBLIC ${CMAKE_CURRENT_BINARY_DIR})

get_target_property(grpc_cpp_plugin_location gRPC::grpc_cpp_plugin LOCATION)
protobuf_generate(TARGET opengloves_protos LANGUAGE cpp)
protobuf_generate(TARGET opengloves_protos LANGUAGE grpc GENERATE_EXTENSIONS .grpc.pb.h .grpc.pb.cc PLUGIN "protoc-gen-grpc=${grpc_cpp_plugin_location}")