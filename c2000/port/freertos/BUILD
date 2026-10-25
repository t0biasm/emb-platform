cc_library(
    name = "free_rtos_c28x_portable",
    srcs = [
        "portasm.asm",
        "port.c",
    ],
    hdrs = [
        "portmacro.h",
    ],
    deps = [
        "//c2000/mcal/f28p65/code:mcal_f28p65x_h",
        "//c2000/freertos:free_rtos_c28x_include",
        "@emb_toolchain//platforms/c2000/toolchain:c2000_builtin_directories",
    ],
    includes = ["."],
    visibility = ["//c2000/freertos:__subpackages__"],
)
