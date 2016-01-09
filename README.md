# Strongtalk

Smalltalk… with a need for speed

## Badges

drone.io (makefile): [![Build Status](https://drone.io/github.com/jirkadanek/Strongtalk/status.png)](https://drone.io/github.com/jirkadanek/Strongtalk/latest)      travis-cl (cmake): [![Build Status](https://travis-ci.org/jirkadanek/Strongtalk.svg?branch=master)](https://travis-ci.org/jirkadanek/Strongtalk)      Coverity (cmake): [![Coverity Scan Build Status](https://scan.coverity.com/projects/6340/badge.svg)](https://scan.coverity.com/projects/jirkadanek-strongtalk)

## About this fork

*Mostly small fixes all around, nothing ground breaking or particularly useful to anybody. It compiles, some tests run, the GUI runs, and that's pretty much it.*

This fork fixes the Linux makefile (a70f97767f4caa60a5f18e169a01b4d0b51cf3fb) and some coding errors (c773198356a234075828bbd167bf9239ad48b9e0) so that the project now builds on Linux. In addition, it updates the code so that legacy compiler flags (like `-fpermissive` (eea6849e3a6168ff72770975418951a858f30d5b) and `-fno-for-scope` (1498b7a341a75243bfca4dc3e644e57a472eecf2)) can be dropped. As a side effect, this makes the project compile with clang, as well as GCC. Next, build configuration files for cmake (45f5a9635abd5506c2f702b8151786e2db07b967) and Bazel (96eef2e8ed8a1a231c699ce70591698581b441ab) were added. Some errors found with the `scan.coverity.com` tool were fixed (, , , ) in hopes it would help debugging mysterious crashes in the `strongtalk-test` binary (50730fd8992dbc56b5a39161c6c45dd721eef018) and in the vm itself, if one runs it with valgrind or -fsanitize=address or --covarage. These efforts were partially succesfull. Strongtalk can be now compiled with MinGW (bdd4827881c623cabe7971716006c65a5954e025) and run under wine on Linux. Furthermore, the code was refactored a bit where it made sense to me (61956120c8c81726360899ac9b63f0c768bbb70f, ).

Microsoft Visual Studio 2010 or later would be required to build the project due to including `<stdint.h>`. In addition, `nullptr` from C++11 is now used in one place. That means GCC 4.8+ or so is required.

Long time ago, bf88fb27d2cd1703a39d8234cb6610e78c6ee842 split the VM into a shared library and a related binary. I do not have a preference in this and it is easy to change either way both in cmake and bazel.

## Building on Linux

*This fork was tested only on x86_64 Arch Linux. It will likely work on other distributions. Strongtalk's JIT generates machine code, so IA-32 (or the backwards-compatible x86_64) CPU is required. See README.txt or wiki pages on Google Code for build instructions on WIndows and OSX. They probably won't work, though.*

Be sure to have IA-32 dev environment installed. The compiler is being executed with `-m32` flag.

### make

In the `build` directory, run

    make -f ../build.linux/makefile ROOT_DIR=$(cd ..; pwd) default
    
This performs in-source build. Binaries are placed in the `build` directory.
    
### cmake (primary)

In the `build` directory, run

    make -f ../build.linux/makefile ROOT_DIR=$(cd ..; pwd) dependencies

Create a build directory, say `cmakebuild` and switch to it. Then run

    cmake -DCMAKE_BUILD_TYPE=Debug ..
    make VERBOSE=1
    
This is an out-of-source build. Binaries are placed in the `cmakebuild` directory. Run tests with `cmakebuild/strongtalk-test`.

#### Cross compilation for Windows

On ArchLinux, install `mingw-w64-toolchain` from the repository and `mingw-w64-cmake` from the AUR. Create a build directory, say `windowsbuild` and switch to it. Then run

    cmake -DCMAKE_TOOLCHAIN_FILE=/usr/share/mingw/toolchain-i686-w64-mingw32.cmake ..
    make
    
Test with `wine` You may need to bring in `libwinpthread-1.dll` with

    cp /usr/i686-w64-mingw32/bin/libwinpthread-1.dll ./
    
Unlike on Linux, `strongtalk` IDE works. Run it with `wine windowsbuild/strongtalk` and the GUI should open. Running tests (`strongtalk-test`) is analogous.

### Bazel

In the `build` directory, run

    make -f ../build.linux/makefile ROOT_DIR=$(cd ..; pwd) dependencies
    
Run `bazel build :all` to build, `bazel run :strongtalk-test` to run tests.

Too bad Bazel is an unwieldy dependency.

## Running it

*Linux support is minimal. It is only possible to run vm tests and scripts from the command line (with `-script`). The GUI stuff (meaning Strongtalk IDE) work only on Windows or under wine.*

Check that the binary works by running ` tools/test3.dlt`, a simple test script. It should not crash and it should print `false`.

    build/strongtalk -script tools/test3.dlt
    
Try also

    build/strongtalk -script tools/test3.dlt

output of this should end with

    Time = 1198
    Testing completed.
    identifier expected

    Halt: 
    Process state: Stopped
    - Stack trace (1, 40)
    [...]

and then it crashes and opens a REPL of sorts. Try evaluating `^3+4`.

## About Smalltalk

Smalltalk can be called a Lisp of the Object Oriented world. The language is fairly minimal and as much functionality as possible is pushed into the standard library. That includes program flow constructs, like if-then-else. All code in the standard library is visible to the programmer and can be easily modified and extended. Smalltalk is similar to Ruby in this respect. Or Emacs.

Good introduction to Smalltalk in Czech is a series of articles [Squeak: návrat do budoucnosti](http://www.root.cz/clanky/squeak-navrat-do-budoucnosti/) by Pavel Křivánek.

## About Strongtalk

Strongtalk is a legendary virtual machine and Smalltalk implementation created from 1995 to 1996 by Longview Technologies, a small startup company. Strongtalk's main features were optional sound type system and the groundbreaking use of JIT compilation. In 1997, Longview Technologies were acquired by Sun Microsystems and the team working on Strongtalk was put to work on the Java HotSpot VM. All work on Strongtalk ceased in favor of Java and Strongtalk has never become a product.

There is a recent (Summer 2015) talk by Gilad Bracha ([youtube.com](www.youtube.com/watch?v=dZ0z0DGMSmU)), former Longview Technologies employe, that goes through the original 1996 slides and gives a retrospective overview of the project.

In 2006, Sun released Strongtalk under the BSD licence. David Greenwald, CEO of the original startup, became leader of the developer community around it. See [http://strongtalk.org/](http://strongtalk.org/) for more on the history of Strongtalk.

Strongtalk was again abandoned in 2011. See the mailing list [strongtalk-general](https://groups.google.com/forum/#!forum/strongtalk-general) to get the Zeitgeist.

## Development

All branches except master are considered to be in flux and may be rebased at any time, this is especially true for the `travis` and `coverity-scan` branches. The reason why I keep my work in the open like this is to run Travis after every config change I make. I then squish all changes into a single commit. Coverity works with travis and to take advantage of it I have to build in public.

I decided never to commit directly to master except from pull requests. Again, this gives Travis a chance to run on the CL and avoids broken builds, which is embarrassing.

### Roadmap

The repository contains binaries. Keeping the Strongtalk image (strongtalk.bst) in git is reasonable, all other binaries must be removed, eventually. Pruned from the git history, too.

It would be interesting to redo some of the performance benchmarks that were done in the past and see how Strongtalk stands (and how the world has moved ; )

It should be possible to run gcov, valgrind, helgrind, various sanitizers in GCC and clang etc. without important errors. Leaked memory at exist is ok, anything else (leaks while running, jumps based on undefined data) is not.

Coverity should find nothing.

There should be unit tests. For now, the strongtalk-test binary requires initialized VM. Starting the VM is fast, but not instantaneous, especially not under Valgrind and such tools.

Testing framework being used is ancient and generally abandoned. It prints a lot of output when reporting results and does not return !0 on failure. New tests should be written with something else. Google Test is widely used and has similar API as the old one in the xUnit tradition. CHECK is cool, but it is "different".

As a far more ambitious goal, it would be "fun" to rewrite the interpreter in C and porting it to other architectures, like x86_64 or ARM. Smalltalks generally provide a poor offering when it comes to multithreading. Threads inside Strongtalk map 1:1 to operating system threads, but only one of them may be running at the same time. Strongtalk provides concurency, but no parallelism. Compared to many other Smalltalks, this is quite advanced stuff, but not enough for the 21th century. It might be possible to "sell" Strongtalk as an internet-of-things "solution", although say Lua does make much better deal at that. Well...