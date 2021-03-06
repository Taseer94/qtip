##########################################
#####Docker container for QTIP############
##########################################

FROM ubuntu:16.04
MAINTAINER Yujun Zhang <zhang.yujunz@zte.com.cn>
LABEL version="0.1" description="OPNFV QTIP Docker container"

ARG BRANCH=master

ENV REPOS_DIR /home/opnfv/repos
ENV QTIP_DIR /home/opnfv/repos/qtip
ENV PYTHONPATH /home/opnfv/repos/qtip
ENV HOME /home/opnfv
WORKDIR /home/opnfv

# Packaged Dependencies
RUN apt-get update && apt-get install -y \
    software-properties-common \
    wget \
    git \
    gcc \
    ssh \
    expect \
    build-essential\
    libssl-dev\
    libffi-dev\
    python-matplotlib \
    python-dev \
    python-pip \
    libjpeg62 \
    libjpeg62-dev \
    zlib1g-dev \
    python-tk \
    curl \
    supervisor \
    python-setuptools \
    iputils-ping\
    rsync \
    --no-install-recommends \
&& rm -rf /var/lib/apt/lists/*

RUN pip install -U pip
RUN pip install 'setuptools>=17.1'

RUN apt-add-repository ppa:ansible/ansible -y
RUN apt-key update -y
RUN apt-get update && apt-get install ansible -y

RUN mkdir -p ${REPOS_DIR}
RUN mkdir -p /root/.ssh
RUN mkdir -p $HOME/qtip/logs
RUN mkdir -p $HOME/qtip/results

RUN chmod 700 /root/.ssh

#Config ansible
COPY ansible.cfg.default /root/.ansible.cfg

#Cloning Repos
RUN git config --global http.sslVerify false
RUN git clone -b $BRANCH https://gerrit.opnfv.org/gerrit/qtip $REPOS_DIR/qtip
RUN git clone https://gerrit.opnfv.org/gerrit/releng $REPOS_DIR/releng

RUN pip install -U -r $REPOS_DIR/qtip/requirements.txt

#Config supervisor
RUN mkdir -p /var/log/supervisor
RUN locale-gen en_US en_US.UTF-8
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

CMD ["/usr/bin/supervisord"]
