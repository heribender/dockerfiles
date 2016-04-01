# Image for Java8u77b03 server-jre on ubuntu14.04.1
####################################################

####################################################
# java version "1.8.0_25"
# Java(TM) SE Runtime Environment (build 1.8.0_25-b17)
# Java HotSpot(TM) 64-Bit Server VM (build 25.25-b02, mixed mode)
####################################################

FROM ubuntu:14.04.1
MAINTAINER Heri Bender <heribender@web.de>

# Install cURL and unzip
RUN apt-get update \
    && apt-get -y --force-yes install curl unzip

####################################################
# Install java
####################################################
	
# JAVA_HOME is a link (/usr/local/jdk ) which points to the install dir of java
# (here : /usr/local/jdk1.8.0_77)
ENV JAVA_HOME_PRE /usr/local
ENV JAVA_HOME ${JAVA_HOME_PRE}/jdk

# Java Version
ENV JAVA_VERSION_MAJOR 8
ENV JAVA_VERSION_MINOR 77
ENV JAVA_VERSION_BUILD 03
ENV JAVA_PACKAGE       server-jre
# checksum is the md5 hash:
ENV JAVA_CHECKSUM      32f19f3a0fb872f363689c85321e0021

# http://download.oracle.com/otn-pub/java/jdk/8u77
# Download, verify and extract Java
RUN curl -kLOH "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" \
http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-b${JAVA_VERSION_BUILD}/${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz \
  && echo "${JAVA_CHECKSUM}  ${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz" > ${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz.md5.txt  \
  && md5sum -c ${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz.md5.txt \
  && gunzip ${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz \
  && tar -xf ${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar -C ${JAVA_HOME_PRE} \
  && rm ${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar* \
  && ln -s ${JAVA_HOME_PRE}/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR} ${JAVA_HOME} 
  
ENV PATH ${PATH}:${JAVA_HOME}/bin

RUN echo ${JAVA_HOME} \
    && echo ${PATH} \
    && ls -la /usr/local/ \
    && ${JAVA_HOME}/bin/java -version
	
####################################################
# java version "1.8.0_77"
# Java(TM) SE Runtime Environment (build 1.8.0_77-b03)
# Java HotSpot(TM) 64-Bit Server VM (build 25.77-b03, mixed mode)
####################################################
	
	
# 8000 for java remote debugging
EXPOSE 8000

