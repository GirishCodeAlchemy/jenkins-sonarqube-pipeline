FROM jenkins/jenkins:lts

USER root

RUN apt-get update && \
    apt-get -y install \
    apt-transport-https \
    ca-certificates \
    curl \
    wget \
    gnupg2 \
    software-properties-common
RUN curl -4fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | apt-key add -
RUN add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
    $(lsb_release -cs) \
    stable"

RUN apt-get update && \
    apt-get -y install \
    docker.io 
# docker-ce-cli containerd.io
# sudo usermod -aG docker ${USER}
RUN usermod -aG docker jenkins

USER jenkins