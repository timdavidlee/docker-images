ARG UBUNTU_VERSION=18.04

ARG CUDA=9.0
FROM nvidia/cuda:9.0-base-ubuntu16.04 as base
ARG CUDNN=7.4.1.5-1

LABEL maintainer="chaffixdev@gmail.com"

# Needed for string substitution 
SHELL ["/bin/bash", "-c"]
# Pick up some TF dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        cuda-command-line-tools-9-0 \
        cuda-cublas-9-0 \
        cuda-cufft-9-0 \
        cuda-curand-9-0 \
        cuda-cusolver-9-0 \
        cuda-cusparse-9-0 \
        curl \
        libcudnn7=7.4.1.5-1+cuda9.0 \
        libfreetype6-dev \
        libhdf5-serial-dev \
        libzmq3-dev \
        pkg-config \
        software-properties-common \
        unzip

RUN [ ${ARCH} = ppc64le ] || (apt-get update && \
        apt-get install nvinfer-runtime-trt-repo-ubuntu1604-4.0.1-ga-cuda9.0 \
        && apt-get update \
        && apt-get install -y --no-install-recommends libnvinfer4=4.1.2-1+cuda9.0 \
        && apt-get clean \
        && rm -rf /var/lib/apt/lists/*)

# For CUDA profiling, TensorFlow requires CUPTI.
ENV LD_LIBRARY_PATH /usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH

ARG PYTHON=python3
ARG PIP=pip3

# See http://bugs.python.org/issue19846
ENV LANG C.UTF-8

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
RUN ln -s /usr/bin/pip3.6 /usr/bin/pip 

RUN pip install tensorflow-gpu
RUN pip install pandas \
        numpy \
        keras \
        sklearn
