FROM ubuntu:18.04 as base
WORKDIR /tmp/
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    python \
    python3 \
    && rm -rf /var/lib/apt/lists/*

FROM base as builder
ARG Z3_INSTALL_DIR=/tmp/z3_install
RUN mkdir -p $Z3_INSTALL_DIR
RUN git clone -b z3-4.4.1 --depth 1 https://github.com/Z3Prover/z3
WORKDIR /tmp/z3
RUN CXXFLAGS="-std=c++03" python scripts/mk_make.py --prefix=$Z3_INSTALL_DIR
WORKDIR /tmp/z3/build

RUN make -j$(nproc)
RUN make install

FROM ubuntu:18.04 as final
ARG Z3_INSTALL_DIR=/tmp/z3_install
COPY --from=builder $Z3_INSTALL_DIR $Z3_INSTALL_DIR