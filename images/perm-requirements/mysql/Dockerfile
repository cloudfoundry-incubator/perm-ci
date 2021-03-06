FROM cfperm/requirements:base

# Adapted from bosh's CI:
# https://github.com/cloudfoundry/bosh/blob/master/ci/docker/main-mysql/Dockerfile

ENV DEBIAN_FRONTEND noninteractive

RUN \
  echo "mysql-server mysql-server/root_password password password" | debconf-set-selections && \
  echo "mysql-server mysql-server/root_password_again password password" | debconf-set-selections && \
  echo "mysql-community-server	mysql-community-server/root-pass	password password" | debconf-set-selections && \
  echo "mysql-community-server	mysql-community-server/re-root-pass	password password" | debconf-set-selections && \
  echo "mysql-community-server	mysql-community-server/data-dir	note" | debconf-set-selections && \
  echo "mysql-community-server	mysql-community-server/remove-data-dir	boolean	false" | debconf-set-selections

# Try multiple keyservers in case of flakiness
RUN \
  apt-get update -qq && \
  apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys 5072E1F5 || \
    apt-key adv --keyserver pgp.mit.edu --recv-keys 5072E1F5 || \
    apt-key adv --keyserver keyserver.pgp.com --recv-keys 5072E1F5 && \
  echo "deb http://repo.mysql.com/apt/ubuntu/ trusty mysql-5.7" | tee -a /etc/apt/sources.list.d/mysql.list && \
  apt-get update -qq && \
  apt-get install -yqq --no-install-recommends \
    mysql-server && \
  apt-get clean

RUN \
  rm -rf \
    /var/lib/mysql && \
  mkdir -p \
    /var/lib/mysql \
    /var/run/mysqld && \
  chown -R mysql:mysql \
    /var/lib/mysql \
    /var/run/mysqld && \
  chmod 777 /var/run/mysqld
