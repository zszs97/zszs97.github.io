FROM node:14.5.0-slim

# Create app directory
RUN mkdir -p /opt/src/app
WORKDIR /opt/src/app

RUN apt-get update && apt-get -y install curl

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./
RUN npm install

RUN sed -i "s/archive.ubuntu./mirrors.aliyun./g" /etc/apt/sources.list
RUN sed -i "s/deb.debian.org/mirrors.aliyun.com/g" /etc/apt/sources.list
RUN sed -i "s/security.debian.org/mirrors.aliyun.com\/debian-security/g" /etc/apt/sources.list
# Bundle and pre-compile the wasm files
COPY *.wasm ./
COPY pre.sh ./
RUN chmod +x pre.sh
RUN ./pre.sh

# Bundle the JS bootstrap server
COPY *.js ./

# Listen http server port 
EXPOSE 9000
ENTRYPOINT [ "node", "server.js" ]



