# https://hub.docker.com/_/openjdk
FROM centos:7.5.1804

# set label
LABEL maintainer="niaoshuai <niao.shuai123@163.com>"

# set environment
ENV SEATA_USER="seata" \
    JAVA_HOME="/usr/lib/jvm/java-1.8.0-openjdk" \
    JAVA="/usr/lib/jvm/java-1.8.0-openjdk/bin/java" \
    TIME_ZONE="Asia/Shanghai" 

ARG SEATA_VERSION=0.5.0

WORKDIR /$BASE_DIR

RUN set -x \
    && yum update -y \
    && yum install -y java-1.8.0-openjdk java-1.8.0-openjdk-devel wget iputils nc vim libcurl \
    && wget  https://github.com/seata/seata/releases/download/${SEATA_VERSION}/seata-server-${SEATA_VERSION}.tar.gz -P /home \
    && mkdir /opt/seata \
    && tar -xzvf /home/seata-server-${SEATA_VERSION}.tar.gz -C /opt/seata \
    && rm -rf /home/seata-server-${SEATA_VERSION}.tar.gz \
    && ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo '$TIME_ZONE' > /etc/timezone \
    && yum clean all

# 设置额外参数
ENV EXTRA_JVM_ARGUMENTS="-XX:MaxDirectMemorySize=1024M"

ENTRYPOINT ["sh","/opt/seata/bin/seata-server.sh"]
EXPOSE 8091