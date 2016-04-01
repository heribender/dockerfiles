# Image for Java8u77b03 server-jre on alpine
####################################################

####################################################
# java version "1.8.0_77"
# Java(TM) SE Runtime Environment (build 1.8.0_77-b03)
# Java HotSpot(TM) 64-Bit Server VM (build 25.77-b03, mixed mode)
####################################################
	

FROM heribender/alpine-bash
MAINTAINER Heri Bender <heribender@web.de>

# Install cURL and unzip
RUN apk upgrade --update && \
    apk add --update curl && \
    apk add --update unzip

####################################################
# Download glibc (needed for java)
####################################################
	
RUN curl -L -o /tmp/glibc-2.21-r2.apk "https://circle-artifacts.com/gh/andyshinn/alpine-pkg-glibc/6/artifacts/0/home/ubuntu/alpine-pkg-glibc/packages/x86_64/glibc-2.21-r2.apk" && \
    apk add --allow-untrusted /tmp/glibc-2.21-r2.apk && \
    curl -L -o /tmp/glibc-bin-2.21-r2.apk "https://circle-artifacts.com/gh/andyshinn/alpine-pkg-glibc/6/artifacts/0/home/ubuntu/alpine-pkg-glibc/packages/x86_64/glibc-bin-2.21-r2.apk" && \
    apk add --allow-untrusted /tmp/glibc-bin-2.21-r2.apk && \
    /usr/glibc/usr/bin/ldconfig /lib /usr/glibc/usr/lib
	
####################################################
# Install java
####################################################
	
# JAVA_HOME is a link (/usr/local/jre ) which points to the install dir of java
# (here : /usr/local/jdk1.8.0_77)
ENV JAVA_HOME_PRE /usr/local
ENV JAVA_HOME ${JAVA_HOME_PRE}/jre

# Java Version
# (checksum is the md5 hash)
ENV JAVA_VERSION_MAJOR=8 \
    JAVA_VERSION_MINOR=77 \
    JAVA_VERSION_BUILD=03 \
    JAVA_PACKAGE=server-jre \
    JAVA_CHECKSUM=32f19f3a0fb872f363689c85321e0021 \
    PATH=${PATH}:${JAVA_HOME}/bin \
    LANG=C.UTF-8

# http://download.oracle.com/otn-pub/java/jdk/8u77
# Download, verify and extract Java
RUN curl -kLOH "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" \
http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-b${JAVA_VERSION_BUILD}/${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz \
  && echo "${JAVA_CHECKSUM}  ${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz" > ${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz.md5.txt  \
  && md5sum -c ${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz.md5.txt \
  && gunzip ${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz \
  && tar -xf ${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar -C ${JAVA_HOME_PRE} \
  && rm ${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar* \
  && ln -s ${JAVA_HOME_PRE}/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR}/jre ${JAVA_HOME} 

# remove all JDK stuff /everything except the sub folder jre):  
RUN find ${JAVA_HOME_PRE}/jdk1.${JAVA_VERSION_MAJOR}.0_${JAVA_VERSION_MINOR} -maxdepth 1 -mindepth 1 | grep -v jre | xargs rm -rf  

# remove unneeded stuff in jre:
RUN rm -rf ${JAVA_HOME}/plugin \
           ${JAVA_HOME}/bin/javaws \
           ${JAVA_HOME}/bin/jjs \
           ${JAVA_HOME}/bin/keytool \
           ${JAVA_HOME}/bin/orbd \
           ${JAVA_HOME}/bin/pack200 \
           ${JAVA_HOME}/bin/policytool \
           ${JAVA_HOME}/bin/rmid \
           ${JAVA_HOME}/bin/rmiregistry \
           ${JAVA_HOME}/bin/servertool \
           ${JAVA_HOME}/bin/tnameserv \
           ${JAVA_HOME}/bin/unpack200 \
           ${JAVA_HOME}/lib/javaws.jar \
           ${JAVA_HOME}/lib/deploy* \
           ${JAVA_HOME}/lib/desktop \
           ${JAVA_HOME}/lib/*javafx* \
           ${JAVA_HOME}/lib/*jfx* \
           ${JAVA_HOME}/lib/amd64/libdecora_sse.so \
           ${JAVA_HOME}/lib/amd64/libprism_*.so \
           ${JAVA_HOME}/lib/amd64/libfxplugins.so \
           ${JAVA_HOME}/lib/amd64/libglass.so \
           ${JAVA_HOME}/lib/amd64/libgstreamer-lite.so \
           ${JAVA_HOME}/lib/amd64/libjavafx*.so \
           ${JAVA_HOME}/lib/amd64/libjfx*.so \
           ${JAVA_HOME}/lib/ext/jfxrt.jar \
           ${JAVA_HOME}/lib/ext/nashorn.jar \
           ${JAVA_HOME}/lib/oblique-fonts \
           ${JAVA_HOME}/lib/plugin.jar

RUN echo JAVA_HOME: ${JAVA_HOME} \
    && echo PATH: ${PATH} \
    && ls -la /usr/local/ 	\
    && ${JAVA_HOME}/bin/java -version
	
# 8000 for java remote debugging
EXPOSE 8000

# clean up
RUN apk del curl && \
    apk del unzip && \
	rm -rf /var/cache/apk/*
	
