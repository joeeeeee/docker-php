ARG MYSQL_VERSION
FROM mysql:${MYSQL_VERSION}

LABEL maintainer="wuyanzhou"

# 设置时区

ARG TZ=UTC
ENV TZ ${TZ}
RUN ln -snf /user/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone && chown -R mysql:root /var/lib/mysql/

COPY my.cnf /etc/mysql/conf.d/my.cnf

RUN chmod 0444 /etc/mysql/conf.d/my.cnf

CMD ["mysqld"]

EXPOSE 3306