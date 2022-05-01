# Create: https://gist.github.com/rlefevre/1523f47e75310e28eee243c9c5651ac9
# Delete: docker system prune -a ; docker images -a

FROM debian:bullseye-slim

# branch
ARG branch=0.19.1
# commit or tag
ARG commit=HEAD

# Install required packages
RUN apt-get update \
    && apt-get install -y --no-install-recommends build-essential \
        automake \
        autotools-dev \
        make \
        g++ \
        ca-certificates \
        software-properties-common \
        apt-transport-https \
        lsb-base \
        lsb-release \
        zlib1g-dev \
        libpcre3-dev \
        libcurl4-openssl-dev \
        libc-dev \
        libxml2-dev \
        libsnmp-dev \
        libssh2-1-dev \
        libevent-dev \
        libopenipmi-dev \
        libpng-dev \
        pkg-config \
        libfontconfig1 \
        git \
        bzip2 \
        zip \
        unzip \
        musl-dev \
        ghc \
        cabal-install \
        libmpfr-dev \
       && apt-get clean \
       && rm -rf /var/lib/apt/lists/*

# Checkout elm compiler
WORKDIR /tmp
RUN git clone -b $branch https://github.com/elm/compiler.git

# Build a statically linked elm binary
WORKDIR /tmp/compiler
RUN git checkout $commit
RUN rm worker/elm.cabal
RUN cabal new-update
RUN cabal new-configure --disable-executable-dynamic --ghc-option=-optl=-static --ghc-option=-optl=-pthread
RUN cabal new-build
RUN strip -s ./dist-newstyle/build/*-linux/ghc-*/elm-*/x/elm/build/elm/elm
RUN cp ./dist-newstyle/build/*-linux/ghc-*/elm-*/x/elm/build/elm/elm /usr/local/bin/elm
