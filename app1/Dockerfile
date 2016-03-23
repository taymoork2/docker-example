FROM phusion/baseimage:latest

# File Author / Maintainer
MAINTAINER Maintainer

# Use baseimage-docker's init process.
CMD ["/sbin/my_init"]

# Update System and install Utilites and Node
RUN apt-get update && apt-get upgrade -y -o Dpkg::Options::="--force-confold"
RUN add-apt-repository -y -r ppa:chris-lea/node.js
RUN curl -sL https://deb.nodesource.com/setup_5.x | bash -
RUN apt-get install -y build-essential libkrb5-dev make python git vim nodejs

# Install global packages
RUN npm install -g babel
RUN npm install -g babel-cli
RUN npm install -g bower
RUN npm install pm2@latest -g
RUN pm2 update
RUN pm2 set pm2:passwd 2pm_wordpass

# Provides cached layer for node_modules
ADD package.json /tmp/package.json
ADD bower.json /tmp/bower.json
RUN cd /tmp && npm install --production && bower --allow-root install
RUN mkdir -p /src && cp -a /tmp/node_modules /src/ && cp -a /tmp/bower_components /src/

# Define working directory
WORKDIR /src
ADD . /src

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Start App
CMD ["node", "/src/pm2_config.js"]