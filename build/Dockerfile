# https://hub.docker.com/_/openjdk
FROM openjdk:8-jre

# set environment
ENV FESCAR_USER="fescar" \
    TIME_ZONE="Asia/Shanghai"

ARG FESCAR_VERSION=0.1.1

WORKDIR /$BASE_DIR

RUN set -x \
    && wget  https://github.com/alibaba/fescar/releases/download/v${FESCAR_VERSION}/fescar-server-${FESCAR_VERSION}.tar.gz -P /home \
    && mkdir /opt/fescar \
    && tar -xzvf /home/fescar-server-${FESCAR_VERSION}.tar.gz -C /opt/fescar \
    && rm -rf /home/fescar-server-${FESCAR_VERSION}.tar.gz \
    && rm -rf /opt/fescar/bin/* \
    && ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo '$TIME_ZONE' > /etc/timezone

# add docker-startup.sh
ADD bin/docker-startup.sh /opt/fescar/bin/docker-startup.sh
#RUN chmod +x /opt/fescar/bin/docker-startup.sh

EXPOSE 8091
#ENTRYPOINT ["sh","/opt/fescar/bin/fescar-server.sh"]
ENTRYPOINT ["sh","/opt/fescar/bin/docker-startup.sh"]