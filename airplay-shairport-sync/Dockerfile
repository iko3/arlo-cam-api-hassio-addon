ARG BUILD_FROM=hassioaddons/base:15.0.7
FROM ${BUILD_FROM}

ENV LANG C.UTF-8

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN apk -U add \
        git \
        build-base \
        autoconf \
        automake \
        libtool \
        alsa-lib-dev \
        libdaemon-dev \
        libplist-dev \
        libsodium-dev \
        libgcrypt-dev \
        popt-dev \
        libressl-dev \
        soxr-dev \
        avahi-dev \
        mosquitto-dev \
        libconfig-dev

 ##### ALAC #####
 RUN git clone https://github.com/mikebrady/alac
 WORKDIR /alac
 RUN autoreconf -i
 RUN ./configure
 RUN make
 RUN make install
 WORKDIR /
 ##### ALAC END #####

 RUN cd /root \
 && git clone https://github.com/mikebrady/shairport-sync.git \
 && cd shairport-sync \
 && autoreconf -i -f \
 && ./configure \
        --with-alsa \
        --with-pipe \
        --with-avahi \
        --with-ssl=openssl \
        --with-soxr \
        --with-metadata \
        --with-airplay-2 \
        --with-mqtt-client \
        --with-apple-alac \
 && make \
 && make install \
 && cd / \
 && apk --purge del \
        git \
        build-base \
        autoconf \
        automake \
        libtool \
        alsa-lib-dev \
        libdaemon-dev \
        popt-dev \
        libressl-dev \
        soxr-dev \
        avahi-dev \
        libconfig-dev \
 && apk add \
        dbus \
        alsa-lib \
        alsa-plugins-pulse \
        libdaemon \
        popt \
        libressl \
        soxr \
        avahi \
        libconfig \
 && rm -rf \
        /etc/ssl \
        /var/cache/apk/* \
        /lib/apk/db/* \
        /root/shairport-sync

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="Shairport Sync" \
    io.hass.description="Shairport Sync for Hass.io" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Oleg Andrushko <oleg@andrushko.net>"