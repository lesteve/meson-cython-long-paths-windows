project(
  'package',
  'c', 'cpp', 'cython',
  version: '0.1',
  license: 'BSD-3',
  meson_version: '>= 1.1.0',
  default_options: [
    'c_std=c11',
    'cpp_std=c++14',
  ],
)

cc = meson.get_compiler('c')
cpp = meson.get_compiler('cpp')
cython = meson.get_compiler('cython')

_global_c_args = cc.get_supported_arguments(
  '-Wno-unused-but-set-variable',
  '-Wno-unused-function',
  '-Wno-conversion',
  '-Wno-misleading-indentation',
)
add_project_arguments(_global_c_args, language : 'c')

# We need -lm for all C code (assuming it uses math functions, which is safe to
# assume for scikit-learn). For C++ it isn't needed, because libstdc++/libc++ is
# guaranteed to depend on it.
m_dep = cc.find_library('m', required : false)
if m_dep.found()
  add_project_link_arguments('-lm', language : 'c')
endif

py = import('python').find_installation(pure: false, required: true)

# Copy all the .py files to the install dir, rather than using
# py.install_sources and needing to list them explicitly one by one
install_subdir('package', install_dir: py.get_install_dir())

subdir('package')
