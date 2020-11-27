## Laboratory work VII [![Build Status](https://travis-ci.com/puchkovki/lab07-Hunter.svg?branch=master)](https://travis-ci.com/puchkovki/lab07-Hunter)

Данная лабораторная работа посвещена изучению систем управления пакетами на примере **Hunter**

```sh
$ open https://github.com/ruslo/hunter
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab07** на сервисе **GitHub**
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
$ export GITHUB_USERNAME=<имя_пользователя>
$ alias gsed=sed
```

```sh
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace ~
$ source scripts/activate
```

```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab06 projects/lab07
Cloning into 'projects/lab07'...
Unpacking objects: 100% (50/50), done.
$ cd projects/lab07
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab07
```

```sh
$ mkdir -p cmake # flag p: no error if existing, make parent directories as needed
$ wget https://raw.githubusercontent.com/cpp-pm/gate/master/cmake/HunterGate.cmake -O cmake/HunterGate.cmake # download HunterGate
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.112.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 17070 (17K) [text/plain]
Saving to: ‘cmake/HunterGate.cmake’
$ gsed -i '/cmake_minimum_required(VERSION 3.4)/a\ # add package GTest

include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.23.251.tar.gz"
    SHA1 "5659b15dc0884d4b03dbd95710e6a1fa0fc3258d"
)
' CMakeLists.txt
```

```sh
$ git rm -rf third-party/gtest
rm 'third-party/gtest'
$ gsed -i '/set(PRINT_VERSION_STRING "v\${PRINT_VERSION}")/a\

hunter_add_package(GTest)
find_package(GTest CONFIG REQUIRED)
' CMakeLists.txt
$ gsed -i 's/add_subdirectory(third-party/gtest)//' CMakeLists.txt
$ gsed -i 's/gtest_main/GTest::gtest_main/' CMakeLists.txt
```

```sh
$ cmake -H. -B_builds -DBUILD_TESTS=ON
-- [hunter] Calculating Toolchain-SHA1
-- [hunter] Calculating Config-SHA1
-- [hunter] HUNTER_ROOT: /home/puchkovki/.hunter
-- [hunter] [ Hunter-ID: 5659b15 | Toolchain-ID: 511a137 | Config-ID: 8a1641b ]
-- [hunter] GTEST_ROOT: /home/puchkovki/.hunter/_Base/5659b15/511a137/8a1641b/Install (ver.: 1.10.0)
-- Configuring done
-- Generating done
-- Build files have been written to: /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/_builds
$ cmake --build _builds
-- [hunter] Calculating Toolchain-SHA1
-- [hunter] Calculating Config-SHA1
-- [hunter] HUNTER_ROOT: /home/puchkovki/.hunter
-- [hunter] [ Hunter-ID: 5659b15 | Toolchain-ID: 511a137 | Config-ID: 8a1641b ]
-- [hunter] GTEST_ROOT: /home/puchkovki/.hunter/_Base/5659b15/511a137/8a1641b/Install (ver.: 1.10.0)
-- Configuring done
-- Generating done
-- Build files have been written to: /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/_builds
[ 50%] Built target print
Scanning dependencies of target check
[ 75%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[100%] Linking CXX executable check
[100%] Built target check
$ cmake --build _builds --target test # run test
Running tests...
Test project /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/_builds
    Start 1: check
1/1 Test #1: check ............................   Passed    0.39 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.55 sec
$ ls -la $HOME/.hunter
total 0
drwxrwxrwx 1 puchkovki puchkovki 4096 Apr  2 12:52 .
drwxr-xr-x 1 puchkovki puchkovki 4096 Apr  3 14:34 ..
drwxrwxrwx 1 puchkovki puchkovki 4096 Apr  2 13:02 _Base
```

```sh
$ git clone https://github.com/cpp-pm/hunter $HOME/projects/hunter
Cloning into '/home/puchkovki/projects/hunter'...
Checking out files: 100% (2761/2761), done.
$ export HUNTER_ROOT=$HOME/projects/hunter
$ rm -rf _builds
$ cmake -H. -B_builds -DBUILD_TESTS=ON
-- The C compiler identification is GNU 7.4.0
-- The CXX compiler identification is GNU 7.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- [hunter] Calculating Toolchain-SHA1
-- [hunter] Calculating Config-SHA1
-- [hunter] HUNTER_ROOT: /home/puchkovki/projects/hunter
-- [hunter] [ Hunter-ID: xxxxxxx | Toolchain-ID: 511a137 | Config-ID: 2f6b703 ]
-- [hunter] GTEST_ROOT: /home/puchkovki/projects/hunter/_Base/xxxxxxx/511a137/2f6b703/Install (ver.: 1.10.0)
-- [hunter] Building GTest
-- Configuring done
-- Generating done
-- Build files have been written to: /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/_builds
$ cmake --build _builds
Scanning dependencies of target print
[ 25%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
Scanning dependencies of target check
[ 75%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[100%] Linking CXX executable check
[100%] Built target check
$ cmake --build _builds --target test
Running tests...
Test project /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/_builds
    Start 1: check
1/1 Test #1: check ............................   Passed    0.09 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.16 sec
```

```sh
$ cat $HUNTER_ROOT/cmake/configs/default.cmake | grep GTest # show default version
hunter_default_version(GTest VERSION 1.7.0-hunter-6)
  hunter_default_version(GTest VERSION 1.10.0)
$ cat $HUNTER_ROOT/cmake/projects/GTest/hunter.cmake # show all versions
# Copyright (c) 2013, Ruslan Baratov
# All rights reserved.

# !!! DO NOT PLACE HEADER GUARDS HERE !!!

include(hunter_add_version)
include(hunter_cacheable)
include(hunter_download)
include(hunter_pick_scheme)
include(hunter_cmake_args)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter.tar.gz"
    SHA1
    1ed1c26d11fb592056c1cb912bd3c784afa96eaa
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-1"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-1.tar.gz"
    SHA1
    0cb1dcf75e144ad052d3f1e4923a7773bf9b494f
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.10.0-p1"
    URL
    "https://github.com/hunter-packages/googletest/archive/v1.10.0-p1.tar.gz"
    SHA1
    06a1f667f200ff94d38b608e44c3c8061c7b8f2f
)

if(HUNTER_GTest_VERSION VERSION_LESS 1.8.0)
  set(_gtest_license "LICENSE")
else()
  set(_gtest_license "googletest/LICENSE")
endif()

# gtest_force_shared_crt prevents GoogleTest from modifying options
# rather than forcing it to use shared libraries
hunter_cmake_args(
    GTest
    CMAKE_ARGS
    HUNTER_INSTALL_LICENSE_FILES=${_gtest_license}
    gtest_force_shared_crt=TRUE
)

hunter_pick_scheme(DEFAULT url_sha1_cmake)
hunter_cacheable(GTest)
hunter_download(PACKAGE_NAME GTest PACKAGE_INTERNAL_DEPS_ID 1)
$ mkdir cmake/Hunter
$ cat > cmake/Hunter/config.cmake <<EOF # add source file for the next lab 	
hunter_config(GTest VERSION 1.7.0-hunter-9)
EOF
# add LOCAL in HunterGate function
```

```sh
$ mkdir demo
$ cat > demo/main.cpp <<EOF
#include <print.hpp>

#include <cstdlib>

int main(int argc, char* argv[])
{
  const char* log_path = std::getenv("LOG_PATH");
  if (log_path == nullptr)
  {
    std::cerr << "undefined environment variable: LOG_PATH" << std::endl;
    return 1;
  }
  std::string text;
  while (std::cin >> text)
  {
    std::ofstream out{log_path, std::ios_base::app};
    print(text, out);
    out << std::endl;
  }
}
EOF

$ gsed -i '/endif()/a\

add_executable(demo ${CMAKE_CURRENT_SOURCE_DIR}/demo/main.cpp)
target_link_libraries(demo print)
install(TARGETS demo RUNTIME DESTINATION bin)
' CMakeLists.txt
```

```sh
$ mkdir tools
$ git submodule add https://github.com/ruslo/polly tools/polly
Cloning into '/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/tools/polly'...
Receiving objects: 100% (6407/6407), 1.63 MiB | 526.00 KiB/s, done.
Resolving deltas: 100% (4408/4408), done.
$ tools/polly/bin/polly.py --test # use polly command for building instead of cmake
Python version: 3.6
Build dir: /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/_builds/default
Execute command: [
  `which`
  `cmake`
]

[/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07]> "which" "cmake"

/usr/bin/cmake
Execute command: [
  `cmake`
  `--version`
]

[/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07]> "cmake" "--version"

cmake version 3.10.2

CMake suite maintained and supported by Kitware (kitware.com/cmake).
Execute command: [
  `cmake`
  `-H.`
  `-B/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/_builds/default`
  `-DCMAKE_TOOLCHAIN_FILE=/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/tools/polly/default.cmake`
]

Run tests
Execute command: [
  `ctest`
]

[/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/_builds/default]> "ctest"

*********************************
No test configuration file found!
*********************************
Usage

  ctest [options]

-
Log saved: /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/_logs/polly/default/log.txt
-
Generate: 0:03:41.145670s
Build: 0:00:07.869776s
Test: 0:00:00.835556s
-
Total: 0:03:49.868820s
-
SUCCESS
$ tools/polly/bin/polly.py --install
[ 50%] Built target print
[100%] Built target demo
Install the project...
-
Log saved: /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/_logs/polly/default/log.txt
-
Generate: 0:00:47.928894s
Build: 0:00:03.116745s
-
Total: 0:00:51.124709s
-
SUCCESS
$ tools/polly/bin/polly.py --toolchain clang-cxx14
Python version: 3.6
Build dir: /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/_builds/clang-cxx14
Execute command: [
  `which`
  `cmake`
]

[/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07]> "which" "cmake"

/usr/bin/cmake
Execute command: [
  `cmake`
  `--version`
]

[/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07]> "cmake" "--version"

cmake version 3.10.2

CMake suite maintained and supported by Kitware (kitware.com/cmake).
Execute command: [
  `cmake`
  `-H.`
  `-B/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/_builds/clang-cxx14`
  `-GUnix Makefiles`
  `-DCMAKE_TOOLCHAIN_FILE=/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/tools/polly/clang-cxx14.cmake`
]

[/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07]> "cmake" "-H." "-B/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/_builds/clang-cxx14" "-GUnix Makefiles" "-DCMAKE_TOOLCHAIN_FILE=/mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/tools/polly/clang-cxx14.cmake"

-- [polly] Used toolchain: clang / c++14 support
Scanning dependencies of target print
[ 25%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
Scanning dependencies of target demo
[ 75%] Building CXX object CMakeFiles/demo.dir/demo/main.cpp.o
[100%] Linking CXX executable demo
[100%] Built target demo
-
Log saved: /mnt/c/Users/dns/Documents/GitHub/puchkovki/workspace/projects/lab07/_logs/polly/clang-cxx14/log.txt
-
Generate: 0:03:04.046021s
Build: 0:00:05.530102s
-
Total: 0:03:09.627254s
-
SUCCESS
```

## Report

```sh
$ popd
$ export LAB_NUMBER=07
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```

## Homework

### Задание
1. Создайте cвой hunter-пакет.

## Links

- [Create Hunter package](https://docs.hunter.sh/en/latest/creating-new/create.html)
- [Custom Hunter config](https://github.com/ruslo/hunter/wiki/example.custom.config.id)
- [Polly](https://github.com/ruslo/polly)

```
Copyright (c) 2015-2020 The ISC Authors
```
