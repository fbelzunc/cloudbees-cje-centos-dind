# Official Docker Image from https://hub.docker.com/_/docker/
# Set the Docker version you want to use
FROM fbelzunc/centos-dind:latest

# Defining default variables and build arguments
ARG user=jenkins
ARG group=jenkins
ARG uid=1000
ARG gid=1000
ARG jenkins_user_home=/home/${user}

ENV JENKINS_USER_HOME=${jenkins_user_home} \
  LANG=C.UTF-8 \
  JAVA_HOME=/usr/lib/jvm/java-1.8-openjdk \
  PATH=${PATH}:/usr/local/bin:/usr/lib/jvm/java-1.8-openjdk/jre/bin:/usr/lib/jvm/java-1.8-openjdk/bin \
  DOCKER_IMAGE_CACHE_DIR=/docker-cache \
  AUTOCONFIGURE_DOCKER_STORAGE=true

# Install required packages for running a Jenkins agent
RUN yum -y update ;\
    yum -y install epel-release python-pip git openssh-server openssh-clients make wget sudo vim java-1.8.0-openjdk; \
    yum clean all  

# Set up default user for jenkins
RUN groupadd  -g ${gid} ${group} \
  && useradd \
    -d "${jenkins_user_home}" \
    -u "${uid}" \
    -G "${group}" \
    -s /bin/bash \
    -g "${group}" \
    "${user}" \
  && echo "${user}:${user}" | chpasswd

# Adding the default user to groups used by Docker engine
# "docker" for avoiding sudo, and "dockremap" if you enable user namespacing
RUN usermod -aG docker ${user}

# Custom start script
COPY entrypoint.bash /usr/local/bin/

RUN set -eux &&\
    chmod +x /usr/local/bin/entrypoint.bash

# Those folders should not be on the Docker "layers"
VOLUME ${jenkins_user_home} /docker-cache /tmp

# Default working directory
WORKDIR ${jenkins_user_home}

# Define the "default" entrypoint command executed on the container as PID 1
ENTRYPOINT ["/usr/local/bin/entrypoint.bash"]