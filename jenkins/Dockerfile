
FROM jenkins/jenkins:lts-jdk17

USER root
RUN apt-get update && apt-get install -y git && rm -rf /var/lib/apt/lists/*

# Instalar plugins usando la CLI oficial
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN jenkins-plugin-cli --plugin-file /usr/share/jenkins/ref/plugins.txt

USER jenkins
