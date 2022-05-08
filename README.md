# A Depend-on-What-You-Use and gRPC demo

## Problem Description

Currently, running `bazel build --config=dwyu //proto/...` fails with the following errmsgs:

```
Use --sandbox_debug to see verbose messages from the sandbox
================================================================================
DWYU analyzing: '//proto:hello_world_cc_grpc'

Result: FAILURE

Includes which are not available from the direct dependencies:
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/client_callback.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/message_allocator.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/server_context.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/method_handler.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/status.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/client_context.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/sync_stream.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/completion_queue.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/service_type.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/stub_options.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/async_stream.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/async_unary_call.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/async_generic_service.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/server_callback_handlers.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/server_callback.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/rpc_method.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/client_callback.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/message_allocator.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/server_context.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/method_handler.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/status.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/client_context.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/sync_stream.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/completion_queue.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/service_type.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/stub_options.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/async_stream.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/async_unary_call.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/async_generic_service.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/server_callback_handlers.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/server_callback.h'
  File='bazel-out/k8-fastbuild/bin/proto/hello_world.grpc.pb.h', include='grpcpp/impl/codegen/rpc_method.h'
================================================================================
INFO: Elapsed time: 0.315s, Critical Path: 0.10s
INFO: 3 processes: 3 internal.
FAILED: Build did NOT complete successfully
```

## Analysis

From DWYU's perspective, this issue was due to the missing direct dependency of `@com_github_grpc_grpc//:grpc++_codegen_base`.
In fact, if `@com_github_grpc_grpc//:grpc++_codegen_base` was added to `deps` of `//proto:hello_world_cc_proto`, running DWYU
will succeed.

However, since the `cc_grpc_library` rule was provided by gRPC, making changes to `cc_grpc_library` rule implementation seems a
better solution for issues like this.

