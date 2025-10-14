Reproducer for meson issue with Cython related to`MAX_PATH` on Windows. It was originally seen in scikit-learn, see https://github.com/scikit-learn/scikit-learn/pull/31212 for more details.

### To reproduce the issue

```
meson setup build --buildtype=release
ninja -C build
```

Fails with:
```
fatal error C1083: Cannot open compiler generated file: '': Invalid argument
```

Output with more details:
```
[1/2] Compiling C++ object package/metrics/_pairwise_dist...stances_reduction__radius_neighbors_classmode.pyx.cpp.ob
FAILED: [code=1] package/metrics/_pairwise_distances_reduction/_radius_neighbors_classmode.cp313-win_amd64.pyd.p/meson-generated_package_metrics__pairwise_distances_reduction__radius_neighbors_classmode.pyx.cpp.obj
"cl" "-Ipackage\metrics\_pairwise_distances_reduction\_radius_neighbors_classmode.cp313-win_amd64.pyd.p" "-Ipackage\metrics\_pairwise_distances_reduction" "-I..\package\metrics\_pairwise_distances_reduction" "-IC:\Users\rjxQE\.local\share\mamba\envs\test\Include" "/MD" "/nologo" "/showIncludes" "/utf-8" "/Zc:__cplusplus" "/W2" "/EHsc" "/std:c++14" "/permissive-" "/O2" "/Gw" "/Fdpackage\metrics\_pairwise_distances_reduction\_radius_neighbors_classmode.cp313-win_amd64.pyd.p\meson-generated_package_metrics__pairwise_distances_reduction__radius_neighbors_classmode.pyx.cpp.pdb" /Fopackage/metrics/_pairwise_distances_reduction/_radius_neighbors_classmode.cp313-win_amd64.pyd.p/meson-generated_package_metrics__pairwise_distances_reduction__radius_neighbors_classmode.pyx.cpp.obj "/c" package/metrics/_pairwise_distances_reduction/_radius_neighbors_classmode.cp313-win_amd64.pyd.p/package/metrics/_pairwise_distances_reduction/_radius_neighbors_classmode.pyx.cpp
package/metrics/_pairwise_distances_reduction/_radius_neighbors_classmode.cp313-win_amd64.pyd.p/package/metrics/_pairwise_distances_reduction/_radius_neighbors_classmode.pyx.cpp(5312): warning C4551: function call missing argument list
package/metrics/_pairwise_distances_reduction/_radius_neighbors_classmode.cp313-win_amd64.pyd.p/package/metrics/_pairwise_distances_reduction/_radius_neighbors_classmode.pyx.cpp(5319): warning C4551: function call missing argument list
c:\msys64\home\rjxQE\dev\meson-cython-long-paths-windows\build\package\metrics\_pairwise_distances_reduction\_radius_neighbors_classmode.cp313-win_amd64.pyd.p\package\metrics\_pairwise_distances_reduction\_radius_neighbors_classmode.pyx.cpp : fatal error C1083: Cannot open compiler generated file: '': Invalid argument
ninja: build stopped: subcommand failed.
```

`MAX_PATH` issue seem to depend on the absolute path generated and my git checkout is in a folder like
```
c:\msys64\home\usera\dev\meson-cython-long-paths-windows
```

Side-comment: it only fails in C++ mode i.e. `override_options: ['cython_language=cpp']` for some reason ...
