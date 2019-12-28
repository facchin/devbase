# build image
FROM alpine:3.10.3

LABEL description="Bootstrap image to use for dev with Alpine 3.10.3" \
      maintainer="Facchin < twitter @patrickfacchin >" \
      vendor=Facchin \
      name="facchin.devbase.bootstrap.alpine-3.10.3"

RUN echo "facchin.devbase.bootstrap.alpine-3.10.3" > /image.txt

# TODO: Change TIMEZONE=UTC
ENV TIMEZONE=America/Sao_Paulo

# bootstrap
RUN apk update \
    && apk upgrade --force \
    && apk add --no-cache \
        bash \
        bash-completion \
        ca-certificates \
        coreutils \
        musl-utils \
        tzdata \
        openssl \
    && update-ca-certificates 2>/dev/null || true \
    && ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    && echo "${TIMEZONE}" | tee /etc/timezone

# cleanup
RUN rm -rf /tmp/* /var/cache/apk/* \
    && find /var/log -type f | while read f; do echo -n '' > ${f}; done