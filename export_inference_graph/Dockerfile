# Dockerfile for building Tensorflow model from checkpoint files
#   
#   Author: Ke-Jou Hsu <nosus_hsu@gatech.edu>

FROM tensorflow/tensorflow:1.12.0

ARG MAIN_WORKDIR=/model_builder

# Install git
RUN apt-get update && apt-get install -y git 

# Download slim library
RUN mkdir $MAIN_WORKDIR && cd $MAIN_WORKDIR && \
    git init && \
    git remote add origin https://github.com/tensorflow/models.git && \
    git fetch origin && \
    git checkout origin/master -- research/slim

WORKDIR $MAIN_WORKDIR/research/slim
