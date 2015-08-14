FROM jboss/base

USER root
RUN yum install httpd mod_ssl -y && yum clean all

ADD jboss_community.conf /etc/httpd/conf.d/

CMD ["/sbin/httpd", "-DFOREGROUND"]

