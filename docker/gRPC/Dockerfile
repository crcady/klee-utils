FROM ubuntu:18.04 as base
WORKDIR /tmp/
RUN apt update && apt install -y \
    autoconf \
    build-essential \
    git \
    libtool \
    pkg-config \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Need to install CMake separately, because older versions of linux don't have a new enough version to build gRPC
RUN wget -q -O /tmp/cmake-linux.sh https://github.com/Kitware/CMake/releases/download/v3.24.2/cmake-3.24.2-Linux-x86_64.sh \
    && sh /tmp/cmake-linux.sh -- --skip-license --prefix=/usr/local \
    && rm /tmp/cmake-linux.sh

FROM base as builder
ARG GRPC_INSTALL_DIR=/tmp/grpc_install
RUN mkdir -p $GRPC_INSTALL_DIR
RUN git clone --recurse-submodules -b v1.49.1 --depth 1 --shallow-submodules https://github.com/grpc/grpc grpc_source
RUN mkdir -p grpc_build
WORKDIR /tmp/grpc_build
RUN cmake \
    -DCMAKE_INSTALL_PREFIX=$GRPC_INSTALL_DIR \
    -DgRPC_BUILD_TESTS=OFF \
    -DgRPC_INSTALL=ON \
    ../grpc_source

RUN make -j$(nproc)
RUN make install

FROM base as final
ARG GRPC_INSTALL_DIR=/tmp/grpc_install
COPY --from=builder $GRPC_INSTALL_DIR $GRPC_INSTALL_DIR