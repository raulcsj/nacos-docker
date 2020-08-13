# Dockerfile for alibab nacos to reduce offical images size
FROM openjdk:8
MAINTAINER caoshengjie <cao.shengjie777@gmail.com>

# set environment
ENV BASE_DIR="/home/nacos" \
    MODE="cluster" \
    NACOS_USER="nacos" \
    PREFER_HOST_MODE="ip"\
    CLASSPATH=".:${BASE_DIR}/conf:$CLASSPATH" \
    CLUSTER_CONF="${BASE_DIR}/conf/cluster.conf" \
    FUNCTION_MODE="all" \
    # JAVA_HOME="/usr/lib/jvm/java-1.8.0-openjdk" \
    JAVA="${JAVA_HOME}/bin/java" \
    JVM_XMS="2g" \
    JVM_XMX="2g" \
    JVM_XMN="1g" \
    JVM_MS="128m" \
    JVM_MMS="320m" \
    NACOS_DEBUG="n" \
    TOMCAT_ACCESSLOG_ENABLED="false" \
    TIME_ZONE="Asia/Shanghai"

ARG NACOS_VERSION=1.3.1

WORKDIR /$BASE_DIR

RUN echo "http://mirrors.aliyun.com/alpine/v3.8/main" > /etc/apk/repositories \
    && echo "http://mirrors.aliyun.com/alpine/v3.8/community" >> /etc/apk/repositories \
    && apk update upgrade \
    && apk add --no-cache procps wget bash tzdata \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "$TIME_ZONE" > /etc/timezone \
    && wget  https://github.com/alibaba/nacos/releases/download/${NACOS_VERSION}/nacos-server-${NACOS_VERSION}.tar.gz -P /home \
    && tar -xzvf /home/nacos-server-${NACOS_VERSION}.tar.gz -C /home \
    && rm -rf /home/nacos-server-${NACOS_VERSION}.tar.gz /home/nacos/bin/* /home/nacos/conf/*.properties /home/nacos/conf/*.example /home/nacos/conf/nacos-mysql.sql \
    && apk del wget

ADD bin/docker-startup.sh bin/docker-startup.sh
ADD conf/application.properties conf/application.properties
ADD init.d/custom.properties init.d/custom.properties


# set startup log dir
RUN mkdir -p logs \
	&& cd logs \
	&& touch start.out \
	&& ln -sf /dev/stdout start.out \
	&& ln -sf /dev/stderr start.out
RUN chmod +x bin/docker-startup.sh

EXPOSE 8848
ENTRYPOINT ["bin/docker-startup.sh"]
