FROM postgres:9.4-alpine

ENV DEFAULT_TIMEZONE UTC

RUN echo http://mirror.lzu.edu.cn/alpine/v3.6/main > /etc/apk/repositories; \
    echo http://mirror.lzu.edu.cn/alpine/v3.6/community >> /etc/apk/repositories
# Install some packages to use WAL
RUN apk add --no-cache \
      build-base \
      curl \
      libc6-compat \
      libffi-dev \
      linux-headers \
      python-dev \
      py-pip \
      py-cryptography
RUN pip install -i https://mirrors.aliyun.com/pypi/simple/ --upgrade pip

RUN pip --no-cache-dir install -i https://mirrors.aliyun.com/pypi/simple/ 'wal-e<1.0.0' envdir \
    && rm -rf /var/cache/apk/* /tmp/* /var/tmp/*

# Add wale script
COPY setup-wale.sh /docker-entrypoint-initdb.d/

#Healthcheck to make sure container is ready
HEALTHCHECK CMD pg_isready -U postgres || exit 1

# Add and configure entrypoint and command
COPY entrypoint.sh /
ENTRYPOINT ["/entrypoint.sh"]
CMD ["postgres"]

VOLUME ["/var/run/postgresql", "/usr/share/postgresql/", "/var/lib/postgresql/data", "/tmp"]
