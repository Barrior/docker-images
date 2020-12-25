## centos-nginx-node-pm2-yarn

A Integration environment of the Node.js application. 

### System & Software

```bash
Centos   latest

Nginx    latest
Node     14.x (specify version via NODE_VER variable in Dockerfile) 
PM2      latest
Yarn     latest
```

### Usage in Dockerfile

```bash
FROM  centos-nginx-node-pm2-yarn

WORKDIR /app

COPY . .

RUN yarn install --production

ENTRYPOINT pm2 start app.js && sleep 9999d
```
