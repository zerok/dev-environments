FROM ubuntu:20.04 as BASE
RUN apt-get update && apt-get install -y entr curl git tmux neovim

FROM BASE AS WITHGO
ENV GO_VERSION=1.14.5
RUN curl -L -o /tmp/go${GO_VERSION}.linux-amd64.tar.gz https://golang.org/dl/go${GO_VERSION}.linux-amd64.tar.gz && \
    cd /usr/local && tar -xzvf /tmp/go${GO_VERSION}.linux-amd64.tar.gz && rm -rf /tmp/go${GO_VERSION}.linux-amd64.tar.gz && \
    echo -n 'export PATH=$PATH:/usr/local/go/bin' >> /etc/bash.bashrc

FROM WITHGO AS WITHGOTOOLS
ENV PATH=$PATH:/usr/local/go/bin
RUN go get -u github.com/uudashr/gopkgs/cmd/gopkgs && \
    go get -u github.com/go-delve/delve/cmd/dlv && \
    go get -u golang.org/x/tools/gopls

FROM WITHGOTOOLS AS WITHPYTHON
RUN apt install -y python3 python3-dev python3-pip python3-venv && \
    pip3 install poetry

FROM WITHPYTHON
RUN curl -sL https://deb.nodesource.com/setup_12.x -o /tmp/nodesource_setup.sh && bash /tmp/nodesource_setup.sh && \
    (curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -) && \
    (echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list) && \
    apt-get update && apt-get install -y yarn && rm /tmp/nodesource_setup.sh