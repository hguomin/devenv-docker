#Minimal development environment

FROM ubuntu:20.04

#Install sudo & ssh.
RUN apt-get update && \
    apt-get install sudo vim openssh-server -y

#ssh must have this directory
RUN mkdir /var/run/sshd

#Set root's password to 'root' because the root passord is not set by default.
RUN echo 'root:root' | chpasswd

#Add a default user named 'ubuntu' and set its password to same as name
RUN useradd -rm -d /home/devplus -s /bin/bash -g root -G sudo -u 1000 devplus
RUN echo 'devplus:devplus' | chpasswd

#Make some ssh config
RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/^#?PasswordAuthentication\s+.*/PasswordAuthentication yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config

#Expose the ssh port
EXPOSE 22

#Get all runtime environment for a ssh client login
RUN echo 'export $(cat /var/rt-env | sed -e "/\\bHOME/d" -e "/PWD/d" -e "/USER/d" -e "/LOGNAME/d" -e "/SSH/d" -e "/LS_COLORS/d" -e "/MAIL/d" -e "/LESS/d" -e "/TERM/d" | xargs)' >> /etc/profile

#Dump runtime(main processor of this container) environments and save it to file rt-env, when a ssh client login, it will be exported by /etc/profile
CMD [ "/bin/bash", "-c", "env >> /var/rt-env && /usr/sbin/sshd -D" ]
#CMD ["/usr/sbin/sshd", "-D"]