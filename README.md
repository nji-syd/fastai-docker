# Dockerfiles for the fast.ai library supporting GPU
The Docker files download the fast.ai repo and build the Python environment as defined in the environment.yml file provided by fastai. 
The files build upon nvidia/cuda images. 

Running the container starts a jupyter notebook at localhost:8888

jupyter password: fastai

### Comtents

The repo contains two docker files 

 1. fastai.latest.cuda8
 2. fastai.latest.cuda9

The docker files support nvidia-docker for versions 8 and 9 of cuda respectively. There are two differences between the files:

 1. The cuda 8 version inherits from an ubuntu16.04 image with CUDA 8 installed whereas the cuda 9 version has cuda 9 installed. Both images are sourced from NVIDIA.
```
FROM nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04
```
```
FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04
```

 2. The cuda 8 version installs the cuda80 python package rather than the cuda90 package for version 9. The Fastai python environment is created from the environment.yml file included in the Fastai github repository. To support cuda 8, we replace the cuda90 python package provided in environment.yml with the cuda80 package:

```
# clone fastai repo
RUN git clone https://github.com/fastai/fastai.git /usr/local/fastai

# fastai.latest.cuda8: replace cuda90 package with cuda80 for host machines supporting cuda8
RUN sed -i -e 's/cuda90/cuda80/' /usr/local/fastai/environment.yml
```

### Requirements
Requires docker and [nvidia-docker](https://github.com/NVIDIA/nvidia-docker) on host machine.
### Quickstart

##### Build Docker Image
```sh
mkdir docker
cd docker
git clone https://github.com/nji-syd/fastai-docker
cd fastai-docker
docker build -f fastai.latest.cuda9 -t fastai .
```
##### Run Image
```sh
nvidia-docker run --rm -d --name fastai -p 8888:8888 -v /home:/home fastai
```

##### Attach with Command Line Access (if required)
```sh
docker exec -it fastai-gpu bash
```
##### Jupyter Notebook
```sh
localhost:8888
```

