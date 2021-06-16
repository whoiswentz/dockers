FROM alpine:latest

RUN apk update \
       && apk add --no-cache ca-certificates ruby ruby-irb ruby-etc ruby-webrick tini \
       && apk add --no-cache --virtual .build-deps build-base linux-headers ruby-dev gnupg \
       && echo 'gem: --no-document' >> /etc/gemrc \
       && gem install oj json async-http ext_monitor bigdecimal \
       && gem install fluentd fluent-plugin-elasticsearch \
       && apk del .build-deps \
       && rm -rf /tmp/* /var/tmp/* /usr/lib/ruby/gems/*/cache/*.gem

RUN addgroup -S fluent && adduser -S -g fluent fluent \
       # for log storage (maybe shared with host)
       && mkdir -p /fluentd/log \
       # configuration/plugins path (default: copied from .)
       && mkdir -p /fluentd/etc /fluentd/plugins \
       && chown -R fluent /fluentd && chgrp -R fluent /fluentd

COPY /conf/fluent.conf /fluentd/etc/
COPY entrypoint.sh /bin/

RUN chmod +x /bin/entrypoint.sh

ENV FLUENTD_CONF="fluent.conf"

ENV LD_PRELOAD=""
EXPOSE 24224 5140

USER fluent
ENTRYPOINT ["tini",  "--", "/bin/entrypoint.sh"]
CMD ["fluentd"]