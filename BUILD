cc_library(
        name = "libfieldconverter",
        srcs = glob(["src/*.cpp"]),
        hdrs = glob(["include/*.h"]),
        linkopts = ["-pthread"],
        visibility = ["//visibility:public"],
        )

cc_test(
        name = "fieldconverter-test",
        srcs = glob(["test/*.cpp", "test/*.h"]),
        copts = ["-Iexternal/gtest/include"],
        deps = [ 
        ":libfieldconverter",
        "@gtest//:main",
        ],
       )
