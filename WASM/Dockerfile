
FROM ubuntu:20.04
ARG DEBIAN_FRONTEND=noninteractive
ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH
RUN apt-get update \
    && apt-get install -y tzdata \
    && apt-get -y upgrade && apt-get install -y build-essential curl wget git vim libboost-all-dev llvm-dev liblld-10-dev \
    && apt-get install -y libjpeg-dev libpng-dev
RUN wget https://storage.googleapis.com/tensorflow/libtensorflow/libtensorflow-cpu-linux-x86_64-2.3.0.tar.gz \
    && tar -C /usr/local -xzf libtensorflow-cpu-linux-x86_64-2.3.0.tar.gz
RUN wget https://github.com/second-state/ssvm-tensorflow-deps/releases/download/0.1.0/ssvm-tensorflow-deps-lite-0.1.0-linux-x64.tar.gz \
    && tar -zxvf ssvm-tensorflow-deps-lite-0.1.0-linux-x64.tar.gz \
    && cp ./libtensorflowlite_c.so /usr/local/lib \
    && ldconfig
RUN curl -sL https://deb.nodesource.com/setup_14.x |  bash \
    && apt-get install -y nodejs \
    && npm install -y -g ssvmup --unsafe-perm \
    && npm install -y ssvm \
    && npm install -y ssvm-extensions \
    && npm install express express-fileupload
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y