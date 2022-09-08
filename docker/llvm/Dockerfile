FROM ubuntu:18.04 as base

RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    python3 \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /tmp/
FROM base as builder
RUN git clone -b llvmorg-11.1.0 --depth 1 https://github.com/llvm/llvm-project.git
RUN mkdir -p llvm-build

WORKDIR /tmp/llvm-build

RUN cmake -G "Unix Makefiles" \
    -DCMAKE_BUILD_TYPE=MinSizeRel \
    -DCMAKE_EXE_LINKER_FLAGS="-fuse-ld=gold" \
    -DCMAKE_INSTALL_PREFIX="/tmp/llvm-install" \
    -DLLVM_ENABLE_PROJECTS="clang" \
    -DLLVM_TARGETS_TO_BUILD="X86" \
    ../llvm-project/llvm

RUN make -j$(nproc) && make install

FROM ubuntu:18.04 as final

COPY --from=builder /tmp/llvm-install /tmp/llvm-install