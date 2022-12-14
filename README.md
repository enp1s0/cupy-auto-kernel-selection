# How to setup automatic kernele selection enabled cupy

Before starting the following instructions, activate your Python environment such as `pyenv virtualenv`.

## Install required Python packages using pip
```bash
git clone https://github.com/enp1s0/cupy-auto-kernel-selection
cd cupy-auto-kernel-selection
pip install -r requirements.txt
```

## Install cuMpSGEMM and its Python module
```bash
git clone https://github.com/enp1s0/cuMpSGEMM --recursive -b 29-exp-stats
cd cuMpSGEMM
mkdir build
cd build
cmake ..
make -j4
export CUMPSGEMM_PATH=$(pwd)/libcumpsgemm.so
echo "Append a path ${CUMPSGEMM_PATH} to an environmental variable LD_PRELOAD"
export LD_PRELOAD=$CUMPSGEMM_PATH:$LD_PRELOAD
cd ../python
python setup.py install
cd ../..
```

## Install custom CuPy
On a node that is equipped with a GPU
```bash
git clone https://github.com/enp1s0/cupy --recursive -b 1-fp16tcec_available
cd cupy
python setup.py install
cd ..
```

## Source code preparation

```python
import cumpsgemm_hijack_control as chc

chc.enable_exp_stats()
# arg = (ignore_threshold, lost_threshold)
chc.set_exp_stats_params(1 / (2**50), 1 / (2**20))
chc.set_global_lost_ratio_threshold(0.1)
```
