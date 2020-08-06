FROM ansible/ansible:ubuntu1404
LABEL maintainer="zhiwei.liu19@gmail.com"

RUN pip install ansible
COPY ./id_rsa /root/.ssh/
COPY ./id_rsa.pub /root/.ssh/