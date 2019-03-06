FROM centos:latest

ARG PROGRAM_DIR=/opt
ARG NODE_VERSION_LTS=10.15.2

ENV HOME=/root

# 重建 rpm 数据库
RUN yum clean all && yum makecache && rpm --rebuilddb \
  #
  # 更新所有已安装包
  && yum update -y \
  #
  # 安装系统编译相关程序
  && yum install -y gcc gcc-c++ automake autoconf libtool make \
  #
  # 安装图形图像相关基础依赖库
  && yum install -y fftw* freetype* zlib \
    OpenEXR* openjpeg* openjpeg2* \
    libjpeg* libpng* libwebp* libtiff* libwmf* giflib* libungif* \
    libX11* libXext* libXt* lcms* librsvg2* \
    djvulibre-devel jasper-devel libtool-ltdl-devel \
  #
  # 安装 Word to PDF 转换程序
  && yum install -y libreoffice unoconv \
  #
  # 切换到程序安装目录
  && cd $PROGRAM_DIR \
  #
  # 安装 PDF 解析程序
  # && curl -OL https://github.com/ArtifexSoftware/ghostpdl-downloads/releases/download/gs926/ghostscript-9.26.tar.gz \
  # && tar -zxvf ghostscript-9.26.tar.gz && cd ghostscript-9.26 \
  # && ./configure && make && make install \
  # && cd $PROGRAM_DIR \
  #
  # 安装 PDF 解析程序
  && yum install -y ghostscript \
  #
  # 安装图像处理程序 ImageMagick
  # 不采用 RPM 的方式安装，因为 RPM 安装需要依赖 libgs.so.9 等基础库，不好安装。
  # 官方参考 https://imagemagick.org/script/install-source.php
  && curl -OL https://imagemagick.org/download/ImageMagick.tar.gz \
  && tar -zxvf ImageMagick.tar.gz && mv ImageMagick-* ImageMagick \
  && cd ImageMagick && ./configure && make && make install \
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