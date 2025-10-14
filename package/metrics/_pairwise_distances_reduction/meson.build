_radius_neighbors_classmode = py.extension_module(
  '_radius_neighbors_classmode',
  '_radius_neighbors_classmode.pyx',
  subdir: 'package/metrics/_pairwise_distances_reduction',
  # only fails generating cpp, works fine with default (c) ...
  override_options: ['cython_language=cpp'],
  cython_args: cython_args,
  install: true
)
