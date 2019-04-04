# https://hub.docker.com/_/openjdk
FROM centos:7.5.1804

# set label
LABEL maintainer="fescar-group <niao.shuai123@163.com>"

# set environment
ENV FESCAR_USER="fescar" \
    JAVA_HOME="/usr/lib/jvm/java-1.8.0-openjdk" \
    JAVA="/usr/lib/jvm/java-1.8.0-openjdk/bin/java" \
    TIME_ZONE="Asia/Shanghai" 

ARG FESCAR_VERSION=0.4.1

WORKDIR /$BASE_DIR

RUN set -x \
    && yum update -y \
    && yum install -y java-1.8.0-openjdk java-1.8.0-openjdk-devel wget iputils nc vim libcurl \
    && wget  https://github.com/alibaba/fescar/releases/download/v${FESCAR_VERSION}/fescar-server-${FESCAR_VERSION}.tar.gz -P /home \
    && mkdir /opt/fescar \
    && tar -xzvf /home/fescar-server-${FESCAR_VERSION}.tar.gz -C /opt/fescar \
    && rm -rf /home/fescar-server-${FESCAR_VERSION}.tar.gz \
    && ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo '$TIME_ZONE' > /etc/timezone \
    && yum clean all

# 设置额外参数
ENV EXTRA_JVM_ARGUMENTS="-XX:MaxDirectMemorySize=1024M"

ENTRYPOINT ["sh","/opt/fescar/bin/fescar-server.sh"]
EXPOSE 8091