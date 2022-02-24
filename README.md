# gpu-burn
Multi-GPU CUDA stress test
http://wili.cc/blog/gpu-burn.html

# Prerequisite
docker container NVIDIA runtine in place. If this is missing, please consult our documentation here: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker, it is just a few install commands

# I recommend to use our NGC container, you just have to have the  
docker run --rm --gpus all nvidia/cuda:11.4.1-devel-ubuntu20.04 /bin/bash -c "apt update && apt install -y git && git clone https://github.com/wilicc/gpu-burn && cd gpu-burn && make -j && ./gpu_burn 600"

# Typical output
root@6649bccfb3a4:/home/thomass/gpu-burn# ./gpu_burn 10
GPU 0: NVIDIA RTX A6000 (UUID: GPU-5db0a814-1f18-6948-b810-636d23e7d82f)       
Initialized device 0 with 48682 MB of memory (47940 MB available, using 43146 MB of it), using FLOATS       
Results are 16777216 bytes each, thus performing 2694 iterations       
60.0%  proc'd: 2694 (7987 Gflop/s)   errors: 0   temps: 43 C       
	Summary at:   Thu Feb 24 12:20:06 UTC 2022      

90.0%  proc'd: 5388 (15256 Gflop/s)   errors: 0   temps: 43 C      
	Summary at:   Thu Feb 24 12:20:09 UTC 2022       

100.0%  proc'd: 8082 (15247 Gflop/s)   errors: 0   temps: 45 C     
Killing processes.. Freed memory for dev 0      
Uninitted cublas     
done     

Tested 1 GPUs:    
	GPU 0: OK     
root@6649bccfb3a4:/home/thomass/gpu-burn#    

# Easy docker build and run
```
git clone https://github.com/wilicc/gpu-burn
cd gpu-burn
docker build -t gpu_burn .
docker run --rm --gpus all gpu_burn
```

# Building
To build GPU Burn:

`make`

To remove artifacts built by GPU Burn:

`make clean`

GPU Burn builds with a default Compute Capability of 5.0.
To override this with a different value:

`make COMPUTE=<compute capability value>`

CFLAGS can be added when invoking make to add to the default
list of compiler flags:

`make CFLAGS=-Wall`

LDFLAGS can be added when invoking make to add to the default
list of linker flags:

`make LDFLAGS=-lmylib`

NVCCFLAGS can be added when invoking make to add to the default
list of nvcc flags:

`make NVCCFLAGS=-ccbin <path to host compiler>`

CUDAPATH can be added to point to a non standard install or
specific version of the cuda toolkit (default is 
/usr/local/cuda):

`make CUDAPATH=/usr/local/cuda-<version>`

CCPATH can be specified to point to a specific gcc (default is
/usr/bin):

`make CCPATH=/usr/local/bin`

# Usage

    GPU Burn
    Usage: gpu_burn [OPTIONS] [TIME]
    
    -d	Use doubles
    -tc	Use Tensor cores
    -h	Show this help message
    
    Example:
    gpu-burn -d 3600
