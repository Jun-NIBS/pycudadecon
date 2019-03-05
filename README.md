# pyCUDAdecon

[![Documentation Status](https://readthedocs.org/projects/pycudadecon/badge/?version=latest)](https://pycudadecon.readthedocs.io/en/latest/?badge=latest) [![Python 3.6](https://img.shields.io/badge/python-3.6-green.svg)](https://www.python.org/downloads/release/python-360/) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
    

This package provides a python wrapper and convenience functions for [cudaDeconv](https://github.com/dmilkie/cudaDecon) (which is a CUDA/C++ implementation of an accelerated Richardson Lucy Deconvolution<sup>1</sup>).  cudaDeconv was originally written by [Lin Shao](https://github.com/linshaova) and modified by [Dan Milkie](https://github.com/dmilkie), at Janelia Research campus.  This package makes use of a shared library interface that I wrote for cudaDecon while developing [LLSpy](https://github.com/tlambert03/LLSpy) (a Lattice light-sheet post-processing utility), that adds a couple additional kernels for affine transformations and camera corrections.  The code here is mostly extracted from that package to allow it to be used independently of LLSpy.

The main features are:
* radially averaged OTF generation
* OTF interpolation for voxel size independence between PSF and data volumes
* CUDA accelerated deconvolution with a handful of artifact-reducing features. 
* Deskew, Rotation, and general affine transformations
* CUDA-based camera-correction for [sCMOS artifact correction](https://llspy.readthedocs.io/en/latest/camera.html)
* a few context managers for setup/breakdown of GPU-I/O-heavy tasks and convenience functions
* windows, linux, mac

[Documentation](https://pycudadecon.readthedocs.io/en/latest/index.html)


### Why do we need yet another python package for deconvolution?
Honestly, we probably don't.  But since cudaDecon was recently open-sourced, and I had mostly already written this wrapper, it seemed appropriate to release it.  I do think the C++ backbone is well done, and it's relatively mature and tested at this point.  That said, there are some other good python deconvolution packages out there such as [flowdec](https://github.com/hammerlab/flowdec), and probably many others.

### gputools
Similarly, if you've stumbled upon this looking for GPU-accelerated affine transformations, then feel free to try these out, but don't miss the fantastic [gputools](https://github.com/maweigert/gputools) package, which provides OpenCL-acceleration for a number of image processing algorithms including affine transforms (and much more).

## Installation
Precompiled libraries are available for windows, linux, and mac via conda.  
Install [anaconda](https://www.anaconda.com/distribution/#download-section) or [miniconda](https://docs.conda.io/en/latest/miniconda.html), add a couple channels to your config, then install pycudadecon:

```bash
$ conda config --add channels conda-forge
$ conda config --add channels talley
$ conda install pycudadecon
```

### GPU requirements

This software requires a CUDA-compatible NVIDIA GPU.  
The underlying libraries (llspylibs) have been compiled against different versions of the CUDA toolkit.  The required CUDA libraries are bundled in the conda distributions so you don't need to install the CUDA toolkit separately.  If desired, you can pick which version of CUDA you'd like based on your needs, but please note that different versions of the CUDA toolkit have different GPU driver requirements (the OS X build has only been compiled for CUDA 9.0).  To see which version you have installed currently, use `conda list llspylibs`, and to manually select a specific version of llspylibs:

| CUDA  | Linux driver | Win driver | Install With |
| ------------- | ------------ | --------   | -----------  |
| 10.0  | ≥ 410.48     | ≥ 411.31   | `conda install llspylibs=<version>=cu10.0`  |
|  9.0  | ≥ 384.81     | ≥ 385.54   | `conda install llspylibs=<version>=cu9.0`  |

...where `<version>` is the version of llspylibs you'd like to install (use `conda search llspylibs` to see available versions)

## Usage
I'll try to write up better examples eventually, but for now take a look through the tests for examples on use, or have a look at the [documentation](https://pycudadecon.readthedocs.io/en/latest/index.html)
___

<sup>1</sup> D.S.C. Biggs and M. Andrews, Acceleration of iterative image restoration algorithms, Applied Optics, Vol. 36, No. 8, 1997.
