FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04 as base

LABEL maintainer="chaffixdev@gmail.com"

# Needed for string substitution 
SHELL ["/bin/bash", "-c"]
# Pick up some TF dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        curl \
        pkg-config \
        software-properties-common \
        unzip \
        vim \
        libjpeg-dev \
        libpng-dev \
        && rm -rf /var/lib/apt/lists/*


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

RUN pip install -U \
    pyyaml \
    scipy \
    ipython \
    mkl \
    mkl-include \
    cython \
    typing

RUN pip install pandas \
        numpy \
        keras \
        sklearn \
        torchvision
