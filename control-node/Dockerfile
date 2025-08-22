FROM ubuntu 

ARG USER=${USER}

# Add sudo and openssh-server
ENV DEBIAN_FRONTEND=noninteractive
RUN apt update && apt install openssh-server sudo  -y

# Setup running user on the container with sudo rights and
# password-less ssh login
RUN useradd -m ${USER} 
RUN adduser ${USER} sudo
RUN echo "${USER} ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/sudoers
RUN chsh ${USER} -s /bin/bash

# As the user setup the ssh identity using the key in the tmp folder
USER "${USER}"
RUN mkdir ~/.ssh
RUN chmod -R 700 ~/.ssh

COPY --chown=${USER}:sudo ./keys/id_ed25519.pub /home/${USER}/.ssh/id_ed25519.pub
# UNSAFE !!!
COPY --chown=${USER}:sudo ./keys/id_ed25519 /home/${USER}/.ssh/id_ed25519

RUN cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys

RUN chmod 600 ~/.ssh/id_ed25519.pub
RUN chmod 600 ~/.ssh/id_ed25519
RUN chmod 600 ~/.ssh/authorized_keys

# start ssh with port exposed
USER root
RUN service ssh start

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]

##################################################################

RUN apt install python3 python3-pip -y
RUN apt install ansible -y
