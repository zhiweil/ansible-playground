FROM ansible/ansible:ubuntu1404
LABEL maintainer="zhiwei.liu19@gmail.com"

RUN pip install ansible
RUN apt-get update
RUN apt-get install -y curl
COPY ./id_rsa /root/.ssh/
COPY ./id_rsa.pub /root/.ssh/
COPY ./playground/ /root/playground