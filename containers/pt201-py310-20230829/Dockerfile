#!/bin/bash

# Paperspace Dockerfile for Gradient base image
# This docker image is located in Dockerhub registry: noxouille/gradient_base

# ==================================================================
# Module list
# ------------------------------------------------------------------
# python                3.11.0           (apt)
# torch                 2.0.1+cu118      (pip)
# torchvision           0.15.2+cu118     (pip)
# torchaudio            2.0.2+cu118      (pip)
# tensorflow            2.9.2            (pip)
# jax                   0.3.23           (pip)
# transformers          4.21.3           (pip)
# datasets              2.4.0            (pip)
# jupyterlab            3.4.6            (pip)
# numpy                 1.23.4           (pip)
# scipy                 1.9.2            (pip)
# pandas                1.5.0            (pip)
# cloudpickle           2.2.0            (pip)
# scikit-image          0.19.3           (pip)
# scikit-learn          1.1.2            (pip)
# matplotlib            3.6.1            (pip)
# ipython               8.5.0            (pip)
# ipykernel             6.16.0           (pip)
# ipywidgets            8.0.2            (pip)
# cython                0.29.32          (pip)
# tqdm                  4.64.1           (pip)
# gdown                 4.5.1            (pip)
# xgboost               1.6.2            (pip)
# pillow                9.2.0            (pip)
# seaborn               0.12.0           (pip)
# sqlalchemy            1.4.41           (pip)
# spacy                 3.4.1            (pip)
# nltk                  3.7              (pip)
# boto3                 1.24.90          (pip)
# tabulate              0.9.0            (pip)
# future                0.18.2           (pip)
# gradient              2.0.6            (pip)
# jsonify               0.5              (pip)
# opencv-python         4.6.0.66         (pip)
# sentence-transformers 2.2.2            (pip)
# wandb                 0.13.4           (pip)
# nodejs                16.x latest      (apt)
# default-jre           latest           (apt)
# default-jdk           latest           (apt)


# ==================================================================
# Initial setup
# ------------------------------------------------------------------

    # Ubuntu 20.04 as base image
    FROM ubuntu:20.04
    RUN yes| unminimize

    # Set ENV variables
    ENV LANG C.UTF-8
    ENV SHELL=/bin/bash
    ENV DEBIAN_FRONTEND=noninteractive

    ENV APT_INSTALL="apt-get install -y --no-install-recommends"
    ENV PIP_INSTALL="python3 -m pip --no-cache-dir install --upgrade"
    ENV GIT_CLONE="git clone --depth 10"


# ==================================================================
# Tools
# ------------------------------------------------------------------

    RUN apt-get update && \
        $APT_INSTALL \
        aria2 \
        apt-utils \
        gcc \
        make \
        pkg-config \
        apt-transport-https \
        build-essential \
        ca-certificates \
        wget \
        rsync \
        git \
        vim \
        mlocate \
        libssl-dev \
        curl \
        openssh-client \
        unzip \
        unrar \
        zip \
        csvkit \
        emacs \
        joe \
        jq \
        dialog \
        man-db \
        manpages \
        manpages-dev \
        manpages-posix \
        manpages-posix-dev \
        nano \
        iputils-ping \
        sudo \
        ffmpeg \
        libsm6 \
        libxext6 \
        libboost-all-dev \
        cifs-utils \
        software-properties-common


# ==================================================================
# Python
# ------------------------------------------------------------------

    #Based on https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa

    # Adding repository for python3.10
    RUN add-apt-repository ppa:deadsnakes/ppa -y && \

    # Installing python3.10
        $APT_INSTALL \
        python3.10 \
        python3.10-dev \
        python3.10-venv \
        python3-distutils-extra

    # Add symlink so python and python3 commands use same python3.10 executable
    RUN ln -s /usr/bin/python3.10 /usr/local/bin/python3 && \
        ln -s /usr/bin/python3.10 /usr/local/bin/python

    # Installing pip
    RUN curl -sS https://bootstrap.pypa.io/get-pip.py | python3.10
    ENV PATH=$PATH:/root/.local/bin


# ==================================================================
# Installing CUDA packages (CUDA Toolkit 12.2 & CUDNN 8.9.4)
# ------------------------------------------------------------------

    # Based on https://developer.nvidia.com/cuda-toolkit-archive
    # Based on https://developer.nvidia.com/rdp/cudnn-archive
    # Based on https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html#package-manager-ubuntu-install

    # Installing CUDA Toolkit

    RUN wget https://developer.download.nvidia.com/compute/cuda/12.2.1/local_installers/cuda_12.2.1_535.86.10_linux.run && \
        bash cuda_12.2.1_535.86.10_linux.run --silent --toolkit && \
        rm cuda_12.2.1_535.86.10_linux.run

    ENV PATH=$PATH:/usr/local/cuda-12.2/bin
    ENV LD_LIBRARY_PATH=/usr/local/cuda-12.2/lib64

    # Installing CUDNN
    RUN wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin && \
        mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600 && \
        apt-get install dirmngr -y && \
        apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub && \
        add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/ /" && \
        apt-get update && \
        apt-get install libcudnn8=8.9.4.*-1+cuda12.2 -y && \
        apt-get install libcudnn8-dev=8.9.4.*-1+cuda12.2 -y && \
        rm /etc/apt/preferences.d/cuda-repository-pin-600

# ==================================================================
# PyTorch and ComfyUI requirements
# ------------------------------------------------------------------

    # Based on https://github.com/comfyanonymous/ComfyUI/blob/master/requirements.txt
    # Based on https://pytorch.org/get-started/locally/

    RUN $PIP_INSTALL \
        xformers!=0.0.18 \
        torch \
        torchsde \
        einops \
        transformers>=4.25.1 \
        safetensors>=0.3.0 \
        aiohttp \
        accelerate \
        pyyaml \
        Pillow \
        scipy \
        tqdm \
        psutil \
        torchvision \
        torchaudio && \


# ==================================================================
# JAX
# ------------------------------------------------------------------

    # Based on https://github.com/google/jax#pip-installation-gpu-cuda

    # $PIP_INSTALL "jax[cuda12_pip]" -f https://storage.googleapis.com/jax-releases/jax_cuda_releases.html && \
    # $PIP_INSTALL flax==0.7.2 && \


# ==================================================================
# TensorFlow
# ------------------------------------------------------------------

    # Based on https://www.tensorflow.org/install/pip

    # $PIP_INSTALL tensorflow && \


# ==================================================================
# Hugging Face
# ------------------------------------------------------------------
    
    # Based on https://huggingface.co/docs/transformers/installation
    # Based on https://huggingface.co/docs/datasets/installation

    # $PIP_INSTALL datasets && \


# ==================================================================
# JupyterLab
# ------------------------------------------------------------------

    # Based on https://jupyterlab.readthedocs.io/en/stable/getting_started/installation.html#pip

    $PIP_INSTALL jupyterlab==3.4.6


# ==================================================================
# Additional Python Packages
# ------------------------------------------------------------------

    # $PIP_INSTALL \
    #     numpy==1.23.5 \
    #     scipy==1.10.1 \
    #     pandas==1.5.3 \
    #     cloudpickle==2.2.1 \
    #     scikit-image==0.19.3 \
    #     scikit-learn==1.1.2 \
    #     matplotlib==3.7.1 \
    #     ipython==8.5.0 \
    #     ipykernel==6.16.0 \
    #     ipywidgets==8.0.2 \
    #     cython==0.29.36 \
    #     tqdm==4.66.1 \
    #     gdown==4.5.1 \
    #     xgboost==1.7.6 \
    #     pillow==9.4.0 \
    #     seaborn==0.12.2 \
    #     sqlalchemy==1.4.41 \
    #     spacy==3.6.1 \
    #     nltk==3.8.1 \
    #     boto3==1.24.90 \
    #     tabulate==0.9.0 \
    #     future==0.18.3 \
    #     gradient==2.0.6 \
    #     jsonify==0.5 \
    #     opencv-python==4.6.0.66 \
    #     sentence-transformers==2.2.2 \
    #     wandb==0.13.4 \
    #     awscli==1.25.91 \
    #     jupyterlab-snippets==0.4.1 \
    #     tornado==6.1

    # Fix installation due to the recent release of Cython 3.0.0
    # RUN echo "Cython<3" > cython_constraint.txt  && \
    #     PIP_CONSTRAINT=cython_constraint.txt $PIP_INSTALL "gradient==2.0.6" "wandb==0.15.9" "awscli==1.25.91"

# ==================================================================
# Installing transformers & scikit image (fixed)
# ------------------------------------------------------------------
    # RUN pip install -U git+https://github.com/huggingface/transformers

    # RUN pip install --pre scikit-learn
# ==================================================================
# Installing JRE and JDK
# ------------------------------------------------------------------

    RUN $APT_INSTALL \
        default-jre \
        default-jdk


# ==================================================================
# CMake
# ------------------------------------------------------------------

    RUN $GIT_CLONE https://github.com/Kitware/CMake ~/cmake && \
        cd ~/cmake && \
        ./bootstrap && \
        make -j"$(nproc)" install


# ==================================================================
# Node.js and Jupyter Notebook Extensions
# ------------------------------------------------------------------

    RUN curl -sL https://deb.nodesource.com/setup_16.x | bash  && \
        $APT_INSTALL nodejs  && \
        $PIP_INSTALL jupyter_contrib_nbextensions jupyterlab-git && \
        jupyter contrib nbextension install --user


# ==================================================================
# Startup
# ------------------------------------------------------------------

    EXPOSE 8888 6006

    CMD jupyter lab --allow-root --ip=0.0.0.0 --no-browser --ServerApp.trust_xheaders=True --ServerApp.disable_check_xsrf=False --ServerApp.allow_remote_access=True --ServerApp.allow_origin='*' --ServerApp.allow_credentials=True
