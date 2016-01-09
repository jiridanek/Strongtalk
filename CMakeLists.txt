cmake_minimum_required(VERSION 2.8.7)
project(Strongtalk)

add_definitions(-DDELTA_COMPILER -DASSERT -DDEBUG)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -m32 -fno-rtti -Wno-write-strings -fno-operator-names -fms-extensions")

# http://www.artificialworlds.net/blog/2014/07/18/best-gcc-warning-flags-for-compiling-c/
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wredundant-decls -Wcast-align -Wmissing-declarations -Wmissing-include-dirs -Wswitch-enum -Wswitch-default -Wextra -Wall -Werror -Winvalid-pch -Wredundant-decls -Wformat=2 -Wmissing-format-attribute -Wformat-nonliteral")
# TODO: gragually enable
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-error -Wno-unused-parameter -Wno-reorder -Wno-redundant-decls -Wno-missing-declarations -Wno-unused-variable -Wno-type-limits -Wno-unused-function -Wno-switch-default -Wno-switch -Wno-format-security -Wno-format-nonliteral -Wno-switch-enum -Wno-conversion-null -Wno-format -Wno-sign-compare -Wno-empty-body -Wno-char-subscripts")
# https://stackoverflow.com/questions/10046114/in-cmake-how-can-i-test-if-the-compiler-is-clang
if (CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  # using Clang
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-overloaded-virtual -Wno-cast-align -Wno-undefined-bool-conversion -Wno-sometimes-uninitialized -Wno-shift-negative-value -Wno-constant-conversion -Wno-unused-private-field -Wno-extern-initializer -Wno-new-returns-null -Wno-tautological-constant-out-of-range-compare -Wno-tautological-undefined-compare")
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
  # using GCC
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-unused-but-set-variable -Wno-suggest-attribute=format")
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Intel")
  # using Intel C++
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")
  # using Visual Studio C++
endif()

# https://gcc.gnu.org/onlinedocs/gcc/Gcov-and-Optimization.html
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fprofile-arcs -ftest-coverage")

# https://developerblog.redhat.com/2014/10/16/gcc-undefined-behavior-sanitizer-ubsan/
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=undefined")

#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=address")
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=thread")

include_directories(
    build
    vm/asm
    vm/code
    vm/compiler
    vm/disasm
    vm/interpreter
    vm/lookup
    vm/memory
    vm/oops
    vm/prims
    vm/recompiler
    vm/runtime
    vm/topIncludes
    vm/utilities
    easyunit
    easyunit/easyunit
    test/runtime
    test/utilities
    test/memory
)

set(LIBSTRONGTALK_SOURCE_FILES
    vm/asm/assembler.cpp
    vm/asm/assembler.hpp
    vm/asm/codeBuffer.cpp
    vm/asm/codeBuffer.hpp
    vm/asm/location.cpp
    vm/asm/location.hpp
    vm/asm/x86_mapping.cpp
    vm/asm/x86_mapping.hpp
    vm/code/codeTable.cpp
    vm/code/codeTable.hpp
    vm/code/compiledIC.cpp
    vm/code/compiledIC.hpp
    vm/code/compiledPIC.cpp
    vm/code/compiledPIC.hpp
    vm/code/iCache.cpp
    vm/code/iCache.hpp
    vm/code/inliningdb.cpp
    vm/code/inliningdb.hpp
    vm/code/jumpTable.cpp
    vm/code/jumpTable.hpp
    vm/code/locations.cpp
    vm/code/locations.hpp
    vm/code/mapConformance.cpp
    vm/code/mapConformance.hpp
    vm/code/nameDesc.cpp
    vm/code/nameDesc.hpp
    vm/code/nativeInstruction.cpp
    vm/code/nativeInstruction.hpp
    vm/code/ncode.cpp
    vm/code/ncode.hpp
    vm/code/nmethod.cpp
    vm/code/nmethod.hpp
    vm/code/nmethodScopes.cpp
    vm/code/nmethodScopes.hpp
    vm/code/pcDesc.cpp
    vm/code/pcDesc.hpp
    vm/code/pregMapping.cpp
    vm/code/pregMapping.hpp
    vm/code/registerMask.cpp
    vm/code/registerMask.hpp
    vm/code/relocInfo.cpp
    vm/code/relocInfo.hpp
    vm/code/scopeDesc.cpp
    vm/code/scopeDesc.hpp
    vm/code/scopeDescRecorder.cpp
    vm/code/scopeDescRecorder.hpp
    vm/code/stubRoutines.cpp
    vm/code/stubRoutines.hpp
    vm/code/zone.cpp
    vm/code/zone.hpp
    vm/code/zoneHeap.cpp
    vm/code/zoneHeap.hpp
    vm/compiler/basicBlock.cpp
    vm/compiler/basicBlock.hpp
    vm/compiler/bitVector.cpp
    vm/compiler/bitVector.hpp
    vm/compiler/codeGenerator.cpp
    vm/compiler/codeGenerator.hpp
    vm/compiler/compiler.cpp
    vm/compiler/compiler.hpp
    vm/compiler/compileTimeClosure.cpp
    vm/compiler/compileTimeClosure.hpp
    vm/compiler/compUtils.cpp
    vm/compiler/compUtils.hpp
    vm/compiler/costModel.cpp
    vm/compiler/costModel.hpp
    vm/compiler/defUse.cpp
    vm/compiler/defUse.hpp
    vm/compiler/expr.cpp
    vm/compiler/expr.hpp
    vm/compiler/inline.cpp
    vm/compiler/inliner.cpp
    vm/compiler/inliner.hpp
    vm/compiler/intervalInfo.cpp
    vm/compiler/intervalInfo.hpp
    vm/compiler/loopOpt.cpp
    vm/compiler/loopOpt.hpp
    vm/compiler/node.cpp
    vm/compiler/node.hpp
    vm/compiler/node.inline.hpp
    vm/compiler/nodeBuilder.cpp
    vm/compiler/nodeBuilder.hpp
    vm/compiler/oldCodeGenerator.cpp
    vm/compiler/oldCodeGenerator.hpp
    vm/compiler/opcode.cpp
    vm/compiler/opcode.hpp
    vm/compiler/preg.cpp
    vm/compiler/preg.hpp
    vm/compiler/primInliner.cpp
    vm/compiler/primInliner.hpp
    vm/compiler/regAlloc.cpp
    vm/compiler/regAlloc.hpp
    vm/compiler/rscope.cpp
    vm/compiler/rscope.hpp
    vm/compiler/scope.cpp
    vm/compiler/scope.hpp
    vm/compiler/slist.cpp
    vm/compiler/slist.hpp
    vm/compiler/split.cpp
    vm/compiler/split.hpp
    vm/disasm/disassembler.cpp
    vm/disasm/disassembler.hpp
    vm/interpreter/bytecodes.cpp
    vm/interpreter/bytecodes.hpp
    vm/interpreter/codeIterator.cpp
    vm/interpreter/codeIterator.hpp
    vm/interpreter/dispatchTable.cpp
    vm/interpreter/dispatchTable.hpp
    vm/interpreter/floats.cpp
    vm/interpreter/floats.hpp
    vm/interpreter/hCodeBuffer.cpp
    vm/interpreter/hCodeBuffer.hpp
    vm/interpreter/ic_iterator.cpp
    vm/interpreter/ic_iterator.hpp
    vm/interpreter/interpretedIC.cpp
    vm/interpreter/interpretedIC.hpp
    vm/interpreter/interpreter.cpp
    vm/interpreter/interpreter.hpp
    vm/interpreter/interpreterStatistics.cpp
    vm/interpreter/interpreterStatistics.hpp
    vm/interpreter/methodIterator.cpp
    vm/interpreter/methodIterator.hpp
    vm/interpreter/methodPrinter.cpp
    vm/interpreter/methodPrinter.hpp
    vm/interpreter/missingMethodBuilder.cpp
    vm/interpreter/missingMethodBuilder.hpp
    vm/interpreter/prettyPrinter.cpp
    vm/interpreter/prettyPrinter.hpp
    vm/lookup/key.cpp
    vm/lookup/key.hpp
    vm/lookup/lookupCache.cpp
    vm/lookup/lookupCache.hpp
    vm/lookup/lookupTypes.cpp
    vm/lookup/lookupTypes.hpp
    vm/memory/ageTable.cpp
    vm/memory/ageTable.hpp
    vm/memory/allocation.cpp
    vm/memory/allocation.hpp
    vm/memory/debugPrintable.cpp
    vm/memory/debugPrintable.hpp
    vm/memory/error.cpp
    vm/memory/error.hpp
    vm/memory/generation.cpp
    vm/memory/generation.hpp
    vm/memory/generation.inline.hpp
    vm/memory/handle.cpp
    vm/memory/handle.hpp
    vm/memory/iterator.cpp
    vm/memory/iterator.hpp
    vm/memory/markSweep.cpp
    vm/memory/markSweep.hpp
    vm/memory/oopFactory.cpp
    vm/memory/oopFactory.hpp
    vm/memory/printlayout.cpp
    vm/memory/printlayout.hpp
    vm/memory/reflection.cpp
    vm/memory/reflection.hpp
    vm/memory/rSet.cpp
    vm/memory/rSet.hpp
    vm/memory/rSet.inline.hpp
    vm/memory/snapshot.cpp
    vm/memory/snapshot.hpp
    vm/memory/space.cpp
    vm/memory/space.hpp
    vm/memory/spaceSize.cpp
    vm/memory/spaceSize.hpp
    vm/memory/symbolTable.cpp
    vm/memory/symbolTable.hpp
    vm/memory/universe.cpp
    vm/memory/universe.hpp
    vm/memory/universe.inline.hpp
    vm/memory/universe.store.hpp
    vm/memory/universe_more.cpp
    vm/memory/util.cpp
    vm/memory/util.hpp
    vm/memory/vmSymbols.cpp
    vm/memory/vmSymbols.hpp
    vm/oops/associationKlass.cpp
    vm/oops/associationKlass.hpp
    vm/oops/associationOop.cpp
    vm/oops/associationOop.hpp
    vm/oops/blockKlass.cpp
    vm/oops/blockKlass.hpp
    vm/oops/blockOop.cpp
    vm/oops/blockOop.hpp
    vm/oops/blockOop.inline.hpp
    vm/oops/byteArrayKlass.cpp
    vm/oops/byteArrayKlass.hpp
    vm/oops/byteArrayOop.cpp
    vm/oops/byteArrayOop.hpp
    vm/oops/dByteArrayKlass.cpp
    vm/oops/dByteArrayKlass.hpp
    vm/oops/dByteArrayOop.cpp
    vm/oops/dByteArrayOop.hpp
    vm/oops/doubleKlass.cpp
    vm/oops/doubleKlass.hpp
    vm/oops/doubleOop.cpp
    vm/oops/doubleOop.hpp
    vm/oops/dValueArrayKlass.cpp
    vm/oops/dValueArrayKlass.hpp
    vm/oops/dValueArrayOop.cpp
    vm/oops/dValueArrayOop.hpp
    vm/oops/klass.cpp
    vm/oops/klass.hpp
    vm/oops/klassKlass.cpp
    vm/oops/klassKlass.hpp
    vm/oops/klassOop.cpp
    vm/oops/klassOop.hpp
    vm/oops/markOop.cpp
    vm/oops/markOop.hpp
    vm/oops/memOop.cpp
    vm/oops/memOop.hpp
    vm/oops/memOop.inline.hpp
    vm/oops/memOopKlass.cpp
    vm/oops/memOopKlass.hpp
    vm/oops/methodKlass.cpp
    vm/oops/methodKlass.hpp
    vm/oops/methodOop.cpp
    vm/oops/methodOop.hpp
    vm/oops/mixinKlass.cpp
    vm/oops/mixinKlass.hpp
    vm/oops/mixinOop.cpp
    vm/oops/mixinOop.hpp
    vm/oops/objArrayKlass.cpp
    vm/oops/objArrayKlass.hpp
    vm/oops/objArrayOop.cpp
    vm/oops/objArrayOop.hpp
    vm/oops/oop.cpp
    vm/oops/oop.hpp
    vm/oops/oop.inline.hpp
    vm/oops/oopsHierarchy.hpp
    vm/oops/processKlass.cpp
    vm/oops/processKlass.hpp
    vm/oops/processOop.cpp
    vm/oops/processOop.hpp
    vm/oops/proxyKlass.cpp
    vm/oops/proxyKlass.hpp
    vm/oops/proxyOop.cpp
    vm/oops/proxyOop.hpp
    vm/oops/smiKlass.cpp
    vm/oops/smiKlass.hpp
    vm/oops/smiOop.cpp
    vm/oops/smiOop.hpp
    vm/oops/symbolKlass.cpp
    vm/oops/symbolKlass.hpp
    vm/oops/symbolOop.cpp
    vm/oops/symbolOop.hpp
    vm/oops/vframeKlass.cpp
    vm/oops/vframeKlass.hpp
    vm/oops/vframeOop.cpp
    vm/oops/vframeOop.hpp
    vm/oops/weakArrayKlass.cpp
    vm/oops/weakArrayKlass.hpp
    vm/prims/behavior_prims.cpp
    vm/prims/behavior_prims.hpp
    vm/prims/behavior_prims_gen.cpp
    vm/prims/block_prims.cpp
    vm/prims/block_prims.hpp
    vm/prims/block_prims_gen.cpp
    vm/prims/byteArray_prims.cpp
    vm/prims/byteArray_prims.hpp
    vm/prims/callBack_prims.cpp
    vm/prims/callBack_prims.hpp
    vm/prims/dByteArray_prims.cpp
    vm/prims/dByteArray_prims.hpp
    vm/prims/debug_prims.cpp
    vm/prims/debug_prims.hpp
    vm/prims/dll.cpp
    vm/prims/dll.hpp
    vm/prims/double_prims.cpp
    vm/prims/double_prims.hpp
    vm/prims/double_prims_gen.cpp
    vm/prims/dValueArray_prims.cpp
    vm/prims/dValueArray_prims.hpp
    vm/prims/generatedPrimitives.cpp
    vm/prims/generatedPrimitives.hpp
    vm/prims/integerOps.cpp
    vm/prims/integerOps.hpp
    vm/prims/method_prims.cpp
    vm/prims/method_prims.hpp
    vm/prims/mixin_prims.cpp
    vm/prims/mixin_prims.hpp
    vm/prims/objArray_prims.cpp
    vm/prims/objArray_prims.hpp
    vm/prims/oop_prims.cpp
    vm/prims/oop_prims.hpp
    vm/prims/prim.cpp
    vm/prims/prim.hpp
    vm/prims/prim_def.hpp
    vm/prims/prim_impl.hpp
    vm/prims/process_prims.cpp
    vm/prims/process_prims.hpp
    vm/prims/proxy_prims.cpp
    vm/prims/proxy_prims.hpp
    vm/prims/smi_prims.cpp
    vm/prims/smi_prims.hpp
    vm/prims/smi_prims_gen.cpp
    vm/prims/system_prims.cpp
    vm/prims/system_prims.hpp
    vm/prims/vframe_prims.cpp
    vm/prims/vframe_prims.hpp
    vm/recompiler/recompile.cpp
    vm/recompiler/recompile.hpp
    vm/recompiler/recompiler.cpp
    vm/recompiler/recompiler.hpp
    vm/recompiler/rframe.cpp
    vm/recompiler/rframe.hpp
    vm/runtime/abort.cpp
    vm/runtime/abort.hpp
    vm/runtime/arguments.cpp
    vm/runtime/arguments.hpp
    vm/runtime/bootstrap.cpp
    vm/runtime/bootstrap.hpp
    vm/runtime/callBack.cpp
    vm/runtime/callBack.hpp
    vm/runtime/debug.cpp
    vm/runtime/debug.hpp
    vm/runtime/delta.cpp
    vm/runtime/delta.hpp
    vm/runtime/disclaimer.cpp
    vm/runtime/disclaimer.hpp
    vm/runtime/evaluator.cpp
    vm/runtime/evaluator.hpp
    vm/runtime/except.cpp
    vm/runtime/fprofiler.cpp
    vm/runtime/fprofiler.hpp
    vm/runtime/frame.cpp
    vm/runtime/frame.hpp
    vm/runtime/init.cpp
    vm/runtime/init.hpp
    vm/runtime/os.hpp
    vm/runtime/os_darwin.cpp
    vm/runtime/os_linux.cpp
    vm/runtime/os_nt.cpp
    vm/runtime/preemption.cpp
    vm/runtime/preemption.hpp
    vm/runtime/process.cpp
    vm/runtime/process.hpp
    vm/runtime/runtime.cpp
    vm/runtime/runtime.hpp
    vm/runtime/savedRegisters.cpp
    vm/runtime/savedRegisters.hpp
    vm/runtime/shell.cpp
    vm/runtime/shell.hpp
    vm/runtime/stackChunk.cpp
    vm/runtime/stackChunk.hpp
    vm/runtime/sweeper.cpp
    vm/runtime/sweeper.hpp
    vm/runtime/systemAverage.cpp
    vm/runtime/systemAverage.hpp
    vm/runtime/task.cpp
    vm/runtime/task.hpp
    vm/runtime/tempDecoder.cpp
    vm/runtime/tempDecoder.hpp
    vm/runtime/timer.cpp
    vm/runtime/timer.hpp
    vm/runtime/uncommonBranch.cpp
    vm/runtime/uncommonBranch.hpp
    vm/runtime/vframe.cpp
    vm/runtime/vframe.hpp
    vm/runtime/virtualspace.cpp
    vm/runtime/virtualspace.hpp
    vm/runtime/vmOperations.cpp
    vm/runtime/vmOperations.hpp
    vm/topIncludes/asserts.hpp
    vm/topIncludes/bits.cpp
    vm/topIncludes/bits.hpp
    vm/topIncludes/config.hpp
    vm/topIncludes/macros.hpp
    vm/topIncludes/std_includes.hpp
    vm/topIncludes/tag.hpp
    vm/topIncludes/top.hpp
    vm/topIncludes/types.hpp
    vm/utilities/eventLog.cpp
    vm/utilities/eventLog.hpp
    vm/utilities/growableArray.cpp
    vm/utilities/growableArray.hpp
    vm/utilities/longInt.cpp
    vm/utilities/longInt.hpp
    vm/utilities/lprintf.cpp
    vm/utilities/lprintf.hpp
    vm/utilities/objectIDTable.cpp
    vm/utilities/objectIDTable.hpp
    vm/utilities/ostream.cpp
    vm/utilities/ostream.hpp
)

set(STRONGTALKTEST_SOURCE_FILES
# compiler
#     test/compiler/compiler_tests.cpp # CRASH: Linux/GCC & Linux/clang & Mingw/wine
    test/compiler/node_tests.cpp

# interpreter OK:  Linux/GCC & Mingw/wine
    test/interpreter/hCodeBufferTests.cpp
    test/interpreter/interpretedIC_tests.cpp
    test/interpreter/missingMethodBuilderTest.cpp

# memory
    # klass
        test/memory/association_klass_tests.cpp
        test/memory/blockClosureKlassTests.cpp
        test/memory/byteArrayKlassTests.cpp
#         test/memory/contextKlassTests.cpp  # CRASH: Linux/GCC & Linux/clang & wine/Mingw
    #double
        test/memory/doubleByteArrayTests.cpp
        test/memory/doubleValueArrayTests.cpp
        test/memory/doublOopKlassTests.cpp
        test/memory/edenMark.hpp
        test/memory/memOopAllocationTests.cpp
        test/memory/memOopKlassTests.cpp
#         test/memory/methodLookupTests.cpp # Strongtalk throws doesNotUnderstand but all tests succeed
        test/memory/mixinKlassTests.cpp
        test/memory/objArrayKlassTests.cpp
        test/memory/old_space_tests.cpp
        test/memory/oldGeneration.cpp
        test/memory/processKlassTests.cpp
        test/memory/proxyKlassTests.cpp
#         test/memory/proxyPrimsTest.cpp # vm asserts failures but all tests (assers in tests) succeed
        test/memory/universe_tests.cpp
        test/memory/vframeKlassTests.cpp
# prims/alienInteger
#     test/prims/alienIntegerCallout0Tests.cpp  # CRASH: Linux/GCC & Linux/clang & wine/Mingw
#     test/prims/alienIntegerCallout1Tests.cpp  # OK: wine/Mingw FAIL: Linux/GCC & Linux/clang
    test/prims/alienIntegerCallout2Tests.cpp
    test/prims/alienIntegerCallout3Tests.cpp
    test/prims/alienIntegerCallout4Tests.cpp
    test/prims/alienIntegerCallout5Tests.cpp
    test/prims/alienIntegerCallout6Tests.cpp
    test/prims/alienIntegerCallout7Tests.cpp
    #test/prims/alienIntegerCalloutWithArgumentsTests.cpp # FAIL: Linux/clang
# prims/behavior OK: Linux/GCC & wine/Mingw
    test/prims/behaviorPrimsSuperclassTests.cpp
    test/prims/behaviorPrimsTests.cpp
# prims/byteArray
    test/prims/byteArrayPrimsTests.cpp     # FAIL: Linux/GCC & Linux/clang & wine/Mingw
    test/prims/dbyteArrayPrimsTests.cpp
# prims/alien OK: Linux/GCC & wine/Mingw
    test/prims/directAlienPrimsTests.cpp
    test/prims/indirectAlienPrimsTests.cpp
# prims/integer OK: Linux/GCC & wine/Mingw
    test/prims/integerOpsTests.cpp
    test/prims/largeIntegerByteArrayPrimTests.cpp
    test/prims/objArrayPrimsTests.cpp
# prims/oop OK: Linux/GCC & wine/Mingw
    test/prims/oop_prims.cpp
    test/prims/oop_prims_perform.cpp # FAIL: Linux/clang
# prims OK: Linux/GCC & wine/Mingw
    test/prims/pointerAlienPrimsTest.cpp
    test/prims/smi_prims_tests.cpp
    test/prims/systemPrims.cpp
# runtime OK: Linux/GCC & Mingw/wine
    test/runtime/testProcess.hpp
    test/runtime/virtualspace.cpp
# utilities OK: Linux/GCC & Mingw/wine
    test/utilities/longInt.cpp
    test/utilities/lprintf_test.cpp
    test/utilities/testNotifier.hpp
    test/utilities/testUtils.hpp
# ??? what is it for ???
#     test/callbacks.cpp
)

set(LIBEASYUNIT_SOURCE_FILES
    easyunit/src/alloc.cpp
    easyunit/src/defaulttestprinter.cpp
    easyunit/src/simplestring.cpp
    easyunit/src/test.cpp
    easyunit/src/testcase.cpp
    easyunit/src/testpartresult.cpp
    easyunit/src/testregistry.cpp
    easyunit/src/testresult.cpp
    easyunit/src/testrunner.cpp
)
    
add_library(libstrongtalk ${LIBSTRONGTALK_SOURCE_FILES})

add_library(libeasyunit ${LIBEASYUNIT_SOURCE_FILES})

add_executable(strongtalk vm/runtime/main.cpp)
target_link_libraries(strongtalk libstrongtalk)

add_executable(strongtalk-test ${STRONGTALKTEST_SOURCE_FILES} test/main/main.cpp)
target_link_libraries(strongtalk-test libstrongtalk libeasyunit)

# OS dependent defines.
# should work for Linux/GCC, Linux/clang and Linux/MinGw&wine

if(UNIX)
    # TODO: following works for Linux. Should be tried on OSX.
    target_link_libraries(libstrongtalk -lpthread -ldl -lrt)
elseif(WIN32)   # really WIN32 and not MINGW, this is crosscompilation
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -static-libgcc -static-libstdc++")
endif()