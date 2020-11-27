FROM nvidia/cuda:11.1-cudnn8-runtime-ubuntu18.04 as base

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
         wget \
         ncurses-dev \
         ca-certificates \
         libjpeg-dev \
         libpng-dev \
         zsh \
         locales && \
     rm -rf /var/lib/apt/lists/*

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

# change to USER
USER ${UNAME}
ENV HOME /home/${UNAME}

# install miniconda for USER
ENV CONDA_DIR $HOME/miniconda3
RUN curl -o ~/miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
     chmod +x ~/miniconda.sh && \
     ~/miniconda.sh -b -p $CONDA_DIR && \
     rm ~/miniconda.sh && \
     $CONDA_DIR/bin/conda install -y python=$PYTHON_VERSION && \
     $CONDA_DIR/bin/conda install -y numpy && \
     $CONDA_DIR/bin/conda install -y pyyaml scipy ipython ninja cython typing && \
     $CONDA_DIR/bin/conda install -y pytorch torchvision cudatoolkit=11.0 -c pytorch-nightly && \
     $CONDA_DIR/bin/conda install -y -c pytorch magma-cuda111 && \
     $CONDA_DIR/bin/conda install -y -c conda-forge pytorch-lightning matplotlib ipdb && \
     $CONDA_DIR/bin/conda install -y -c anaconda pylint scikit-learn scikit-image && \
     $CONDA_DIR/bin/conda install -y mkl=2020.0 && \
     $CONDA_DIR/bin/conda clean -ya
ENV PATH $CONDA_DIR/bin:$PATH
ENV MKL_DEBUG_CPU_TYPE 5

WORKDIR /daintlab/home/${UNAME}

