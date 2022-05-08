load("@com_github_grpc_grpc//bazel:cc_grpc_library.bzl", "cc_grpc_library")
load("@rules_cc//cc:defs.bzl", "cc_proto_library")
load("@rules_proto//proto:defs.bzl", "proto_library")

package(default_visibility = ["//visibility:public"])

cc_grpc_library(
    name = "hello_world_cc_grpc",
    srcs = [":hello_world_proto"],
    grpc_only = True,
    # tags = ["no-dwyu"],
    deps = [
        ":hello_world_cc_proto",
        # "@com_github_grpc_grpc//:grpc++_codegen_base",
    ],
)

cc_proto_library(
    name = "hello_world_cc_proto",
    deps = [
        ":hello_world_proto",
    ],
)

proto_library(
    name = "hello_world_proto",
    srcs = ["hello_world.proto"],
)
