FROM ubuntu:ubuntu18

MAINTAINER savipavan@gmail.com

ARG PACKAGES=ca-certificates\ git

ENV PUPPERWARE_ANALYTICS_APP_NAME="puppetserver" \
    PUPPERWARE_ANALYTICS_ENABLED=false \
    PUPPETSERVER_JAVA_ARGS="-Xms512m -Xmx512m" \
    PATH=/opt/puppetlabs/server/bin:/opt/puppetlabs/puppet/bin:/opt/puppetlabs/bin:$PATH \
    SSLDIR=/etc/puppetlabs/puppet/ssl \
    LOGDIR=/var/log/puppetlabs/puppetserver \
    PUPPETSERVER_HOSTNAME="" \
    DNS_ALT_NAMES="" \
    PUPPET_MASTERPORT=8140 \
    AUTOSIGN="" \
    PUPPETSERVER_MAX_ACTIVE_INSTANCES=1 \
    PUPPETSERVER_MAX_REQUESTS_PER_INSTANCE=0 \
    CA_ENABLED=true \
    CA_HOSTNAME=puppet \
    CA_MASTERPORT=8140 \
    CA_ALLOW_SUBJECT_ALT_NAMES=false \
    USE_PUPPETDB=true \
    PUPPETDB_SERVER_URLS=https://puppetdb:8081 \
    PUPPET_STORECONFIGS_BACKEND="puppetdb" \
    PUPPET_STORECONFIGS=true \
    PUPPET_REPORTS="puppetdb"
    
# NOTE: this is just documentation on defaults
EXPOSE 8140

ENTRYPOINT ["dumb-init", "/docker-entrypoint.sh"]
CMD ["foreground"]

ADD https://github.com/Yelp/dumb-init/releases/download/v"$DUMB_INIT_VERSION"/dumb-init_"$DUMB_INIT_VERSION"_amd64.deb /

COPY docker/puppetserver/docker-entrypoint.sh \
     docker/puppetserver/healthcheck.sh \
      /
COPY docker/puppetserver/docker-entrypoint.d /docker-entrypoint.d
# k8s uses livenessProbe, startupProbe, readinessProbe and ignores HEALTHCHECK
HEALTHCHECK --interval=10s --timeout=15s --retries=12 --start-period=3m CMD ["/healthcheck.sh"]

# no need to pin versions or clear apt cache as its still being used
# hadolint ignore=DL3008,DL3009
RUN chmod +x /docker-entrypoint.sh /healthcheck.sh && \
    apt-get update && \
    apt-get install -y --no-install-recommends $PACKAGES && \
    dpkg -i dumb-init_"$DUMB_INIT_VERSION"_amd64.deb && \
    rm dumb-init_"$DUMB_INIT_VERSION"_amd64.deb
