FROM ubuntu:18.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update &&\
	DEBIAN_FRONTEND=noninteractive apt-get install -y apt-utils npm nodejs \
    tmux wget iperf3 curl iputils-ping expect git git-extras \
    g++ gcc make python-minimal openssh-server

RUN git clone git://github.com/handshake-org/hsd.git
WORKDIR /hsd
RUN npm install --production

ENV PATH="${PATH}:/hsd/code/bin:/hsd/code/node_modules/.bin"

ENTRYPOINT ["/bin/bash"]
