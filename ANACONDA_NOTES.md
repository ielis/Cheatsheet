# Anaconda Python
My notes on usage of Python from Anaconda and solutions to problems that I've encountered.

## Virtual environments
Basic usage of virtual environments managed by Anaconda

```bash
# create environment, install also a few packages
conda create -n test_env python=3.6 pip ipykernel

# list present virtual environments
conda env list

# activate the test_env virtual environment (the command prompt should change)
conda activate test_env

# deactivate the current virtual environment
conda deactivate

# export configuration to YAML file for allowing to clone the environment
conda env export -n test_env > environment.yml

# create an environment `other_venv` based on an environment YAML file
conda env create -n other_venv -f path/to/environment.yml

# remove virtual environment
conda remove --name test_venv --all
```
> - `conda` is a tool for managing and deploying applications, environments and packages
> - `test_env` is the name of the environment in this example

### Update all available packages

```bash
conda update --all
```

### Install non-anaconda packages

In order to install package that is not managed by anaconda (e.g PyVCF, PySAM, ..):

```bash
# enter the environment
conda activate test_env

# install package with pip
pip install PyVCF

# or use another channel
conda install --channel "bioconda" pyvcf
```

### Use virtual environment as Jupyter kernel

**Add** virtual environments among Jupyter kernels. The particular kernel **must** be activated at the moment!
```bash
conda activate test_env
pip install ipykernel
python -m ipykernel install --user --name test_env --display-name "Python (whatever you want to call it)"
```

**Remove** the unused kernel:
```bash
# kernels should be somewhere at path like `$HOME/.local/share/jupyter/kernels`
jupyter kernelspec uninstall test_env
```
