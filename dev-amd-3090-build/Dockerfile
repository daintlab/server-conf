FROM pytorch/pytorch:1.9.0-cuda11.1-cudnn8-devel as base

RUN apt-get update && apt-get install -y --no-install-recommends \
         build-essential \
         git \
         curl \
	 cmake \
         sudo \
         htop \
         vim \
         wget \
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
RUN addgroup --gid 990 docker
RUN useradd -m -u ${UID} --gid ${UID} -s /bin/bash -p ${UNAME} ${UNAME}
RUN usermod -aG docker ${UNAME}
RUN echo "${UNAME}:${UNAME}" | chpasswd
RUN echo "root:root" | chpasswd
RUN adduser ${UNAME} sudo

# change to USER
USER ${UNAME}
ENV HOME /home/${UNAME}

WORKDIR /daintlab/home/${UNAME}

