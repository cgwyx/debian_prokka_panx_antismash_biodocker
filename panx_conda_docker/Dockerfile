#################################################################
# Dockerfile
#
# Version:          1.0
# Software:         panX
# Software Version: 1.0
# Description:      Microbial pan-genome analysis and exploration toolkit
# Code:                  https://github.com/neherlab/pan-genome-analysis
# Base Image:       debian:stretch
# Build Cmd:        sudo docker build -t debian_panx:panx_docker_v1.0 .
# Pull Cmd:           docker pull panx:panx_docker_v1.0
# Run Cmd:          sudo docker run -it --rm -v /home:/home -p 8000:8000 --name=panx debian_panx:panx_docker_v1.0
# File Author / Maintainer: cheng gong <512543469@qq.com>
#################################################################

# Set the base image to debian
FROM continuumio/miniconda

MAINTAINER cheng gong <512543469@qq.com>

RUN conda update --all -y &&\
         conda config --add channels r &&\
         conda config --add channels bioconda&&\
         #conda config --set show_channel_urls yes
         git clone https://github.com/neherlab/pan-genome-analysis.git &&\
         cd pan-genome-analysis &&\
         git submodule update --init &&\
         conda env create -f panX-environment.yml &&\
         source activate panX

#Expose port 8000 (webserver)
EXPOSE :8000

WORKDIR /pan-genome-analysis

CMD ["/bin/bash"]


