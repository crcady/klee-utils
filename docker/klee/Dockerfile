FROM crcady/llvm:11.1.0-18.04-MinSizeRel as llvm_base
FROM crcady/z3:4.4.1-18.04-MinSizeRel as z3_base
FROM ubuntu:18.04 as base
COPY --from=llvm_base /tmp/ /tmp/
COPY --from=z3_base /tmp/ /tmp/

RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    curl \
    file \
    g++-multilib \
    gcc-multilib \
    git \
    libcap-dev \
    libgoogle-perftools-dev \
    libncurses5-dev \
    libsqlite3-dev \
    libtcmalloc-minimal4 \
    python3 \
    python3-pip \
    unzip \
    graphviz \
    doxygen \
    zlib1g-dev \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /tmp/
FROM base as builder
RUN git clone -b v2.3 --depth 1 https://github.com/klee/klee.git
RUN mkdir -p klee-build

WORKDIR /tmp/klee-build

# FROM builder as ready-to-build

RUN cmake -G "Unix Makefiles" \
    -DCMAKE_BUILD_TYPE=MinSizeRel \
    -DCMAKE_EXE_LINKER_FLAGS="-fuse-ld=gold" \
    -DCMAKE_INSTALL_PREFIX="/tmp/klee-install" \
    -DENABLE_SOLVER_STP=OFF \
    -DENABLE_POSIX_RUNTIME=OFF \
    -DENABLE_SOLVER_Z3=ON \
    -DLLVM_CONFIG_BINARY=/tmp/llvm-install/bin/llvm-config \
    -DZ3_INCLUDE_DIRS=/tmp/z3_install/include \
    -DZ3_LIBRARIES=/tmp/z3_install/lib/libz3.so \
    -DENABLE_UNIT_TESTS=OFF \
    -DENABLE_SYSTEM_TESTS=OFF \
    ../klee

RUN make -j$(nproc) && make install

#FROM builder as final
#COPY --from=ready-to-build /tmp/klee-install /tmp/klee-install