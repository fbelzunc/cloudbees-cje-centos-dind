FROM centos:7
MAINTAINER fbelzunc@gmail.com

# Update package database
RUN yum update -y
#RUN yum install -y epel-release
#RUN yum install ansible* curl http* vim wget ca-certificates lxc iptables jq -y

# Install Docker from Docker Inc. repositories.
RUN curl -sSL https://get.docker.com/ | sh

VOLUME /var/lib/docker

# Install the magic wrapper.
ADD wrapdocker /usr/local/bin/wrapdocker
RUN chmod +x /usr/local/bin/wrapdocker

ENTRYPOINT [ "/usr/local/bin/wrapdocker" ]
