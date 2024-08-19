ARG BUILD_FROM=ghcr.io/home-assistant/base-debian:16.2.1
FROM ${BUILD_FROM}

ENV LANG C.UTF-8

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN apt update 
RUN apt install -y --no-install-recommends build-essential git autoconf automake libtool \
       libpopt-dev libconfig-dev libasound2-dev avahi-daemon libavahi-client-dev libssl-dev libsoxr-dev \
       libplist-dev libsodium-dev libavutil-dev libavcodec-dev libavformat-dev uuid-dev libgcrypt-dev xxd 
WORKDIR /root
RUN git clone https://github.com/mikebrady/shairport-sync.git 
WORKDIR /root/shairport-sync
RUN autoreconf -fi 
RUN ./configure \
        --with-alsa \
        --with-pipe \
        --with-avahi \
        --with-ssl=openssl \
        --with-soxr \
        --with-metadata \
        --with-airplay-2 
RUN make 
RUN make install

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="Shairport2 Sync" \
    io.hass.description="Shairport2 Sync for Hass.io" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Côme NOËL <come@no-el.fr>"
