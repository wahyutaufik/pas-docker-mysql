FROM ubuntu:trusty
MAINTAINER Wahyu Taufik <wahyutaufik37@gmail.com>

# Add MySQL configuration
ADD my.cnf /etc/mysql/conf.d/my.cnf
ADD mysqld_charset.cnf /etc/mysql/conf.d/mysqld_charset.cnf

RUN apt-get update && \
    apt-get -yq install mysql-server-5.5 pwgen && \
    rm -rf /var/lib/apt/lists/* && \
    rm /etc/mysql/conf.d/mysqld_safe_syslog.cnf && \
    if [ ! -f /usr/share/mysql/my-default.cnf ] ; then cp /etc/mysql/my.cnf /usr/share/mysql/my-default.cnf; fi && \
    mysql_install_db > /dev/null 2>&1 && \
    touch /var/lib/mysql/.EMPTY_DB

# Add MySQL scripts
ADD import_sql.sh /import_sql.sh
ADD run.sh /run.sh

ENV MYSQL_USER=admin \
    MYSQL_PASS=**Random** \
    ON_CREATE_DB=**False** \
    REPLICATION_MASTER=**False** \
    REPLICATION_SLAVE=**False** \
    REPLICATION_USER=replica \
    REPLICATION_PASS=replica \
    ON_CREATE_DB=**False**

# Add VOLUMEs to allow backup of config and databases
# VOLUME  ["/etc/mysql", "/var/lib/mysql"]

RUN /usr/sbin/usermod -u 1000 mysql && \
    /usr/sbin/groupmod -g 1000 mysql && \
    /bin/chown -R mysql:mysql /var/run/mysqld

EXPOSE 3306
CMD ["/run.sh"]