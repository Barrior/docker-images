FROM ubuntu:latest

ARG PROGRAM_DIR=/opt
ARG NODE_VERSION_LTS=10.15.2

ENV HOME=/root

COPY policy.xml $PROGRAM_DIR

RUN set -x \
  #
  # 更新源及已安装的包
  && apt-get -y update && apt-get -y upgrade \
  #
  # 安装编译相关（node-gyp需要）
  && apt-get install -y g++ make python \
  #
  # 安装 curl
  && apt-get install -y curl \
  #
  # 安装 PDF 解析程序和图像处理程序 ImageMagick
  && apt-get install -y ghostscript imagemagick \
  #
  # 打开 ImageMagick PDF 委托处理
  && mv $PROGRAM_DIR/policy.xml /etc/ImageMagick-6/policy.xml \
  #
  # 安装 Word to PDF 转换程序
  && apt-get install -y libreoffice unoconv \
  #
  # 切换到程序安装目录
  && cd $PROGRAM_DIR \
  #
  # 安装音视频处理程序 FFmpeg
  # 静态构建方式安装，官方参考 https://johnvansickle.com/ffmpeg/
  && curl -OL https://johnvansickle.com/ffmpeg/builds/ffmpeg-git-amd64-static.tar.xz \
  && tar xvf ffmpeg-git-amd64-static.tar.xz && mv ffmpeg-*-static ffmpeg-amd64-static \
  && mv ffmpeg-amd64-static/ffmpeg ffmpeg-amd64-static/ffprobe /usr/local/bin/ \
  #
  # 移除安装文件
  && cd / && rm -rf $PROGRAM_DIR/* \
  #
  # 安装 nvm
  # 官方指南 https://github.com/creationix/nvm
  && curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash \
  #
  # 让 nvm 即时可用
  && export NVM_DIR="$HOME/.nvm" \
  && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" \
  && [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" \
  #
  # 安装 node 三个版本
  && nvm install 9.5 && nvm install 11 && nvm install $NODE_VERSION_LTS  \
  #
  # 安装和配置默认 pm2
  && npm i -g pm2 --verbose \
  && ln -s $HOME/.nvm/versions/node/v$NODE_VERSION_LTS/bin/pm2 /usr/local/bin

ENV NVM_DIR=$HOME/.nvm
ENV PATH=$NVM_DIR/versions/node/v$NODE_VERSION_LTS/bin:$PATH