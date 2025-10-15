Reproducer for meson issue with Cython related to`MAX_PATH` on Windows. It was originally seen in scikit-learn, see https://github.com/scikit-learn/scikit-learn/pull/31212 for more details.

### To reproduce the issue

```
meson setup build --buildtype=release
ninja -C build
```

`ninja` fails with:
```
fatal error C1083: Cannot open compiler generated file: '': Invalid argument
```

Output with more details:
```
The Meson build system
Version: 1.8.4
Source dir: C:\msys64\home\rjxQE\dev\meson-cython-long-paths-windows
Build dir: C:\msys64\home\rjxQE\dev\meson-cython-long-paths-windows\build
Build type: native build
Project name: package
Project version: 0.1
C compiler for the host machine: cl (msvc 19.38.33133 "Microsoft (R) C/C++ Optimizing Compiler Version 19.38.33133 for x64")
C linker for the host machine: link link 14.38.33133.0
C++ compiler for the host machine: cl (msvc 19.38.33133 "Microsoft (R) C/C++ Optimizing Compiler Version 19.38.33133 for x64")
C++ linker for the host machine: link link 14.38.33133.0
Cython compiler for the host machine: cython (cython 3.1.4)
Host machine cpu family: x86_64
Host machine cpu: x86_64
Compiler for C supports arguments -Wno-unused-but-set-variable: NO
Compiler for C supports arguments -Wno-unused-function: NO
Compiler for C supports arguments -Wno-conversion: NO
Compiler for C supports arguments -Wno-misleading-indentation: NO
Library m found: NO
Program python3 found: YES (C:\Users\rjxQE\.local\share\mamba\envs\test\python.exe)
Run-time dependency python found: YES 3.13
Build targets in project: 2

package 0.1

  User defined options
    buildtype: release

Found ninja-1.13.1 at C:\Users\rjxQE\.local\share\mamba\envs\test\Library\bin\ninja.EXE

ninja: Entering directory `build'
[3/6] Compiling C++ object package/metrics/_pairwise_dist...istances_reduction__radius_neighbors_classmode.pyx.cpp.ob
FAILED: [code=1] package/metrics/_pairwise_distances_reduction/_radius_neighbors_classmode.cp313-win_amd64.pyd.p/meson-generated_package_metrics__pairwise_distances_reduction__radius_neighbors_classmode.pyx.cpp.obj
"cl" "-Ipackage\metrics\_pairwise_distances_reduction\_radius_neighbors_classmode.cp313-win_amd64.pyd.p" "-Ipackage\metrics\_pairwise_distances_reduction" "-I..\package\metrics\_pairwise_distances_reduction" "-IC:\Users\rjxQE\.local\share\mamba\envs\test\Include" "/MD" "/nologo" "/showIncludes" "/utf-8" "/Zc:__cplusplus" "/W2" "/EHsc" "/std:c++14" "/permissive-" "/O2" "/Gw" "/Fdpackage\metrics\_pairwise_distances_reduction\_radius_neighbors_classmode.cp313-win_amd64.pyd.p\meson-generated_package_metrics__pairwise_distances_reduction__radius_neighbors_classmode.pyx.cpp.pdb" /Fopackage/metrics/_pairwise_distances_reduction/_radius_neighbors_classmode.cp313-win_amd64.pyd.p/meson-generated_package_metrics__pairwise_distances_reduction__radius_neighbors_classmode.pyx.cpp.obj "/c" package/metrics/_pairwise_distances_reduction/_radius_neighbors_classmode.cp313-win_amd64.pyd.p/package/metrics/_pairwise_distances_reduction/_radius_neighbors_classmode.pyx.cpp
package/metrics/_pairwise_distances_reduction/_radius_neighbors_classmode.cp313-win_amd64.pyd.p/package/metrics/_pairwise_distances_reduction/_radius_neighbors_classmode.pyx.cpp(5312): warning C4551: function call missing argument list
package/metrics/_pairwise_distances_reduction/_radius_neighbors_classmode.cp313-win_amd64.pyd.p/package/metrics/_pairwise_distances_reduction/_radius_neighbors_classmode.pyx.cpp(5319): warning C4551: function call missing argument list
c:\msys64\home\rjxQE\dev\meson-cython-long-paths-windows\build\package\metrics\_pairwise_distances_reduction\_radius_neighbors_classmode.cp313-win_amd64.pyd.p\package\metrics\_pairwise_distances_reduction\_radius_neighbors_classmode.pyx.cpp : fatal error C1083: Cannot open compiler generated file: '': Invalid argument
[4/6] Compiling C++ object package/metrics/working.cp313-....pyd.p/meson-generated_package_metrics_working.pyx.cpp.ob
package/metrics/working.cp313-win_amd64.pyd.p/package/metrics/working.pyx.cpp(5307): warning C4551: function call missing argument list
package/metrics/working.cp313-win_amd64.pyd.p/package/metrics/working.pyx.cpp(5314): warning C4551: function call missing argument list
ninja: build stopped: subcommand failed.
```

`MAX_PATH` issue seem to depend on the absolute path generated and my git checkout is in a folder like
```
c:\msys64\home\usera\dev\meson-cython-long-paths-windows
```

Side-comment: it only fails in C++ mode i.e. `override_options: ['cython_language=cpp']` for some reason ...
