FROM ubuntu:20.04

#Install sudo & ssh.
RUN apt-get update && \
    apt-get install sudo openssh-server -y

#ssh must have this directory
RUN mkdir /var/run/sshd

#Set root's password to 'root' because the root passord is not set by default.
RUN echo 'root:root' | chpasswd

#Add a default user named 'ubuntu' and set its password to same as name
RUN useradd -rm -d /home/ubuntu -s /bin/bash -g root -G sudo -u 1000 ubuntu
RUN echo 'ubuntu:ubuntu' | chpasswd

#Make some ssh config
RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/^#?PasswordAuthentication\s+.*/PasswordAuthentication yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config

#Expose the ssh port
EXPOSE 22

#Start ssh service
CMD ["/usr/sbin/sshd","-D"]