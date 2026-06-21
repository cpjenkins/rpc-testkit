
# gRPC :: https://github.com/grpc/grpc
option(RPC_TOOLKIT_USE_SYSTEM_GRPC
    "Use system installed gRPC" OFF
)


if(RPC_TOOLKIT_USE_SYSTEM_GRPC)
    find_package(gRPC CONFIG REQUIRED)
else()

endif()