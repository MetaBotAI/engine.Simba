#!/usr/bin/bash
FROM ubuntu:20.04

ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update

# pre
RUN mkdir -p /app
WORKDIR /app

# steps:
# - name: Install Dependencies (Linux)
RUN apt-get -y install libgtk2.0-dev \
libpango1.0-dev \
libffi-dev \
libxtst-dev \
xvfb

RUN Xvfb :1 &

# workflow vars
# - os: ubuntu-latest
ENV name 'Linux 64'
ENV FPCUP 'https://github.com/LongDirtyAnimAlf/Reiniero-fpcup/releases/download/1.6.8z/fpclazup-x86_64-linux'
ENV FPC_VER 'https://svn.freepascal.org/svn/fpc/tags/release_3_2_0'
ENV LAZ_VER '--build-mode=SIMBA_DARWIN64'
ENV LAZ_OPT '--build-mode=SIMBA_LINUX64'

# - name: Generate Cache Hash
#RUN echo ${FPCUP}${FPCUP_OPT} $FPC_VER$LAZ_VER$NAME1 > cache

# Fix: fpcup error
RUN apt-get -y install subversion

# - name: Install Lazarus
RUN curl --retry 5 -L -o fpcup $FPCUP
RUN mkdir lazarus || true
RUN chmod +x fpcup
# Only used on Mac: $FPCUP_OPT
RUN ./fpcup --installdir=lazarus --fpcURL=$FPC_VER --lazURL=$LAZ_VER $FPCUP_OPT --noconfirm --verbose

# - name: Build Simba
#RUN ./lazarus/lazarus/lazbuild $LAZ_OPT "Projects/Simba/Simba.lpi"

# my: debug:
RUN apt-get install -y git

# final: from: Coreelec builder
RUN adduser --disabled-password --gecos '' docker \
 && adduser docker sudo \
 && echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
# USER docker

# debug:
RUN echo "" > dbg && chmod +x dbg &&\
echo '''\
git log \n\
git status \n\
./lazarus/lazarus/lazbuild $LAZ_OPT "Projects/Simba/Simba.lpi"\
''' > ~/.bash_history

WORKDIR /app/Simba


# linkSource() at runtime
ADD entrypoint /
RUN chmod +x /entrypoint
# Works!
CMD ["/entrypoint"]


# prime. github.com/ollydev/Simba/blob/simba1400/.github/workflows/build.yml

# inspire. github.com/CoreELEC/coreelec-builder/blob/aarch64/Dockerfile
