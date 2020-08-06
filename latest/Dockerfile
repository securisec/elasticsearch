FROM webhippie/alpine:latest-arm64v8

EXPOSE 9200 9300

VOLUME ["/var/lib/elasticsearch"]
WORKDIR /var/lib/elasticsearch
ENTRYPOINT ["/usr/bin/entrypoint"]
CMD ["/usr/bin/server"]

ENV JAVA_HOME=/usr/lib/jvm/java-1.8-openjdk/
ENV ELASTICSEARCH_TARBALL https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.8.1-linux-x86_64.tar.gz

RUN apk update && \
  apk upgrade && \
  mkdir -p /var/lib/elasticsearch && \
  groupadd -g 1000 elastico && \
  useradd -u 1000 -d /var/lib/elasticsearch -g elastico -s /bin/bash -M elastico && \
  apk add coreutils java-jna-native openjdk8-jre && \
  rm -rf /var/cache/apk/*

RUN mkdir -p /usr/share/elasticsearch && \
  curl -sLo - ${ELASTICSEARCH_TARBALL} | tar xvzf - --strip 1 --exclude config --exclude logs --exclude plugins --exclude LICENSE.txt --exclude NOTICE.txt --exclude README.textile --exclude bin/*.bat --exclude bin/*.exe --exclude modules/x-pack-ml -C /usr/share/elasticsearch && \
  ln -sf /usr/share/elasticsearch/bin/elasticsearch* /usr/bin/ && \
  ln -sf /usr/share/elasticsearch/bin/xpack* /usr/bin/ && \
  ln -sf /var/lib/elasticsearch/plugins /usr/share/elasticsearch/plugins && \
  chown elastico:elastico /usr/share/elasticsearch

COPY ./overlay ./overlay-amd64 /
