# FROM maven:3.6.3-openjdk-8
FROM ubuntu:18.04

RUN apt update && apt install -y \
    git \
    docker.io \
    openssh-client \
    default-jdk \
    maven

# Clear cache
RUN apt-get clean

# Confugure ssh client
COPY id_rsa /root/.ssh/id_rsa
RUN chmod 600 /root/.ssh/id_rsa

WORKDIR git