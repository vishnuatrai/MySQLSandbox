FROM ubuntu:xenial
MAINTAINER swapbyt3s@gmail.com

LABEL vendor="Swapbyt3s" \
      description="MySQL Server on Ubuntu Xenial" \
      version="5.7"

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get upgrade -y && apt-get install -y \
  apt-utils \
  curl \
  tzdata \
  vim \
  unzip

ADD https://github.com/stedolan/jq/releases/download/jq-1.5/jq-linux64 /usr/local/bin/
RUN chmod +x /usr/local/bin/jq-linux64 && \
    mv /usr/local/bin/jq-linux64 /usr/local/bin/jq

# -----------------------------------------------------------------------------
# Install MySQL Server
# -----------------------------------------------------------------------------
RUN apt-get install -y mysql-server-5.7 \
                       mysql-client-core-5.7 \
                       mysql-utilities

COPY my.cnf /etc/mysql/mysql.conf.d/mysqld.cnf
RUN chown root:root /etc/mysql/mysql.conf.d/mysqld.cnf

# -----------------------------------------------------------------------------
# Install consul
# -----------------------------------------------------------------------------
ADD https://releases.hashicorp.com/consul/1.0.2/consul_1.0.2_linux_amd64.zip /root/
RUN unzip /root/consul_1.0.2_linux_amd64.zip -d /root/ && \
    rm -f /root/consul_1.0.2_linux_amd64.zip && \
    mv /root/consul /usr/local/bin/ && \
    mkdir -p /etc/consul.d/

COPY config.json /etc/consul.d/config.json

# -----------------------------------------------------------------------------
# Copy script utility
# -----------------------------------------------------------------------------
COPY entrypoint.sh /root/entrypoint.sh
COPY provisioning.sh /root/provisioning.sh
RUN chmod a+x /root/entrypoint.sh
RUN chmod a+x /root/provisioning.sh

# -----------------------------------------------------------------------------
# Clear
# -----------------------------------------------------------------------------
RUN rm -rf /var/lib/apt/lists/*

############################## INSTALLATION END ###############################

# Expose the default http port
EXPOSE 3306

# Start service on run container
ENTRYPOINT ["/root/entrypoint.sh"]
