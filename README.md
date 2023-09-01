# ubuntu-22.04-tf-gpu-install
Notes for installing tensorflow with gpu support on Ubuntu 22.04

Based on https://www.tensorflow.org/install/pip

# create conda environment
`conda create --name <your_env_name> python=3.10`

`conda activate <your_env_name>`

It's a good idea to close this terminal window and open a new one

# Install required packages

Make sure you have activated your new environment first

`conda install -c conda-forge cudatoolkit=11.8.0`

`pip install nvidia-cudnn-cu11==8.6.0.163`

`mkdir -p $CONDA_PREFIX/etc/conda/activate.d`

`echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"))' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh`

`echo 'export LD_LIBRARY_PATH=$CUDNN_PATH/lib:$CONDA_PREFIX/lib/:$LD_LIBRARY_PATH' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh`

`pip install --upgrade pip`

`pip install tensorflow==2.13.*`

To Fix Ubuntu 22.04 specific error

**Install NVCC**

`conda install -c nvidia cuda-nvcc=11.3.58`

**Configure the XLA cuda directory**

`mkdir -p $CONDA_PREFIX/etc/conda/activate.d`

`printf 'export XLA_FLAGS=--xla_gpu_cuda_data_dir=$CONDA_PREFIX/lib/\n' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh`

`source $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh`

Copy libdevice file to the required path

`mkdir -p $CONDA_PREFIX/lib/nvvm/libdevice`

`cp $CONDA_PREFIX/lib/libdevice.10.bc $CONDA_PREFIX/lib/nvvm/libdevice/`

**Verify the GPU setup:**

`python3 -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"`


