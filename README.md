# Tensorflow GPU Images with Python 3.6.8, CUDA 9.0 / 10.0 support

I'm sure that `tensorflow/tensorflow` will soon migrate to both `CUDA10.0` and `python3.6`. But until then, I've made these images to run some experiments and testing. For those of you interested in `cuml` and `cudf`, they require ~3.6.5+ which means you will have a difficult time running these on the stock tensorflow images, as they are based on `python3.5`.

### GPU based dataframes + processing

- https://github.com/rapidsai/cuml
- https://github.com/rapidsai/cudf

## Current Images

### ub18-tf1.13-cuda10.0

This image has the most current parts. Tensorflow had to be `1.13` because that was the only version compatible with `CUDA 10.0`.  Image was tested on AWS instance `g2.2xlarge`.

```bash
docker=v2
CUDA=10.0
ubuntu=18.04
cuDNN=7.4
tensorflow=1.13.0rc1
```

From the documentation, this image will only work on systems with the latest version of docker. How to run:

```bash
docker run --runtime=nvidia -it ub18-tf1.13-cuda10.0
```

### ub16-tf1.12-cuda9.0

Image was designed for those on ubuntu 16.04 LTS, but wanted a python 3.6 interface. Image was tested on AWS instance `g2.2xlarge`.

```bash
CUDA=9.0
ubuntu=16.04
cuDNN=7.4
tensorflow=1.12
```

```bash
# newer
docker run --runtime=nvidia -it ub18-tf1.13-cuda10.0

# older 
nvidia-docker run -it ub18-tf1.13-cuda10.0
```
