FROM ubuntu:16.04

LABEL maintainer="chaffixdev@gmail.com"

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
COPY cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64-deb /
RUN ls /
RUN dpkg -i /cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64-deb
RUN apt-key add /var/cuda-repo-9-0-local/7fa2af80.pub
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y cuda


ARG LD_LIBRARY_PATH=/usr/local/cuda/lib64
ARG PATH=$PATH:/usr/local/cuda-9.0/bin

# install python 3.6
RUN add-apt-repository ppa:deadsnakes/ppa
RUN apt-get update && apt-get install -y --no-install-recommends \
    python3.6 \
    python3.6-dev

RUN apt-get install wget
RUN wget https://bootstrap.pypa.io/get-pip.py
RUN python3.6 get-pip.py

RUN pip3.6 install -U pip
RUN pip3.6 install -U setuptools

# Some TF tools expect a "python" binary
RUN ln -s /usr/bin/python3.6 /usr/bin/python

# check versions
RUN python -V
RUN pip --version

## TF prereqs
COPY libcudnn7_7.4.2.24-1+cuda9.0_amd64.deb /
RUN dpkg -i /libcudnn7_7.4.2.24-1+cuda9.0_amd64.deb
RUN apt-get update

RUN apt-get update && apt-get install -y --no-install-recommends \
        cuda-command-line-tools-9-0 \
        cuda-cublas-9-0 \
        cuda-cufft-9-0 \
        cuda-curand-9-0 \
        cuda-cusolver-9-0 \
        cuda-cusparse-9-0 \
        libcudnn7=7.4.2.24-1+cuda9.0 \
        libfreetype6-dev \
        libhdf5-serial-dev \
        libzmq3-dev
#https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64/nvinfer-runtime-trt-repo-ubuntu1604-4.0.1-ga-cuda9.0_1-1_amd64.deb
COPY nvinfer-runtime-trt-repo-ubuntu1604-4.0.1-ga-cuda9.0_1-1_amd64.deb /
RUN dpkg -i /nvinfer-runtime-trt-repo-ubuntu1604-4.0.1-ga-cuda9.0_1-1_amd64.deb
RUN apt-get update && \
        apt-get install nvinfer-runtime-trt-repo-ubuntu1604-4.0.1-ga-cuda9.0 \
        && apt-get update \
        && apt-get install -y --no-install-recommends libnvinfer4=4.1.2-1+cuda9.0 \
        && apt-get clean \
        && rm -rf /var/lib/apt/lists/*

# For CUDA profiling, TensorFlow requires CUPTI.
ENV LD_LIBRARY_PATH /usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH

ARG PYTHON=python3
ARG PIP=pip3

# See http://bugs.python.org/issue19846
ENV LANG C.UTF-8

RUN pip install tensorflow-gpu
