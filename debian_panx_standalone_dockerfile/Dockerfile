#################################################################
# Dockerfile
#
# Version:          1.0
# Software:         panX
# Software Version: 1.0
# Description:      Microbial pan-genome analysis and exploration toolkit
# Code:             https://github.com/neherlab/pan-genome-analysis
# Base Image:       debian:jessie
# Build Cmd:        sudo docker build -t debian_panx:panx_docker_v1.0 .
# Pull Cmd:         docker pull panx:panx_docker_v1.0
# Run Cmd:          sudo docker run -it --rm -v /home:/home -p 8000:8000 --name=panx debian_panx:panx_docker_v1.0
# File Author / Maintainer: cheng gong <512543469@qq.com>
#################################################################

# Set the base image to debian
FROM debian:latest

MAINTAINER cheng gong <512543469@qq.com>

ADD ./panx-docker-add-new-pages-repo.sh / \
    ./panx-docker-link-to-server.py  /

# Update the repository sources list
RUN apt-get clean all && \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y \
    build-essential \
    gcc-multilib \
    apt-utils \
    expat \
    libexpat-dev \
    wget \
    git \
    dpkg \
    unzip \ 
    python2.7 \
    python-pip \
    python-biopython \
    python-numpy \
    python-scipy \
    ncbi-blast+ \
    mcl \
    mafft \
    fasttree \
    raxml \
    libc6 \
    libstdc++6 \
    libgcc1 \
    libtbb-dev \
    libcmph-dev \
    mpic++ && \
    apt-get clean && \
    apt-get purge && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
    
RUN pip install ete2 --timeout 6000

RUN git clone https://github.com/neherlab/treetime.git && \
    cd treetime && \
    python setup.py install

RUN wget https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/orthagogue/orthAgogue-1.0.2.deb && \
    dpkg -i orthAgogue-1.0.2.deb && \
    apt-get clean
    
# RUN git clone https://github.com/neherlab/pan-genome-analysis.git
RUN wget -t 0 https://github.com/neherlab/pan-genome-analysis/archive/handle_premature_stop_codon.zip && \
    unzip handle_premature_stop_codon.zip && \
    mv pan-genome-analysis-handle_premature_stop_codon pan-genome-analysis && \
    chmod -R 700 pan-genome-analysis/tools/diamond

RUN wget -qO- https://deb.nodesource.com/setup_4.x | bash - && \
    apt-get install nodejs && \
    apt-get clean
    
RUN git clone https://github.com/neherlab/pan-genome-visualization.git && \
    cd pan-genome-visualization && \
    npm install

RUN ln -s /usr/bin/raxmlHPC /usr/bin/raxml




ENV PATH /pan-genome-analysis/tools:$PATH

#Expose port 8000 (webserver)
EXPOSE :8000

CMD ["/bin/bash"]


