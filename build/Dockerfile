# https://hub.docker.com/_/openjdk
FROM centos:7.5.1804

# set environment
ENV FESCAR_USER="fescar" \
    JAVA_HOME="/usr/lib/jvm/java-1.8.0-openjdk" \
    JAVA="/usr/lib/jvm/java-1.8.0-openjdk/bin/java" \
    TIME_ZONE="Asia/Shanghai"

ARG FESCAR_VERSION=0.1.1

WORKDIR /$BASE_DIR

RUN set -x \
    && yum update -y \
    && yum install -y java-1.8.0-openjdk java-1.8.0-openjdk-devel wget iputils nc  vim libcurl\
    && wget  https://github.com/alibaba/fescar/releases/download/v${FESCAR_VERSION}/fescar-server-${FESCAR_VERSION}.tar.gz -P /home \
    && mkdir /opt/fescar \
    && tar -xzvf /home/fescar-server-${FESCAR_VERSION}.tar.gz -C /opt/fescar \
    && rm -rf /home/fescar-server-${FESCAR_VERSION}.tar.gz /opt/fescar/bin/* /opt/fescar/conf/*properties \
    && ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo '$TIME_ZONE' > /etc/timezone \
    && yum clean all

# add docker-startup.sh
ADD bin/docker-startup.sh /opt/fescar/bin/docker-startup.sh
#RUN chmod +x /opt/fescar/bin/docker-startup.sh

EXPOSE 8091
#ENTRYPOINT ["sh","/opt/fescar/bin/fescar-server.sh"]
ENTRYPOINT ["sh","/opt/fescar/bin/docker-startup.sh"]