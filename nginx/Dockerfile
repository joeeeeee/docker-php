FROM nginx:alpine

LABEL maintainer="wuyanzhou"

COPY nginx.conf /etc/nginx/

ARG CHANGE_SOURCE=true
RUN if [ ${CHANGE_SOURCE} = true ]; then \
    # Change application source from dl-cdn.alpinelinux.org to aliyun source
    sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/' /etc/apk/repositories \
;fi

RUN apk update \ 
	&& apk upgrade \
	&& apk --update add logrotate \
	&& apk add --no-cache openssl \
	&& apk add --no-cache bash

RUN apk add --no-cache curl

ARG PGID=1000
ARG PUID=1000

RUN set -x ; \
    addgroup -g ${PGID} -S www-data ; \
    adduser -u ${PUID} -D -S -G www-data www-data && exit 0 ; exit 1

ARG PHP_UPSTREAM_CONTAINER=php-fpm
ARG PHP_UPSTREAM_PORT=9000

RUN touch /var/log/messages

# 日志分割工具 logrotate
COPY logrotate/nginx /etc/logrotate.d/

RUN echo "upstream php-upstream { server ${PHP_UPSTREAM_CONTAINER}:${PHP_UPSTREAM_PORT}; }" > /etc/nginx/conf.d/upstream.conf \
    && rm /etc/nginx/conf.d/default.conf

ADD ./startup.sh /opt/startup.sh
RUN sed -i 's/\r//g' /opt/startup.sh
CMD ["/bin/bash", "/opt/startup.sh"]

EXPOSE 81 443 8080