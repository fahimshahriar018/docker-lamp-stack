FROM centos:7
MAINTAINER "Fahim Shahriar" <fahimshahriar018@gmail.com>

#Update packages
RUN yum -y update --exclude=kernel

#RUN yum -y install systemd; yum clean all; \
#(cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
#rm -f /lib/systemd/system/multi-user.target.wants/*;\
#rm -f /etc/systemd/system/*.wants/*;\
#rm -f /lib/systemd/system/local-fs.target.wants/*; \
#rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
#rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
#rm -f /lib/systemd/system/basic.target.wants/*;\
#rm -f /lib/systemd/system/anaconda.target.wants/*;
#VOLUME [ "/sys/fs/cgroup" ]
#CMD ["/usr/sbin/init"]

#Install Apache 
RUN yum install httpd httpd-tools -y
# Install php repo 
RUN yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm  \
http://rpms.remirepo.net/enterprise/remi-release-7.rpm \
yum-utils

# enabl4 specific php version e.g: 7.3 -> 73, 7.4 ->74
RUN yum-config-manager --enable remi-php73
RUN yum install php php-mcrypt php-cli php-gd php-curl php-mysql php-ldap php-zip php-fileinfo php-dom php-mbstring -y

#Install composer using composer image
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

#Copy application to hosting directory
WORKDIR /var/www/html/
#COPY --chown=apache:apache ../ /var/www/html/
RUN  chown -R apache:apache /var/www/html && chmod -R 777 /var/www/html

COPY  ./httpd/vhost.conf  /etc/httpd/conf.d/

#Enable httpd Service
RUN systemctl enable httpd
EXPOSE 80

# Launch httpd on the foreground
CMD     ["/usr/sbin/httpd","-D","FOREGROUND"]