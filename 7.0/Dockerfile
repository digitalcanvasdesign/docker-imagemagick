FROM debian:stretch

LABEL maintainer="Jason Raimondi <jason@raimondi.us>"

WORKDIR /usr/local/src

ENV MAGICK_VERSION="7.0.7-3"
ENV MAGICK_URL="https://www.imagemagick.org/download/ImageMagick-$MAGICK_VERSION.tar.gz" \
    BUILD_DEPENDENCIES="\
        autoconf \
        ca-certificates \
		dpkg-dev \
		file \
		g++ \
		gcc \
		libc-dev \
		make \
		pkg-config \
        \
        wget\
        " \
    RUN_DEPENDENCIES="\
        ghostscript \
        "

USER root

RUN apt-get update \
    && apt-get install -y --no-install-recommends $BUILD_DEPENDENCIES $RUN_DEPENDENCIES \
    \
    && wget -q $MAGICK_URL -O imagemagick.tar.gz \
    && mkdir /usr/local/src/imagemagick \
    && tar zxf imagemagick.tar.gz -C /usr/local/src/imagemagick --strip-components=1 \
    && ( \
        cd /usr/local/src/imagemagick \
        && ./configure \
        && make \
        && make install \
        && ldconfig /usr/local/lib \
        && make check \
    ) \
    && rm imagemagick.tar.gz \
    \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false $BUILD_DEPENDENCIES \
    && apt-get clean \
    \
    && groupadd -r imagemagick \
    && useradd -r -g imagemagick -s /sbin/nologin imagemagick

USER imagemagick
