## centos-multimedia-nvm

用于处理图像，PDF，音视频的 `Node.js` 服务端应用程序集成环境。

### 系统环境

```bash
CentOS 7.2

# node manager
nvm 0.34
pm2 3.2.2

# word to pdf
LibreOffice 5.3
unoconv 0.6

# image handler
ImageMagick 7.0.8-25
ghostscript 9.26

# audio & video handler
ffmpeg 4.1
```

### Node 版本使用指南

`Node` 版本基于 `nvm` 管理，目前已安装三个版本

```bash
node 9.5         # 9.5 stable
node 10.15       # 10.15 stable
node 11.9        # latest version
```

在 `Docker` 中使用

```bash
RUN nvm use 9.5
```

### Dockerfile 使用示例

```bash
FROM  barrior/centos-multimedia-nvm

WORKDIR /app

COPY . .

RUN nvm use 9.5 && npm install --production

ENTRYPOINT npm run prod
```

