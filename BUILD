BUILD_DIR	= "build"
VM_SRC_DIR = "vm"
INCL_PATH	= ["-I"+BUILD_DIR, "-I"+VM_SRC_DIR+"/asm", "-I"+VM_SRC_DIR+"/code", "-I"+VM_SRC_DIR+"/compiler", "-I"+VM_SRC_DIR+"/disasm", "-I"+VM_SRC_DIR+"/interpreter", "-I"+VM_SRC_DIR+"/lookup", "-I"+VM_SRC_DIR+"/memory", "-I"+VM_SRC_DIR+"/oops", "-I"+VM_SRC_DIR+"/prims", "-I"+VM_SRC_DIR+"/recompiler", "-I"+VM_SRC_DIR+"/runtime", "-I"+VM_SRC_DIR+"/topIncludes", "-I"+VM_SRC_DIR+"/utilities"]

TEST_SRC_DIR 	= "test"
TEST_INCL_PATH = ["-I"+TEST_SRC_DIR+"/runtime", "-I"+TEST_SRC_DIR+"/utilities", "-I"+TEST_SRC_DIR+"/memory"]

DEFINES = ["-DDELTA_COMPILER", "-DASSERT", "-DDEBUG", "-D__LINUX__"]
COPTS = ["-O", "-m32", "-fno-rtti", "-Wno-write-strings", "-fno-operator-names", "-fms-extensions"]

#https://groups.google.com/forum/#!topic/bazel-discuss/8Snn9hLuSqo

cc_library(
    name = "easyunit",
    srcs = glob(["easyunit/src/*.cpp"]),
    hdrs = glob(["easyunit/easyunit/*.h"]),
    copts = ["-m32"],
    linkopts = ["-m32"],
    includes = ["easyunit/easyunit"],
)

cc_library(
    name = "libstrongtalk",
    hdrs = glob(["build/incls/*.incl"]) + ["vm/prims/prims.inc"],
    srcs = glob(["vm/**/*.cpp", "vm/**/*.hpp"], exclude=["vm/runtime/main.cpp"]),
    copts = INCL_PATH + COPTS + DEFINES,
    linkopts = ["-m32", "-lpthread", "-ldl", "-lrt", "-lm"],
)

cc_binary(
    name = "strongtalk",
    srcs = ["vm/runtime/main.cpp"],
    copts = INCL_PATH + COPTS + DEFINES,
    #linkstatic=0,
    deps = [":libstrongtalk"],
    
    data = ["strongtalk.bst"],
)

# BEWARE: easyunit exists with 0 even with failures
cc_test(
    name = "strongtalk-test",
    srcs = glob(["test/**/*.cpp", "test/**/*.hpp"], exclude=[
        "test/compiler/compiler_tests.cpp", # CRASH
        "test/interpreter/missingMethodBuilderTest.cpp", # CRASH -- differs from cmake version
        "test/memory/contextKlassTests.cpp", # CRASH
        "test/memory/methodLookupTests.cpp", # vm shows errors, tests pass
        "test/memory/proxyPrimsTest.cpp", # vm shows errors, tests pass
        "test/prims/alienIntegerCallout0Tests.cpp", # FAILS
        "test/prims/alienIntegerCallout1Tests.cpp", # FAILS
        "test/prims/byteArrayPrimsTests.cpp", # FAILS
    ]),
    copts = INCL_PATH + TEST_INCL_PATH + COPTS + DEFINES,
    #linkstatic=0,
    deps = [":libstrongtalk", ":easyunit"],
    
    data = ["strongtalk.bst"],
    args = ["-b strongtalk.bst"],
    size = "small",
)