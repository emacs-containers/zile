FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      curl ca-certificates pkg-config \
      libc-dev gcc make help2man \
      libncurses5-dev libgc-dev libacl1-dev \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
COPY checksum checksum
ADD https://ftp.gnu.org/gnu/zile/zile-2.4.14.tar.gz zile.tar.gz
RUN sha256sum zile.tar.gz && sha256sum -c checksum
RUN mkdir zile && tar -C zile --strip-components 1 -xf zile.tar.gz
WORKDIR /build/zile
RUN ./configure
RUN make
RUN make install

FROM debian:buster-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
      libacl1 \
      libgc1c2 \
 && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/ /usr/local/
CMD ["zile"]
