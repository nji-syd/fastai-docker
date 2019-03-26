# Docker files for the fast.ai library supporting GPU

### Update March 2019

Docker files have been updated for FastAI version 1.0

 1. fastai0.7.cuda9
 2. fastai1.0.cuda9
 
Running the container starts a jupyter notebook at localhost:8888

jupyter password: fastai

**Important:** To connect a notebook to the Anaconda fastai environment, open the notebook:

1. Select the Kernel menu 
2. Choose Change Kernel -> env:fastai. 
3. Save the Notebook and fastai becomes the default environment for that notebook.

**Set the default working directory for Jupyter Notebook:**

Edit the dockerfile to set the default working directory for Jupyter. Edit \<path-to-directory> in the line shown below.

```
RUN echo "c.NotebookApp.notebook_dir = '/<path-to-directory>'" >> /root/.jupyter/jupyter_notebook_config.py
```

### Legacy Files (Don't Use)

 1. fastai.cuda8.old
 2. fastai.cuda9.old

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
docker build -f fastai1.0.cuda9 -t fastai .
```
##### Run Image
```sh
nvidia-docker run --rm -d --name fastai -p 8888:8888 -v /home:/home fastai
```

##### Attach with Command Line Access (if required)
```sh
docker exec -it fastai bash
```
##### Jupyter Notebook
```sh
localhost:8888
```
### Blog
Further details can be found in the blog post [here](https://nji-syd.github.io/2018/03/26/up-and-running-with-fast-ai-and-docker/)

### PyTorch no longer supports this GPU because it is too old.
For GPUs with CUDA compatibility equal to 3, install pytorch from source to solve this issue. See the file section:

uncomment to fix error: PyTorch no longer supports this GPU because it is too old.

further details [here](http://forums.fast.ai/t/pytorch-not-working-with-an-old-nvidia-card/14632/2)
