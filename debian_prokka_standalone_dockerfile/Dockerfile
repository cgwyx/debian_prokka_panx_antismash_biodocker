#################################################################
# Dockerfile
#
# Version:          1.0
# Software:         prokka
# Software Version: 1.12
# Description:      Microbial pan-genome analysis and exploration toolkit
# Code:             https://github.com/neherlab/pan-genome-analysis
# Base Image:       debian:jessie
# Build Cmd:        sudo docker build -t debian_prokka:prokka_latest_docker1.0 .
# Pull Cmd:         docker pull debian_prokka:prokka_latest_docker1.0
# Run Cmd:          sudo docker run -it --rm -v /home:/home --name=prokka debian_prokka:prokka_latest_docker1.0
# File Author / Maintainer: cheng gong <512543469@qq.com>
#################################################################

# Set the base image to debian
FROM debian:latest

MAINTAINER cheng gong <512543469@qq.com>

# Update the repository sources list
RUN apt-get clean all && \
    apt-get update && \
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
    ###prokka system packages###
    perl \
    cpanminus \ 
    perl-doc \
    libdatetime-perl \ 
    libxml-simple-perl \ 
    libdigest-md5-perl \ 
    bioperl && \
    apt-get clean && \
    apt-get purge && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*   

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
RUN cd /tmp && wget --header "Cookie: oraclelicense=accept-securebackup-cookie;" \
        "http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION}u${JAVA_UPDATE}-b${JAVA_BUILD}/jre-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz" && \
     tar xzf "jre-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz" && \
     mkdir -p /usr/lib/jvm && mv "/tmp/jre1.${JAVA_VERSION}.0_${JAVA_UPDATE}" "/usr/lib/jvm/java-${JAVA_VERSION}-oracle"  && \
     ln -s "java-${JAVA_VERSION}-oracle" $JAVA_HOME && \
     ln -s $JAVA_HOME/bin/java /usr/bin/java && \
     rm -rf $JAVA_HOME/*.txt && \
     rm -rf $JAVA_HOME/*.html
     #apk del wget ca-certificates curl && \
     #rm /tmp/* /var/cache/apk/*


###instation prokka###
RUN git clone https://github.com/tseemann/prokka.git && \
    wget ftp://ftp.ncbi.nih.gov/toolbox/ncbi_tools/converters/by_program/tbl2asn/linux64.tbl2asn.gz && \
    gunzip linux64.tbl2asn.gz && \
    cp linux64.tbl2asn /prokka/binaries/linux/tbl2asn && \
    chmod a+x /prokka/binaries/linux/tbl2asn && \
    /prokka/bin/prokka --setupdb
    
ENV PATH /prokka/bin:$PATH
ENV PATH /prokka/binaries/linux:$PATH
ENV PATH /prokka/binaries/common:$PATH

CMD ["/bin/bash"]


