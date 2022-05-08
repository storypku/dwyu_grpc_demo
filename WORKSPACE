workspace(name = "dwyu_grpc_demo")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

grpc_version = "1.46.0"

http_archive(
    name = "com_github_grpc_grpc",
    sha256 = "67423a4cd706ce16a88d1549297023f0f9f0d695a96dd684adc21e67b021f9bc",
    strip_prefix = "grpc-{}".format(grpc_version),
    urls = [
        "https://github.com/grpc/grpc/archive/v{}.tar.gz".format(grpc_version),
    ],
)

load("@com_github_grpc_grpc//bazel:grpc_deps.bzl", "grpc_deps")

grpc_deps()

load("@com_github_grpc_grpc//bazel:grpc_extra_deps.bzl", "grpc_extra_deps")

grpc_extra_deps()

dwyu_version = "0.0.1"

http_archive(
    name = "depend_on_what_you_use",
    sha256 = "05e8fdd0adbb481908db628a7e60120377b5bd3055e01cfad7ad4550c05114ac",
    strip_prefix = "depend_on_what_you_use-{}".format(dwyu_version),
    urls = [
        "https://github.com/martis42/depend_on_what_you_use/archive/{}.tar.gz".format(dwyu_version),
    ],
)

load("@depend_on_what_you_use//:dependencies.bzl", dwyu_dependencies = "public_dependencies")

dwyu_dependencies()
