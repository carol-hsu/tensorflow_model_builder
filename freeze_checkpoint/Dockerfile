# Dockerfile for building Tensorflow model from checkpoint files
#   
#   Author: Ke-Jou Hsu <nosus_hsu@gatech.edu>

FROM tensorflow/tensorflow:1.12.0

ARG MODEL_WORKDIR=/model_builder

# Download git
RUN apt-get update && apt-get install -y git

# Download slim library
RUN mkdir -p $MODEL_WORKDIR && cd $WORKDIR && \
    git init && \
    git remote add origin https://github.com/tensorflow/models.git && \
    git fetch origin && \
    git checkout origin/master -- research/slim

WORKDIR $MODEL_WORKDIR
