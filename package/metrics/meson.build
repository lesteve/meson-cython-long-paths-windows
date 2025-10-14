working = py.extension_module(
  'working',
  'working.pyx',
  subdir: 'package/metrics',
  override_options: ['cython_language=cpp'],
  cython_args: cython_args,
  install: true
)

subdir('_pairwise_distances_reduction')
