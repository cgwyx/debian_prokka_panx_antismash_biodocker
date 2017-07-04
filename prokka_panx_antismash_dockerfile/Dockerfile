#################################################################
# Dockerfile
#
# Version:          1.0
# Software:         prokka,panX&antismash
# Software Version: 1.12,1.0,3.0.5.1
# Description:      BGDMdocker: an workflow base on Docker for analysis and visualization pan-genome and biosynthetic gene clusters of Bacterial
# Code:             https://github.com/cgwyx/debian_prokka_panx_antismash_docker
# Base Image:       debian:latest
# Build Cmd:        sudo docker build -t BGDMdocker:latest .
# Pull Cmd:         docker pull BGDMdocker:latest
# Run Cmd:          sudo docker run -it --rm -v /home:/home -p 8000:8000 --name=BGDMdocker BGDMdocker:latest
# File Author / Maintainer: cheng gong <512543469@qq.com>
#################################################################

# Set the base image to debian
FROM debian:latest

MAINTAINER cheng gong <512543469@qq.com>


# Update the repository sources list

RUN apt-get update && \
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
    ###panx### 
    python2.7 \
    python-pip \
    python-biopython \
    python-numpy \
    python-scipy \
    python-pandas \
    ncbi-blast+ \
    mcl \
    mafft \
    fasttree \
    raxml \
    ###panx finsh###
    ###prokka packages###
    perl \
    libdatetime-perl \
    libxml-simple-perl \ 
    libdigest-md5-perl \ 
    bioperl && \
    ###prokka packages finsh###
    apt-get clean && \
    apt-get purge && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

###prokka installation ###    
# Java Version
ENV  JAVA_VERSION=8 \
     JAVA_UPDATE=111 \
     JAVA_BUILD=14  \
     JAVA_HOME=/usr/lib/jvm/current-java

# JDK: http://download.oracle.com/otn-pub/java/jdk/8u60-b27/jdk-8u60-linux-x64.tar.gz
# JRE: http://download.oracle.com/otn-pub/java/jdk/8u60-b27/jre-8u60-linux-x64.tar.gz
# http://download.oracle.com/otn-pub/java/jdk/8u101-b13/jdk-8u101-linux-x64.tar.gz
# http://download.oracle.com/otn-pub/java/jdk/8u111-b14/jdk-8u111-linux-x64.tar.gz

# JRE
RUN  cd /tmp && \
     wget https://files.eveoh.nl/jre-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz && \
     tar xzf "jre-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz" && \
     mkdir -p /usr/lib/jvm && mv "/tmp/jre1.${JAVA_VERSION}.0_${JAVA_UPDATE}" "/usr/lib/jvm/java-${JAVA_VERSION}-oracle"  && \
     ln -s "java-${JAVA_VERSION}-oracle" $JAVA_HOME && \
     ln -s $JAVA_HOME/bin/java /usr/bin/java && \
     rm -rf $JAVA_HOME/*.txt && \
     rm -rf $JAVA_HOME/*.html
     #apk del wget ca-certificates curl && \
     #rm /tmp/* /var/cache/apk/*

#instation prokka#
RUN git clone https://github.com/tseemann/prokka.git && \
    wget ftp://ftp.ncbi.nih.gov/toolbox/ncbi_tools/converters/by_program/tbl2asn/linux64.tbl2asn.gz && \
    gunzip linux64.tbl2asn.gz && \
    cp linux64.tbl2asn /prokka/binaries/linux/tbl2asn && \
    chmod a+x /prokka/binaries/linux/tbl2asn && \
    /prokka/bin/prokka --setupdb
    
ENV PATH /prokka/bin:$PATH
ENV PATH /prokka/binaries/linux:$PATH
ENV PATH /prokka/binaries/common:$PATH
##prokka installation finsh ##
  
###panx installation###
RUN pip install ete2 --timeout 6000
 
RUN git clone https://github.com/neherlab/pan-genome-analysis.git && \
#RUN unzip /pan-genome-analysis.zip && \
    cd pan-genome-analysis && \
    git submodule update --init && \
    cd treetime && \
    python setup.py install

RUN wget -qO- https://deb.nodesource.com/setup_4.x | bash - && \
    apt-get install nodejs && \
    apt-get clean
    
RUN git clone https://github.com/neherlab/pan-genome-visualization.git && \
    cd pan-genome-visualization && \
    npm install

ADD ./panx-docker-add-new-pages-repo.sh / \
    ./panx-docker-link-to-server.py  /

RUN ln -s /usr/bin/raxmlHPC /usr/bin/raxml

ENV PATH /pan-genome-analysis/tools:$PATH

#Expose port 8000 (webserver)
EXPOSE :8000
##panx installation finsh##

###antiSMASH installation###

ENV ANTISMASH_URL="https://bitbucket.org/antismash/antismash/downloads"
ENV ANTISMASH_VERSION="3.0.5.1"

# set up antiSMASH deb repo
ADD http://dl.secondarymetabolites.org/antismash.list /etc/apt/sources.list.d/antismash.list
ADD http://dl.secondarymetabolites.org/antismash.asc /tmp/
RUN apt-key add /tmp/antismash.asc

###antismash packages###
RUN apt-get update && \
    apt-get install -y \
    #ncbi-blast+
    glimmerhmm \ 
    tigr-glimmer \ 
    diamond \
    hmmer2 \ 
    hmmer \ 
    hmmer2-compat \ 
    muscle \ 
    prodigal \ 
    fasttree \ 
    default-jre-headless \ 
    python-straight.plugin \ 
    python-pysvg \ 
    python-helperlibs \ 
    #python-biopython
    python-pyquery \ 
    python-backports.lzma \ 
    python-matplotlib && \
    apt-get clean && \
    apt-get purge && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
    ###antismash packages finsh###

# Grab antiSMASH
ADD ${ANTISMASH_URL}/antismash-${ANTISMASH_VERSION}.tar.gz /
RUN tar xf /antismash-${ANTISMASH_VERSION}.tar.gz

# Grab the databases
WORKDIR /antismash-${ANTISMASH_VERSION}
RUN python download_databases.py

ADD ./instance.cfg antismash/config/instance.cfg

WORKDIR /usr/local/bin
RUN ln -s /antismash-${ANTISMASH_VERSION}/run_antismash.py

##antiSMASH installation finsh##

WORKDIR /pan-genome-analysis

CMD ["/bin/bash"]


