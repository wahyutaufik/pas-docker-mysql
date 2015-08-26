FROM debian:wheezy

MAINTAINER Wahyu Taufik <wahyutaufik37@gmail.com>

ENV DEBIAN_FRONTEND noninteractive

COPY root/etc/apt /etc/apt

RUN apt-get update

# Install sshd
RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd

# Set password to 'admin'
RUN printf admin\\nadmin\\n | passwd

# Install MySQL
RUN apt-get install -y mysql-server mysql-client

# Install Apache
RUN apt-get install -y apache2

# Install php
RUN apt-get install -y php5 libapache2-mod-php5 php5-mcrypt php5-mysql

# Install phpMyAdmin
RUN mysqld & \
	service apache2 start; \
	sleep 5; \
	printf y\\n\\n\\n1\\n | apt-get install -y phpmyadmin; \
	sleep 15; \
	mysqladmin -u root shutdown

RUN sed -i "s#// \$cfg\['Servers'\]\[\$i\]\['AllowNoPassword'\] = TRUE;#\$cfg\['Servers'\]\[\$i\]\['AllowNoPassword'\] = TRUE;#g" /etc/phpmyadmin/config.inc.php 

EXPOSE 22
EXPOSE 80
EXPOSE 3306

CMD mysqld_safe & \
	service apache2 start; \
	/usr/sbin/sshd -D
