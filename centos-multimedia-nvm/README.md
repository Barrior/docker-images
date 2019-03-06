## centos-multimedia-nvm

A Integration environment of the Node.js application is used to convert images, PDFs, videos, and so on. 

### System & Software

```bash
centos latest

# Node Manager
nvm 0.34
pm2 3.3.1

# Included Node versions
node 9.5.0         # 9.5 stable
node 10.15.2       # 10.15 stable (default)
node 11.10.1       # latest version

# Word to PDF
LibreOffice 5.3
unoconv 0.6

# Image Handler
ImageMagick 7.0.8-25
ghostscript 9.07

# Audio & Video Handler
ffmpeg 4.1
```

### Usage in Dockerfile

```bash
FROM  barrior/centos-multimedia-nvm

WORKDIR /app

COPY . .

# Simple way to start an application with PM2 3.3.1 and Node 10.15.2 by default.
# ENTRYPOINT npm i && pm2 start app.js

# Use specific version by nvm that require command `. $NVM_DIR/nvm.sh` before.
ENTRYPOINT . $NVM_DIR/nvm.sh \
  && nvm use 9.5 \
  && npm install --production \
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