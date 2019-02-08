ARG UBUNTU_VERSION=18.04

ARG CUDA=9.2
FROM nvidia/cuda:9.2-base-ubuntu18.04 as base
ARG CUDNN=7.4.1.5-1

# Needed for string substitution 
SHELL ["/bin/bash", "-c"]
# Pick up some TF dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        cuda-command-line-tools-9-2 \
        cuda-cublas-9-2 \
        cuda-cufft-9-2 \
        cuda-curand-9-2 \
        cuda-cusolver-9-2 \
        cuda-cusparse-9-2 \
        curl \
        libcudnn7=7.4.1.5-1+cuda9.2 \
        libfreetype6-dev \
        libhdf5-serial-dev \
        libzmq3-dev \
        pkg-config \
        software-properties-common \
        unzip

RUN [ ${ARCH} = ppc64le ] || (apt-get update && \
        apt-get install nvinfer-runtime-trt-repo-ubuntu1804-5.0.2-ga-cuda9.2 \
        && apt-get update \
        && apt-get install -y --no-install-recommends libnvinfer5=5.0.2-1+cuda9.2 \
        && apt-get clean \
        && rm -rf /var/lib/apt/lists/*)

# For CUDA profiling, TensorFlow requires CUPTI.
ENV LD_LIBRARY_PATH /usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH

ARG PYTHON=python3
ARG PIP=pip3

# See http://bugs.python.org/issue19846
ENV LANG C.UTF-8

RUN apt-get update && apt-get install -y \
    python3 \
    python3-dev \
    python3-pip

RUN pip3 --no-cache-dir install --upgrade \
    pip \
    setuptools

# Some TF tools expect a "python" binary
RUN ln -s /usr/local/bin/python3 /usr/local/bin/python 
RUN ln -s /usr/local/bin/pip3 /usr/local/bin/pip 

RUN pip install tensorflow-gpu==1.13.0r1
