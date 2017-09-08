FROM ubuntu:xenial

# For testing purposes - can be removed if desired
RUN apt-get update && apt-get install -y procps net-tools

RUN apt-get update \
    && apt-get install -y apt-transport-https git \
    && \
    NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62; \
    found=''; \
    for server in \
        ha.pool.sks-keyservers.net \
        hkp://keyserver.ubuntu.com:80 \
        hkp://p80.pool.sks-keyservers.net:80 \
        pgp.mit.edu \
    ; do \
        echo "Fetching GPG key $NGINX_GPGKEY from $server"; \
        apt-key adv --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$NGINX_GPGKEY" && found=yes && break; \
    done; \
    test -z "$found" && echo >&2 "error: failed to fetch GPG key $NGINX_GPGKEY" && exit 1; \
    echo "deb http://nginx.org/packages/mainline/ubuntu/ xenial nginx" >> /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y unit \
    && apt-get remove --purge --autoremove -y apt-transport-https \
    && rm -rf /var/lib/apt/lists/*

STOPSIGNAL SIGTERM

CMD ["unitd", "--no-daemon", "--control", "8443"]
