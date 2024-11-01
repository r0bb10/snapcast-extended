# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM=ghcr.io/hassio-addons/base
FROM $BUILD_FROM

# See https://github.com/librespot-org/librespot/wiki/Compiling#addition-features for librespot compile features.
RUN \
	apk add --no-cache --virtual .build-dependencies \
        build-base \
        cargo \
        git \
        protobuf-dev \
        pulseaudio-dev \
        avahi-dev \
    \
    && apk add --no-cache \
        dbus \
        nss \
        avahi-compat-libdns_sd \
        avahi \
        pulseaudio \
        alsa-plugins-pulse \
        snapcast \
        shairport-sync \
    \
    && cargo install \
        --locked \
        --no-default-features \
        --features with-dns-sd \
        --root /usr \
        --bin librespot \
        --version 0.6.0 \
        --verbose \
        -- librespot \
    \
    && apk del --no-cache --purge .build-dependencies \
    && rm -fr \
        /tmp/* \
        ~/.cargo \
        /usr/.crates.toml \
        /usr/.crates2.json

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="r0bb10" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Jsadusk's addons" \
    org.opencontainers.image.authors="r0bb10" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
