FROM ubuntu:18.04

MAINTAINER savipavan@gmail.com

RUN rpm --import https://yum.puppetlabs.com/RPM-GPG-KEY-puppetlabs && \
    rpm -ivh http://yum.puppetlabs.com/puppetlabs-release-el-6.noarch.rpm

RUN apt-get install yum-utils git wget curl 

