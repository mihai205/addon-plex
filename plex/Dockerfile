ARG BUILD_FROM=ghcr.io/hassio-addons/ubuntu-base/amd64:6.2.1
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base system
ARG BUILD_ARCH=amd64
ARG RELEASE=1.41.0.8992-8463ad060
RUN \
    apt-get update \
    \
    && apt-get install -y --no-install-recommends \
        xmlstarlet=1.6.1-2 \
        uuid-runtime=2.31.1-0.4ubuntu3.7 \
        unrar=1:5.5.8-1 \
        unzip=6.0-21ubuntu1 \
        cifs-utils=2:6.8-1 \
    \
    && if [ "${BUILD_ARCH}" = "aarch64" ]; then ARCH="aarch64"; fi \
    && if [ "${BUILD_ARCH}" = "amd64" ]; then ARCH="x86_64"; fi \
    && if [ "${BUILD_ARCH}" = "armv7" ]; then ARCH="armv7hf"; fi \
    && if [ "${BUILD_ARCH}" = "i386" ]; then ARCH="x86"; fi \
    \
    && curl -J -L -o /tmp/plexmediaserver.tgz \
        "https://downloads.plex.tv/plex-media-server-new/${RELEASE}/synology/PlexMediaServer-${RELEASE}-${ARCH}_DSM6.spk" \
    \
    && mkdir /usr/lib/plexmediaserver \
    && tar -xOf /tmp/plexmediaserver.tgz package.tgz | tar -xzf - -C /usr/lib/plexmediaserver/ \
    \
    && curl -J -L -o /tmp/webtool.zip \
        "https://github.com/ukdtom/WebTools.bundle/releases/download/3.0.0/WebTools.bundle.zip" \
    && unzip -d /opt  /tmp/webtool.zip \
    \
    && apt-get -y remove --purge unzip \
    && apt-get -y autoremove \
    && apt-get -y clean \
    \
    && rm -fr \
        /tmp/* \
        /var/{cache,log}/* \
        /var/lib/apt/lists/*

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
    maintainer="Franck Nijhof <frenck@addons.community>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Home Assistant Community Add-ons" \
    org.opencontainers.image.authors="Franck Nijhof <frenck@addons.community>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://addons.community" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
