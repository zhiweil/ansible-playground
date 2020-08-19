# FROM rastasheep/ubuntu-sshd
FROM rastasheep/ubuntu-sshd
LABEL maintainer="zhiwei.liu19@gmail.com"

COPY ./id_rsa.pub /root/.ssh/
COPY ./authorized_keys /root/.ssh/
RUN apt-get update
RUN apt-get install -y python3-apt
EXPOSE 8080
RUN passwd -d root