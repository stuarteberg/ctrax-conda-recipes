conda recipes for installing Ctrax
==================================

This repo contains recipes for building [Ctrax][1] from source into a [conda][2] environment.
At the moment, this only works on Linux.  Also, no camera interfaces are compiled in.

[1]: http://ctrax.sourceforge.net/
[2]: http://conda.pydata.org/docs/

Prerequisites: Install [Miniconda] and conda-build
-----------------------------------------------------

[Miniconda]: http://conda.pydata.org/miniconda.html

```
# Install miniconda to the prefix of your choice, e.g. /my/miniconda

# LINUX:
wget https://repo.continuum.io/miniconda/Miniconda-latest-Linux-x86_64.sh
bash Miniconda-latest-Linux-x86_64.sh

# MAC:
wget https://repo.continuum.io/miniconda/Miniconda-latest-MacOSX-x86_64.sh
bash Miniconda-latest-MacOSX-x86_64.sh

# Activate conda
CONDA_ROOT=`conda info --root`
source ${CONDA_ROOT}/bin/activate root
```

```
# Install conda-build
source activate root
conda install conda-build jinja2
```

Build ctrax from source
-----------------------

For maximum compatibility, we use conda's version of `gcc` for all c++ packages.
For now, we have to use a special version of that package, obtained from ilastik's
binstar channel.

```
# Make the ilastik channel available to our setup
# (Edits ~/.condarc)
conda config -f --add channels ilastik
```

Build everything:

```
cd ctrax-conda-recipes
conda build ctrax
```

Install your locally-built packages
-----------------------------------

```
# Create a fresh environment and install ctrax
conda create -n ctrax-env ctrax --use-local
```

Run Ctrax
---------

```
source ${CONDA_ROOT}/bin/activate ctrax-env
Ctrax
```

Optional: Upload your packages to [binstar]
-------------------------------------------

[binstar]: http://binstar.org

```
# First, install binstar if necessary
source ${CONDA_ROOT}/bin/activate root
conda install binstar

# Upload
binstar upload ${CONDA_ROOT}/conda-bld/linux-64/*.tar.bz2
```

Others can install Ctrax on their own machine using your binstar channel:

```
conda create -n ctrax-env -c your-channel ctrax
```
