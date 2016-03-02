FROM node


# Install global packages
RUN npm install -g nodemon
RUN npm install -g pm2
RUN npm install -g babel
RUN npm install -g babel-cli
RUN npm install -g bower
RUN pm2 set pm2:passwd 2pm_wordpass
RUN pm2 install pm2-redis
RUN pm2 install pm2-postgres
RUN pm2 install pm2-webshell
RUN pm2 set pm2-redis:hostname 0.0.0.0
RUN pm2 set pm2-redis:port 6371
RUN pm2 set pm2-postgres:hostname 0.0.0.0
RUN pm2 set pm2-postgres:port 5431
RUN pm2 conf pm2-webshell:bind 0.0.0.0
RUN pm2 conf pm2-webshell:port 1337
RUN pm2 conf pm2-webshell:password baz

# Provides cached layer for node_modules
ADD package.json /tmp/package.json
ADD bower.json /tmp/bower.json
RUN cd /tmp && npm install --production && bower --allow-root install
RUN mkdir -p /src && cp -a /tmp/node_modules /src/ && cp -a /tmp/bower_components /src/

# Define working directory
WORKDIR /src
ADD . /src

# Expose port
EXPOSE 3001

# Run app using nodemon
CMD ["node", "/src/pm2_config.js"]
