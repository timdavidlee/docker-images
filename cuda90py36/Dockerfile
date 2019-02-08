FROM ubuntu:16.04

RUN apt-get update && apt-get install -y --no-install-recommends\
    build-essential \ 
    apt-utils \
    pkg-config \
    software-properties-common \
    unzip \
    curl

RUN add-apt-repository ppa:graphics-drivers/ppa && apt-get update

# assuming cuda 9
# reference  https://github.com/NVIDIA/nvidia-docker/wiki/CUDA
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends nvidia-384

# install cuda
SHELL ["/bin/bash", "-c"]
COPY cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64.deb .
RUN dpkg -i cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64.deb
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install cuda

# RUN tar -xvf cudnn-8.0-linux-x64-v5.1.tgz -C /usr/local

ARG LD_LIBRARY_PATH=/usr/local/cuda/lib64
ARG PATH=$PATH:/usr/local/cuda-9.0/bin

RUN add-apt-repository ppa:deadsnakes/ppa
RUN apt-get update && apt-get install -y --no-install-recommends \
    python3.6.7 \
    python3-dev \
    python3-pip

RUN pip3 install -U pip
RUN pip3 install -U setuptools

# Some TF tools expect a "python" binary
RUN ln -s /usr/local/bin/python3 /usr/local/bin/python 
RUN ln -s /usr/local/bin/pip3 /usr/local/bin/pip 

## TF prereqs

RUN apt-get update && apt-get install -y --no-install-recommends \
        cuda-command-line-tools-9-0 \
        cuda-cublas-9-0 \
        cuda-cufft-9-0 \
        cuda-curand-9-0 \
        cuda-cusolver-9-0 \
        cuda-cusparse-9-0 \
        libcudnn7=7.4.1.5-1+cuda9.0 \
        libfreetype6-dev \
        libhdf5-serial-dev \
        libzmq3-dev

RUN apt-get update && \
        apt-get install nvinfer-runtime-trt-repo-ubuntu1804-5.0.2-ga-cuda9.0 \
        && apt-get update \
        && apt-get install -y --no-install-recommends libnvinfer5=5.0.2-1+cuda9.0 \
        && apt-get clean \
        && rm -rf /var/lib/apt/lists/*

# For CUDA profiling, TensorFlow requires CUPTI.
ENV LD_LIBRARY_PATH /usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH

ARG PYTHON=python3
ARG PIP=pip3

# See http://bugs.python.org/issue19846
ENV LANG C.UTF-8

RUN pip install tensorflow-gpu