FROM rancher/agent-base:v0.3.0

ARG ARCH

RUN apt-get -q update && apt-get -qy install \
      curl \
      vim-tiny \
      iputils-ping \
      netcat \
      software-properties-common && \
    add-apt-repository ppa:vbernat/haproxy-1.6 && \
    apt-get -q update && apt-get -qy install haproxy && \
    rm -rf /var/lib/apt/lists/* && \
    rm -f /bin/sh && ln -s /bin/bash /bin/sh

RUN mkdir -p /etc/haproxy/

COPY healthcheck /usr/bin/
COPY haproxy_reload.sh /usr/bin
COPY haproxy_template.cfg /etc/haproxy/haproxy_template.cfg

ENV TINI_VERSION v0.15.0-zambon.2
ENV TINI_URL_amd64=https://github.com/zambon/tini/releases/download/${TINI_VERSION}/tini-${ARCH} \
    TINI_URL_arm=https://github.com/zambon/tini/releases/download/${TINI_VERSION}/tini-${ARCH} \
    TINI_URL_ppc64le=https://github.com/zambon/tini/releases/download/${TINI_VERSION}/tini-ppc64el \
    TINI_URL=TINI_URL_${ARCH}

RUN curl -fsSLo /tini ${!TINI_URL}
RUN chmod +x /tini

ENTRYPOINT ["/rancher-entrypoint.sh", "/tini", "--"]
CMD ["healthcheck"]
