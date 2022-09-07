FROM ubuntu:22.04 as base
WORKDIR /tmp/
RUN apt update && apt install -y \
    autoconf \
    build-essential \
    cmake \
    git \
    libtool \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

FROM base as builder
ARG GRPC_INSTALL_DIR=/tmp/grpc_install
RUN mkdir -p $GRPC_INSTALL_DIR
RUN git clone --recurse-submodules -b v1.46.3 --depth 1 --shallow-submodules https://github.com/grpc/grpc grpc_source
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