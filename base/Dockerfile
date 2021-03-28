FROM ubuntu:latest

ENV TZ=Europe/Paris
ENV DEBIAN_FRONTEND=noninteractive
ENV LC_ALL=C

ARG GROUP_ID
ARG USER_ID
ARG GPG_KEY
ARG SSH_KEY

RUN groupadd --gid ${GROUP_ID} appgroup
RUN useradd -m --gid ${GROUP_ID} --uid ${GROUP_ID} appuser

RUN apt-get update && apt-get install -y wget haskell-stack netbase

RUN stack upgrade

RUN mkdir /isabelle
RUN wget -qO- https://isabelle.in.tum.de/dist/Isabelle2021_linux.tar.gz | tar xz --strip-components=1 -C /isabelle

RUN mkdir /afp
RUN wget -qO- https://www.isa-afp.org/release/afp-current.tar.gz | tar xz --strip-components=1 -C /afp

RUN wget -qO /vscode.deb "https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64"
RUN apt-get update && apt-get install -y xterm gnupg git libnss3 libsecret-1-0 libgtk-3-0 libxss1 libgbm1 libasound2
RUN dpkg -i /vscode.deb

USER appuser

ENV PATH=/isabelle/bin:$PATH

RUN mkdir -p ~/.isabelle/Isabelle2021
RUN echo "/afp/thys" >> ~/.isabelle/Isabelle2021/ROOTS

RUN mkdir -p ~/.isabelle/Isabelle2021/jedit
RUN echo "view.fontsize=14" > ~/.isabelle/Isabelle2021/jedit/properties

RUN git config --global user.email "me@eminkarayel.de"
RUN git config --global user.name "Emin Karayel"

RUN echo "$GPG_KEY\n" | gpg --import
RUN eval "$(ssh-agent)" && echo "$SSH_KEY" | tr -d '\r' | ssh-add -
RUN stack ghc -- -e "1+1"

RUN mkdir /home/appuser/workdir
WORKDIR /home/appuser/workdir