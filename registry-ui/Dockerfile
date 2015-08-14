FROM jboss/base:latest

RUN curl -L https://github.com/goldmann/registry-face/archive/highlight_all.tar.gz | tar zx && mv registry-face-highlight_all registry-face

USER root
RUN yum -y install python-setuptools && \
    yum clean all
USER jboss

#ENV PATH $PATH:$HOME/.local/bin
RUN easy_install --user pip
ADD requirements.txt registry-face/

RUN $HOME/.local/bin/pip install --user -r registry-face/requirements.txt

EXPOSE 8080

CMD /usr/bin/python registry-face/run.py -t 0.0.0.0 -p 8080 --prefix=jboss-community-registry.usersys.redhat.com --path=/var/lib/docker-registry
