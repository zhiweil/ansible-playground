# FROM rastasheep/ubuntu-sshd
FROM rastasheep/ubuntu-sshd
LABEL maintainer="zhiwei.liu19@gmail.com"

COPY ./id_rsa.pub /root/.ssh/
COPY ./authorized_keys /root/.ssh/
RUN passwd -d root