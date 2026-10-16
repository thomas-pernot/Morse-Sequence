project('morse_sequence', 'cpp',
  version : '0.1.6',
  meson_version : '>=1.1.0',
  default_options : ['cpp_std=c++20', 'optimization=3', 'b_lto=true']
)

# -march=native seulement si on le demande (sinon le binaire n'est pas portable)
if get_option('native')
  add_project_arguments('-march=native', language : 'cpp')
endif
add_project_arguments('-funroll-loops', language : 'cpp')

py = import('python').find_installation(pure : false)
pybind11_dep = dependency('pybind11', required : true)
boost_dep = dependency('boost', required : true)   # seulement des en-têtes (dynamic_bitset)

simplextree_inc = include_directories('src/simplextree-py/include')
tsl_inc = include_directories('external')

core_sources = files(
  'src/morse_cpp/_core/bindings.cpp',
  'src/morse_cpp/_core/morse_sequence/morse_sequence.cpp',
  'src/morse_cpp/_core/morse_frame/ref_map.cpp',
  'src/morse_cpp/_core/morse_frame/coref_map.cpp',
  'src/morse_cpp/_core/morse_frame/union_find_mf.cpp',
  'src/morse_cpp/_core/morse_frame/utils.cpp'
)

# subdir = dossier relatif à site-packages (au lieu de l'ancien install_dir absolu)
py.extension_module(
  '_core',
  sources             : core_sources,
  include_directories : [include_directories('src/morse_cpp/_core'), simplextree_inc, tsl_inc],
  dependencies        : [pybind11_dep, boost_dep],
  install             : true,
  subdir              : 'morse_sequence'
)

py.install_sources('src/morse_cpp/__init__.py', subdir : 'morse_sequence')