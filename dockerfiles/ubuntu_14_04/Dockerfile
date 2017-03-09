# Start with Ubuntu base image
FROM ubuntu:14.04

MAINTAINER Gabe Perdue <perdue@fnal.gov>

# Install dependencies
RUN apt-get update && apt-get install -y \
  git \
  subversion \ 
  wget \
  python \
  autoconf \
  libx11-dev \
  libxpm-dev \
  libxft-dev \
  libxext-dev \
  libpng-dev \
  libjpeg-dev \
  libtool \
  gfortran \
  libxml2-dev \
  dpkg-dev \
  gdb

# Use bash instead of sh
# RUN ln -snf /bin/bash /bin/sh
# Clone lamp and install
RUN cd /root && git clone https://github.com/GENIEMC/lamp.git && \
  cd lamp && git checkout -b R-2_12_4.0-br R-2_12_4.0 && \
  ./rub_the_lamp.sh -s --no-roomu

# Cleanup
RUN apt-get clean autoclean
RUN apt-get autoremove -y
RUN rm -rf /var/lib/apt/lists/*
