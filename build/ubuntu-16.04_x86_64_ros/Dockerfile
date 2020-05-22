# syntax = docker/dockerfile:1.1.1-experimental
FROM ros:kinetic-ros-core-xenial

ENV MOMO_VERSION 2020.6
ENV WEBRTC_BUILD_VERSION 84.4147.0.0
ENV BOOST_VERSION 1.73.0
ENV JSON_VERSION 3.7.3
ENV CLI11_VERSION 1.9.0
ENV SDL2_VERSION 2.0.12
ENV CMAKE_VERSION 3.17.2


ARG PACKAGE_NAME

LABEL jp.shiguredo.momo=$PACKAGE_NAME

RUN rm -f /etc/apt/apt.conf.d/docker-clean; echo 'Binary::apt::APT::Keep-Downloaded-Packages "true";' > /etc/apt/apt.conf.d/keep-cache

# パッケージのインストール

COPY script/apt_install_x86_64.sh /root/
RUN  /root/apt_install_x86_64.sh \
  && apt-get install -y ros-kinetic-audio-common \
  && rm /var/cache/apt/* -R && rm /var/lib/apt/* -R

# WebRTC の取得

# ARG WEBRTC_BUILD_VERSION

COPY script/get_webrtc.sh /root/
RUN /root/get_webrtc.sh "$WEBRTC_BUILD_VERSION" ubuntu-16.04_x86_64 /root /root
# COPY webrtc/ /root/webrtc/

# コンパイラの取得

COPY script/get_llvm.sh /root/
RUN /root/get_llvm.sh /root/webrtc /root

# Boost のビルド

# ARG BOOST_VERSION

COPY script/setup_boost.sh /root/
RUN \
  set -ex \
  && /root/setup_boost.sh "$BOOST_VERSION" /root/boost-source \
  && cd /root/boost-source/source \
  && echo 'using clang : : /root/llvm/clang/bin/clang++ : ;' > project-config.jam \
  && ./b2 \
    cxxflags=' \
    ' \
    linkflags=' \
    ' \
    toolset=clang \
    visibility=global \
    target-os=linux \
    address-model=64 \
    link=static \
    variant=release \
    install \
    -j`nproc` \
    --ignore-site-config \
    --prefix=/root/boost \
    --with-filesystem

# nlohmann/json の取得

# ARG JSON_VERSION
RUN git clone --branch v$JSON_VERSION --depth 1 https://github.com/nlohmann/json.git /root/json

# CLI11 の取得

# ARG CLI11_VERSION
RUN git clone --branch v$CLI11_VERSION --depth 1 https://github.com/CLIUtils/CLI11.git /root/CLI11

# CMake のインストール
# ARG CMAKE_VERSION
COPY script/get_cmake.sh /root/
RUN /root/get_cmake.sh "$CMAKE_VERSION" Linux /root
ENV PATH "/root/cmake/bin:$PATH"
