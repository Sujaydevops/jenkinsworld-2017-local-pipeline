FROM jenkinsci/ssh-slave

USER root

# Install Docker and allow Jenkins to use it
RUN apt-get update && apt-get install -y apt-transport-https && \
    apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D && \
    echo "deb https://apt.dockerproject.org/repo debian-jessie main" > /etc/apt/sources.list.d/docker.list && \
    apt-get update && \
    apt-cache policy docker-engine && \
    apt-get install -y docker-engine=1.12.1-0~jessie && \
    apt-get install sudo && \
    usermod -g staff jenkins

# SBT Setup
RUN echo "deb https://dl.bintray.com/sbt/debian /" | tee -a /etc/apt/sources.list.d/sbt.list && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 642AC823 && \
    apt-get update && \
    apt-get -y install --no-install-recommends sbt && \
    rm -rf /etc/apt/sources.list.d/* && \
    rm -rf /var/lib/apt/lists/*

# Trigger SBT warm-up

COPY hello/project /tmp/hello/project
COPY hello/build.sbt /tmp/hello/build.sbt
COPY hello/src/main/scala/Hello.scala /tmp/hello/src/main/scala/Hello.scala

RUN cd /tmp/hello && sbt clean compile package test

