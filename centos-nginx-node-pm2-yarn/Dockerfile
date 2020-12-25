FROM centos:latest

ARG NODE_VER=14
ARG TEMP_DIR=/tmp

# 拷贝 nginx.repo 到容器
COPY nginx.repo $TEMP_DIR

# 更新所有已安装包与安装 sudo
RUN yum update -y && yum install sudo -y \
  #
  # 安装系统编译相关程序
  && yum install -y gcc gcc-c++ automake autoconf libtool make \
  #
  # 安装 Nginx
  # https://www.nginx.com/resources/wiki/start/topics/tutorials/install
  && cat $TEMP_DIR/nginx.repo | sudo tee /etc/yum.repos.d/nginx.repo \
  && sudo yum install nginx -y \
  #
  # 安装 Node.js
  # https://github.com/nodesource/distributions#installation-instructions-1
  && curl -sL https://rpm.nodesource.com/setup_$NODE_VER.x | sudo bash - \
  #
  # 安装 Yarn
  # https://classic.yarnpkg.com/en/docs/install/#centos-stable
  && curl -sL https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo \
  && sudo yum install yarn -y \
  #
  # 安装 PM2
  && yarn global add pm2 \
  #
  # 清理操作
  && rm -rf $TEMP_DIR/*
