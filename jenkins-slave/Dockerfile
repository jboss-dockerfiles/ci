FROM jboss/base-jdk:8

USER root

RUN yum -y install openssh-server git python-setuptools docker which gcc python-devel golang rubygem-bundler ruby-devel libxslt-devel libxml2-devel make gcc-c++ && \
    yum clean all

RUN groupadd -r jenkins -g 175 && \
    useradd -u 175 -r -g jenkins -m -d /home/jenkins -s /bin/bash -c "Jenkins user" jenkins && \
    echo "jenkins:jenkins" | chpasswd

# Install nodejs
RUN curl -o - http://nodejs.org/dist/v0.12.0/node-v0.12.0-linux-x64.tar.gz | tar --strip-components 1 -xzf - -C /usr/local

RUN localedef -i en_US -f UTF-8 en_US.UTF-8

ENV LC_ALL=en_US.UTF-8
ENV LANG=en_US.UTF-8

USER jenkins
ENV HOME /home/jenkins
WORKDIR /home/jenkins

# Install PIP
RUN easy_install --user pip

# Disable host verification
RUN git config --global http.sslVerify false

# Install Dock
RUN git clone https://gitlab.mw.lab.eng.bos.redhat.com/cloudenablement/dock.git && \
    cd dock && \
    git checkout ce-ci && \
    easy_install --user .

# Install custom behave
RUN git clone https://gitlab.mw.lab.eng.bos.redhat.com/cloudenablement/behave.git && \
    cd behave && \
    git checkout ce-ci && \
    easy_install --user .

# Install STI
RUN git clone https://github.com/openshift/source-to-image.git && \
    cd source-to-image && \
    git checkout v1.0.1 && \
    hack/build-go.sh && \
    ln -sf /home/jenkins/source-to-image/_output/local/go/bin/sti /home/jenkins/.local/bin

# Instal CTF
RUN git clone https://gitlab.mw.lab.eng.bos.redhat.com/cloudenablement/ctf.git && \
    cd ctf && \
    git checkout ce && \
    easy_install --user .

# Get the docker scripts, behave and ansible
RUN /home/jenkins/.local/bin/pip install --user docker-scripts && \
    /home/jenkins/.local/bin/pip install --user docker-py==1.2.3 && \
    /home/jenkins/.local/bin/pip install --user ansible

# install dockerfile_lint
RUN npm install git+https://github.com/redhataccess/dockerfile_lint && \
    ln -sf /home/jenkins/node_modules/.bin/dockerfile_lint /home/jenkins/.local/bin

# switch back to root from sshd
USER root
EXPOSE 22
RUN mkdir /var/run/sshd
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -N ''
CMD ["/usr/sbin/sshd", "-D"]
