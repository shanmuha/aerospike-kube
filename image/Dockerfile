# ubuntu:xenial is same base as Aerospike 3.10.0.1, saving disk space
FROM ubuntu:xenial

ENV AEROSPIKE_VERSION 3.11.1.1
ENV AEROSPIKE_SHA256 70d96d031125c98f0e6c7b45d267d28423359a6975d3ec3307f5a0d384e99206             


# Install Aerospike Server and Tools


RUN \
  apt-get update -y \
  && apt-get install -y wget python python-argparse python-bcrypt python-openssl logrotate net-tools iproute2 iputils-ping \
  && wget "https://www.aerospike.com/artifacts/aerospike-server-community/${AEROSPIKE_VERSION}/aerospike-server-community-${AEROSPIKE_VERSION}-ubuntu16.04.tgz" -O aerospike-server.tgz \
  && echo "$AEROSPIKE_SHA256 *aerospike-server.tgz" | sha256sum -c - \
  && mkdir aerospike \
  && tar xzf aerospike-server.tgz --strip-components=1 -C aerospike \
  && dpkg -i aerospike/aerospike-server-*.deb \
  && dpkg -i aerospike/aerospike-tools-*.deb \
  && mkdir -p /var/log/aerospike/ \
  && mkdir -p /var/run/aerospike/ \
  && rm -rf aerospike-server.tgz aerospike /var/lib/apt/lists/* \
  && dpkg -r wget ca-certificates \
  && dpkg --purge wget ca-certificates \
  && apt-get purge -y


# Add the Aerospike configuration specific to this dockerfile
COPY aerospike.conf /etc/aerospike/aerospike.conf
VOLUME ["/opt/aerospike/data"]
VOLUME ["/etc/aerospike/"]


# Expose Aerospike ports
#
#   3000 – service port, for client connections
#   3001 – fabric port, for cluster communication
#   3002 – mesh port, for cluster heartbeat
#   3003 – info port
#
EXPOSE 3000 3001 3002 3003

# The peer-finder script provided by google
ADD https://storage.googleapis.com/kubernetes-release/pets/peer-finder /peer-finder

# Local files
ADD install.sh /
ADD on-start.sh /
ADD aerospike.conf /

# Make scripts runnable
RUN chmod -c 755 /peer-finder && chmod -c 755 /install.sh && chmod -c 755 /on-start.sh

ENTRYPOINT /install.sh
