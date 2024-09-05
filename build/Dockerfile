FROM ubuntu:24.04
ENV DEBIAN_FRONTEND noninteractive
ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8

RUN apt-get update \
  && apt-get -y upgrade \
  && apt-get install --no-install-recommends --fix-missing -y \
    build-essential \
    ca-certificates \
    cmake \
    curl \
    git \
    ninja-build \
    python3 \
  && echo $TZ > /etc/timezone \
  && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime \
  && dpkg-reconfigure -f noninteractive tzdata \
  && update-ca-certificates \
  && apt-get clean


WORKDIR /opt
RUN git clone https://github.com/EnzymeAD/rust.git \
  && cd rust/ \
  && ./configure \
    --enable-llvm-link-shared \
    --enable-llvm-plugins \
    --enable-llvm-enzyme \
    --release-channel=nightly \
    --enable-llvm-assertions \
    --enable-clang \
    --enable-lld \
    --enable-option-checking \
    --enable-ninja \
    --disable-docs \
  && ./x.py build --stage 1 library \
  && curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y \
  && . "$HOME/.cargo/env" \
  && ldconfig \
  && rustup toolchain link enzyme build/host/stage1 \
  && rustup toolchain install nightly
