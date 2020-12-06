# build image
FROM ubuntu:16.04

LABEL description="Bootstrap image to use for dev with Ubuntu 16.04" \
      maintainer="Facchin < twitter @patrickfacchin >" \
      vendor=Facchin \
      name="facchin.devbase.bootstrap.ubuntu-16.04"

RUN echo "facchin.devbase.bootstrap.ubuntu-16.04" > /image.txt

# TODO: Change TIMEZONE=UTC
ENV TIMEZONE=America/Sao_Paulo \
    LANGUAGE=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8 \
    DEBIAN_FRONTEND=noninteractive \
    TERM=xterm

# bootstrap
RUN apt-get clean -y \
    && apt-get update -y \
    && apt-get upgrade -y \
    && apt-get install -y locales \
    && export LANGUAGE=${LANGUAGE} \
    && export LANG=${LANG} \
    && export LC_ALL=${LC_ALL} \
    && locale-gen ${LANG} \
    && dpkg-reconfigure --frontend noninteractive locales \
    && apt-get install --no-install-recommends -yq \
        apt-transport-https \
        apt-utils \
        ca-certificates \
        lsb-release \
        python-software-properties \
        software-properties-common \
        tzdata \
    && echo "#!/bin/sh\nexit 0" > /usr/sbin/policy-rc.d \
    && ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    && echo "${TIMEZONE}" | tee /etc/timezone \
    && dpkg-reconfigure --frontend noninteractive tzdata \
    && apt-add-repository -y multiverse \
    && apt-get update -y \
    && apt-get upgrade -y

# entrypoint
RUN echo $'#!/usr/bin/env sh\n\
while true; do sleep 10; done;' > /usr/bin/entrypoint && \
    chmod +x /usr/bin/entrypoint
    
# cleanup
RUN apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /tmp/* /var/tmp/* \
    && find /var/cache/apt/archives /var/lib/apt/lists \
       -not -name lock \
       -type f \
       -delete \
    && find /var/cache -type f -delete \
    && find /var/log -type f | while read f; do echo -n '' > ${f}; done
