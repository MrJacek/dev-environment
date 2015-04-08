FROM rancher/docker-dind-base
MAINTAINER Jacek Hojczak

RUN rm -rf /etc/service/sshd /etc/my_init.d/00_regen_ssh_host_keys.sh
RUN apt-get update && apt-get install -y -q unzip openjdk-7-jre-headless git

RUN mkdir -p /etc/service/go-agent

ADD http://download.go.cd/gocd-deb/go-agent-14.4.0-1356.deb /tmp/go-agent.deb

WORKDIR /tmp
RUN dpkg -i /tmp/go-agent.deb
RUN sed -i -e 's/DAEMON=Y/DAEMON=N/' /etc/default/go-agent /etc/default/go-agent
RUN mkdir -p /var/go/.ssh
ADD ./ssh-agent/id_rsa /var/go/.ssh/id_rsa
RUN echo "Host git\n\tStrictHostKeyChecking no\n" >> /var/go/.ssh/config
RUN chown -R go:go /var/go 
ENV DOCKER_DAEMON_ARGS="-G docker"
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ADD ./go-agent-start.sh /etc/service/go-agent/run
CMD ["/etc/service/go-agent/run"]
