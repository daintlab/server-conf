FROM nvidia/cuda:10.2-cudnn8-devel-ubuntu18.04 as base

ARG PYTHON_VERSION=3.8
ARG WITH_TORCHVISION=1
RUN apt-get update && apt-get install -y --no-install-recommends \
         build-essential \
         cmake \
         git \
         curl \
         sudo \
         htop \
         vim \
         ca-certificates \
         libjpeg-dev \
         libpng-dev && \
     rm -rf /var/lib/apt/lists/*

RUN curl -o ~/miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
     chmod +x ~/miniconda.sh && \
     ~/miniconda.sh -b -p /opt/conda && \
     rm ~/miniconda.sh && \
     /opt/conda/bin/conda install -y python=$PYTHON_VERSION numpy pyyaml scipy ipython mkl mkl-include ninja cython typing && \
     /opt/conda/bin/conda install -y -c pytorch magma-cuda102 torchvision cudatoolkit=10.2 && \
     /opt/conda/bin/conda install -y -c conda-forge pytorch-lightning && \
     /opt/conda/bin/conda clean -ya
ENV PATH /opt/conda/bin:$PATH

# env setting for server
ARG UNAME
ARG UID
ARG GID

RUN addgroup --gid ${UID} ${UNAME}
RUN addgroup --gid 999 docker
RUN useradd -m -u ${UID} --gid ${UID} -s /bin/bash -p ${UNAME} ${UNAME}
RUN usermod -aG docker ${UNAME}
RUN echo "${UNAME}:${UNAME}" | chpasswd
RUN adduser ${UNAME} sudo

USER ${UNAME}
WORKDIR /daintlab/home/${UNAME}

