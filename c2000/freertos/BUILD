load("@bazel_skylib//rules:common_settings.bzl", "string_flag")

# FreeRTOS kernel for TI C28x.
# Kernel sources and headers come from the pinned upstream submodule
# (TexasInstruments/c2000ware-FreeRTOS, see .gitmodules); the C28x port itself
# lives in //c2000/port/freertos.

_SRC = "upstream/kernel/FreeRTOS/Source"

# FreeRTOSConfig.h is project specific. Projects provide it by pointing this
# flag at a cc_library that exposes FreeRTOSConfig.h, e.g. in their .bazelrc:
#   build --@emb_platform//c2000/freertos:config=//applications/foo:freertos_config
label_flag(
    name = "config",
    build_setting_default = ":no_config",
    visibility = ["//visibility:public"],
)

cc_library(
    name = "no_config",
    visibility = ["//visibility:private"],
)

cc_library(
    name = "free_rtos_c28x_include",
    hdrs = glob([_SRC + "/include/*.h"]),
    includes = [_SRC + "/include"],
    deps = [
        ":config",
        "@emb_toolchain//platforms/c2000/toolchain:c2000_builtin_directories",
    ],
    visibility = ["//visibility:public"],
)

cc_library(
    name = "free_rtos_c28x_app",
    srcs = glob([_SRC + "/*.c"]),
    includes = ["."],
    deps = [
        ":free_rtos_c28x_include",
        "//c2000/port/freertos:free_rtos_c28x_portable",
    ],
    visibility = ["//visibility:public"],
)

# Static allocation scheme; add as dep in projects that use heap_1.
cc_library(
    name = "heap_1",
    srcs = [_SRC + "/portable/MemMang/heap_1.c"],
    deps = [":free_rtos_c28x_include"],
    visibility = ["//visibility:public"],
)
