# RedHat Linux Ruby Installation Guide

As there's no offical ruby package > 1.8.7 in Red Hat 6 you need to install new ruby versions using rvm (Ruby version manager).
This document describes the needed steps to configure rvm for your jenkins server installation.

## 1) Upgrade Packages
    sudo yum update
    sudo yum groupinstall "Development Tools"
    sudo yum install gcc-c++ patch readline readline-devel zlib zlib-devel libyaml-devel libffi-devel openssl-devel make bzip2 sqlite-devel autoconf automake libtool bison iconv-devel

## 3) Shell into root user to install rvm
    sudo su -

## 3) Install RVM ( Ruby Version Manager )
    curl -L get.rvm.io | bash -s stable

If the output shows some errors or warnings follow the displayed steps and re-init the command above.

## 4) Setup RVM Environment
    source /etc/profile.d/rvm.sh
    rvm list

## 5) Install Ruby Version
    rvm install 2.2.3

## 6) Setup Default Ruby Version
    rvm use 2.2.3 --default

## 7) Check installation
    ruby --version

## 8) Update rubygems
    gem update --system
    gem install bundler

## 9) Install some core gems and do a final Check
    gem install yard rspec httparty

    rvm list
    gem --version
    ruby -v

## 10) Install gems as tomcat user (jenkins environment)
    su -l tomcat -s /bin/bash
    rvm use 2.2.3 --default
    gem install bundler
    gem install yard rspec httparty

## 11) Jenkins shell execution

You need to determine the location of your rvm-shell:

    which rvm-shell

For using the rvm environment within jenkins job shell executions you need to point to this shell in the header of your scripts, e.g.:

````
#!/usr/local/rvm/bin/rvm-shell
````
