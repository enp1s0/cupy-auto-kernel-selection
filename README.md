# How to setup automatic kernele selection enabled cupy

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
cd ../..
```
