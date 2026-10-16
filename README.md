# Morse-Sequence

C++ implementation, with Python bindings (pybind11), of Morse sequences and
Morse frames on simplicial complexes. The input is a
[`simplextree`](https://github.com/peekxc/simplextree-py) `SimplexTree`.

The code comes from a research internship at the LIGM (Laboratoire
d'Informatique Gaspard-Monge, UMR 8049, CNRS / Université Gustave Eiffel),
supervised by Laurent Najman. It is a C++ port of an existing Python
implementation, based on the work of L. Najman and G. Bertrand (2025).

> **Status:** research code, version 0.1.x. The API may change. There is no
> automated test suite yet (see [Tests](#tests)), and no benchmark is
> published in this repository yet.

## Requirements

- Python 3.9 or newer
- A C++20 compiler (GCC or Clang)
- Boost headers (only `boost/dynamic_bitset.hpp` is used):
  `sudo apt install libboost-dev` on Debian/Ubuntu, `brew install boost` on macOS
- `meson-python`, `ninja` and `pybind11` below version 3: `pip` installs them
  for you in an isolated build, see the note on pybind11 below

## Install

The package is **not on PyPI yet**. Do not use `pip install morse-sequence`:
the 0.1.0 release there is an old placeholder without the compiled extension.

```bash
git clone https://github.com/thomas-pernot/Morse-Sequence.git
cd Morse-Sequence
python -m venv .venv && source .venv/bin/activate
pip install .
```

To tune the binary for your own CPU (`-march=native`, not portable):

```bash
pip install . -Csetup-args=-Dnative=true
```

### Why pybind11 must be below version 3

`MorseSequence` takes a `SimplexTree` created by the `simplextree` package.
pybind11 only shares C++ types between two extension modules when both were
built with the same internal ABI version. `simplextree` currently ships with
pybind11 2.x, so this project pins `pybind11>=2.13,<3` at build time.
`meson.build` therefore reads the pybind11 headers from the Python package seen
by the build (the version pinned in `pyproject.toml`) instead of letting Meson
search for a system copy, which could be a different version.
If you build with pybind11 3.x, the import works but this call fails:

```
TypeError: __init__(): incompatible constructor arguments.
    1. morse_sequence._core.MorseSequence(arg0: SimplexTree)
```

## Usage

```python
from morse_sequence import MorseSequence
from simplextree import SimplexTree

st = SimplexTree([[1, 2, 3]])   # one triangle
ms = MorseSequence(st)

dec, n_crit_dec = ms.decreasing()
print(n_crit_dec)   # 1
print(dec)          # [([2, 3], [1, 2, 3]), ([2], [1, 2]), ([1], [1, 3]), [3]]

inc, n_crit_inc = ms.increasing()
print(n_crit_inc)   # 1
print(inc)          # [[1], ([3], [1, 3]), ([2], [2, 3]), ([1, 2], [1, 2, 3])]
```

Each sequence is a list of pairs `(face, coface)` of regular simplices, and
single-element entries for critical simplices. The other methods exposed by the
module (boundaries, coboundaries, `Max`, `Min`, reference and co-reference
maps) are declared in `src/morse_cpp/_core/bindings.cpp`.

## Development

```bash
pip install meson-python ninja "pybind11>=2.13,<3" simplextree
pip install --no-build-isolation -e .
```

The editable install rebuilds the extension when you import the package after
a change to the C++ sources.

## Tests

`tests/python_tests/` holds the pure-Python reference implementation checks
and an example script; run them from the repository root, for example:

```bash
python -m tests.python_tests.example
```

These are scripts that print results, not assertions. Turning them into a
`pytest` suite that compares the C++ and Python versions is planned.

## Repository layout

| Path | Content |
| --- | --- |
| `src/morse_cpp/` | C++ sources and the Python package (`morse_sequence`) |
| `src/python/` | Pure-Python reference implementation |
| `src/simplextree-py/` | Vendored copy of simplextree-py (Apache-2.0), used for its C++ headers |
| `external/tsl/` | Vendored `robin-map` (MIT) |
| `tests/` | Example, reference checks and timing scripts |

## License

MIT, see `LICENSE`. The vendored libraries keep their own licenses.