# Morse-Sequence

Repository created for the report of an internship as an assistant of research at ESIEE PARIS under Mr. NAJMAN L.
Based on the library SimplexTree to compute Morse Sequences and F-Sequences from a simplicial complex.

## Prerequisites

You will first need to install the library SimplexTree either via pip or locally.
Refer to the following for more information: [simplextree-py](https://github.com/peekxc/simplextree-py)

C++ and Python compilers are also necessary.

## Install 

The easiest way to install the package is via the platform-specific [wheels](https://pythonwheels.com/) on [pypi](https://pypi.org/project/morse-sequence/).

```bash
pip install morse-sequence
```

### Building & Developing

If you would like to build the package yourself for development reasons, a typical workflow is to install the [build-time dependencies](https://pip.pypa.io/en/stable/reference/build-system/pyproject-toml/#build-time-dependencies) first:

```bash
python -m pip install meson-python ninja pybind11 numpy
```

Then, build and install the package

```bash
git clone --recurse-submodules https://github.com/thomas-pernot/Morse-Sequence.git
cd Morse-Sequence
python -m pip install --no-build-isolation --editable .
```

This allows you to modify the code without reinstalling after every change.

## Usage example

```python
from morse_sequence import MorseSequence
from simplextree import SimplexTree

st = SimplexTree([[1,2,3]]) # Creation of simplicial complex via the library SimplexTree
ms = MorseSequence(st) # MorseSequence created

morse_seq_dec, n_crit_dec = ms.ms_decreasing(st) # Computation of a decreasing Morse Sequence on st and its critical simplices
print(f"Critical simplices = {n_crit_dec}") # Critical simplices = 1
print(f"Decreasing Morse Sequence = {morse_seq_dec}") # Decreasing Morse Sequence = [((2, 3), (1, 2, 3)), ((3,), (1, 3)), ((1,), (1, 2)), [(2,)]]

morse_seq_inc, n_crit_inc = ms.ms_increasing(st) # Computation of an increasing Morse Sequence on st and its critical simplices
print(f"Critical simplices = {n_crit_inc}") # Critical simplices = 1
print(f"Increasing Morse Sequence = {morse_seq_inc}") # Increasing Morse Sequence = [[1], ([3], [1, 3]), ([2], [2, 3]), ([1, 2], [1, 2, 3])]

```
