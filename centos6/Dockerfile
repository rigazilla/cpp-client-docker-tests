FROM centos:6

RUN yum -y update && \
   yum -y install \
   cmake \
   deltarpm \
   iputils \
   jdg-cpp-client \
   java-1.8.0-openjdk-devel \
   maven30 \
   openssl-devel \
   rpmdevtools \
   tar \
   valgrind \
   wget && \
   yum -y groupinstall "Development Tools" && \
   yum clean all

RUN wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
RUN yum -y install maven


RUN useradd -ms /bin/bash jboss

# Set the working directory to jboss' user home directory
WORKDIR /home/jboss

# Specify the user which should be used to execute all commands below
USER jboss


