FROM centos:7

RUN yum -y update && \
   yum -y install \
   wget \
   yum -y install \
   cmake \
   deltarpm \
   iputils \
   jdg-cpp-client \
   java-1.8.0-openjdk-devel \
   openssl-devel \
   rpmdevtools \
   tar \
   valgrind \
   maven30 \
   protobuf-devel \
   cyrus-sasl-devel \
   cyrus-sasl-plain \
   cyrus-sasl-md5 \
   cyrus-sasl-gssapi \
   krb5-workstation && \
   yum -y groupinstall "Development Tools" && \
   yum clean all

RUN wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
RUN yum -y install apache-maven

RUN yum -y install which
RUN yum -y install protobuf-static

RUN useradd -ms /bin/bash jboss

# Set the working directory to jboss' user home directory
WORKDIR /home/jboss

# Specify the user which should be used to execute all commands below
USER jboss


