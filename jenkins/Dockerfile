FROM jenkins/jenkins:lts
MAINTAINER susimsek

ARG kubernetes_version=v1.15.5
ARG docker-compose_version=1.25.4

USER root

# En son Docker CE binaryleri yükleyin
RUN apt-get update && apt-get -y install \
      apt-transport-https \
      ca-certificates \
      curl \
      gnupg2 \
      software-properties-common \
      rsync \
      openjdk-8-jdk \
      sudo

RUN curl -sSL https://get.docker.com/ | sh

RUN usermod -a -G docker jenkins

# En son docker-compose binary dosyasını yükleyin
RUN curl -L https://github.com/docker/compose/releases/download/$docker-compose_version/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose


# Kubectl'i yükle
RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/$kubernetes_version/bin/linux/amd64/kubectl
RUN chmod +x ./kubectl
RUN mv ./kubectl /usr/local/bin/kubectl

RUN mkdir /var/jenkins_home/.kube
VOLUME ["/var/jenkins_home/.kube"]

# Jenkins için eklentiler yükleyin (bunları kaldırabilir / ekleyebilirsiniz)
RUN /usr/local/bin/install-plugins.sh \
    workflow-aggregator:2.5 \
    workflow-scm-step:2.6 \
    git-client:2.7.1 \
    pipeline-multibranch-defaults:1.1 \
    docker-workflow:1.15.1 \
    bitbucket:1.1.8 \
    docker-slaves:1.0.7 \
    credentials-binding:1.15 \
    cloudbees-bitbucket-branch-source:2.2.10 \
    antisamy-markup-formatter:1.5 \
    gatling:1.2.2 \
    matrix-auth:2.2 \
    mission-control-view:0.9.13 \
    pipeline-utility-steps:2.0.1 \
    resource-disposer:0.8 \
    slack:2.3 \
    ssh-credentials:1.13 \
    swarm:3.10 \
    windows-slaves:1.3.1 \
    ws-cleanup:0.34 \
    blueocean:1.4.2

COPY entrypoint.sh /
RUN chmod 755 /entrypoint.sh
ENTRYPOINT ["/bin/bash", "-c", "/entrypoint.sh"]
