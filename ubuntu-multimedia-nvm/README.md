## ubuntu-multimedia-nvm

A Node.js integration environment base on the manager NVM that can also be used to convert images, PDFs, videos, and so on.

### System & Software

```bash
Ubuntu latest

# Node Manager
nvm 0.34
pm2 3.3.1

# Included Node versions
node 9.5.0         # 9.5 stable
node 10.15.2       # 10.15 stable (default)
node 11.10.1       # latest version

# Word to PDF
LibreOffice 6.0.7
unoconv 0.7

# Image Handler
ImageMagick 6.9.7
ghostscript 9.26

# Audio & Video Handler
ffmpeg 4.1
```

### Usage in Dockerfile

```bash
FROM  barrior/ubuntu-multimedia-nvm

WORKDIR /app

COPY . .

# Simple way to start an application with PM2 3.3.1 and Node 10.15.2 by default.
# RUN npm install
# ENTRYPOINT pm2 start app.js

# Use specific version by nvm that require command `. $NVM_DIR/nvm.sh` before.
RUN . $NVM_DIR/nvm.sh \
  && nvm use 9.5 \
  && npm install --production

ENTRYPOINT . $NVM_DIR/nvm.sh \
  && nvm use 9.5 \
  && pm2 start app.js
```

### More Examples

```bash
# node 10.15.2, pm2 3.3.1
pm2 start app.js

# node 9.5, pm2 3.3.1
nvm use 9.5 && pm2 start app.js

# node 11, pm2 3.3.1 
nvm install 11 && pm2 start app.js

# node lts, pm2 latest
nvm install --lts && npm i -g pm2 && pm2 start app.js
```

### Set Timezone

The default timezone is ` Asia/Shanghai `, you can change it as below and view this [list of tz database time zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) to get your local timezone name of the system.

```bash
ENV TZ=Etc/UTC

RUN ln -sf /usr/share/zoneinfo/$TZ /etc/localtime \
  && dpkg-reconfigure -f noninteractive tzdata
```
