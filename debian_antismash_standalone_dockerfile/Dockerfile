#################################################################
# Dockerfile
#
# Version:          1.0
# Software:         antiSMASH
# Software Version: 3.0.5.1
# Description:      genome mining of biosynthetic gene clusters
# Code:             https://bitbucket.org/antismash/antismash/downloads/
# Base Image:       debian:jessie
# Build Cmd:        sudo docker build -t debian_antismash:antismash3.0.5_docker1.0 .
# Pull Cmd:         docker pull debian_antismash:antismash3.0.5_docker1.0
# Run Cmd:          sudo docker run -it --rm -v /home:/home --name=antismash3.0.5 debian_antismash:antismash3.0.5_docker1.0
# File Author / Maintainer: cheng gong <512543469@qq.com>
#################################################################

# Standalone antiSMASH build with databases
# VERSION 3.0.5.1

FROM debian:latest
MAINTAINER cheng gong <1461744165@qq.com>

# set up antiSMASH deb repo
ADD http://dl.secondarymetabolites.org/antismash.list /etc/apt/sources.list.d/antismash.list
ADD http://dl.secondarymetabolites.org/antismash.asc /tmp/
RUN apt-key add /tmp/antismash.asc

ENV ANTISMASH_URL="https://bitbucket.org/antismash/antismash/downloads"
ENV ANTISMASH_VERSION="3.0.5.1"

# grab all the dependencies
RUN apt-get update && \ 
    apt-get install -y && \ 
    ncbi-blast+ && \ 
		glimmerhmm && \ 
		tigr-glimmer && \ 
		diamond && \ 
		hmmer2 && \ 
		hmmer && \ 
		hmmer2-compat && \ 
		muscle && \ 
		prodigal && \ 
		fasttree && \ 
		default-jre-headless && \ 
		python-straight.plugin && \ 
		python-pysvg && \ 
		python-helperlibs && \ 
		python-biopython && \ 
		python-pyquery && \ 
		python-backports.lzma && \ 
		python-matplotlib && \
		apt-get clean && \
    apt-get purge && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*   



# Grab antiSMASH
ADD ${ANTISMASH_URL}/antismash-${ANTISMASH_VERSION}.tar.gz /
RUN tar xf /home/antismash-${ANTISMASH_VERSION}.tar.gz

# Grab the databases
WORKDIR antismash-${ANTISMASH_VERSION}
RUN python download_databases.py

ADD instance.cfg antismash/config/instance.cfg

WORKDIR /usr/local/bin
RUN ln -s /antismash-${ANTISMASH_VERSION}/run_antismash.py

WORKDIR /antismash-${ANTISMASH_VERSION}/antismash/

CMD ["/bin/bash"]
