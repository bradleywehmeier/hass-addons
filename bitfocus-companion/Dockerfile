ARG BUILD_FROM
FROM $BUILD_FROM

RUN apt-get update && apt-get install -y --no-install-recommends \
      apt-transport-https \
      lsb-release \
      curl \
      gnupg \
      build-essential \
      python \
      git \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN export DISTRO=$(lsb_release -c -s) \
    && curl -s https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add - \
    && echo "deb https://deb.nodesource.com/node_8.x ${DISTRO} main" > /etc/apt/sources.list.d/nodesource.list \
    && echo "deb-src https://deb.nodesource.com/node_8.x ${DISTRO} main" >> /etc/apt/sources.list.d/nodesource.list \
    && apt-get update && apt-get install -y --no-install-recommends \
      nodejs=8.\* \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV HOME=/root

WORKDIR /root
ENV APPDIR=/root
ENV ELECTRON_CACHE=$HOME/.cache/electron
ENV ELECTRON_BUILDER_CACHE=$HOME/.cache/electron-builder
ENV PATH="$HOME/.yarn/bin:$PATH"

# Clone repository and set as workdir 
RUN cd /root && \
    git clone https://github.com/bitfocus/companion.git && \
    mv companion/.[!.]* . && \
    mv companion/* . && \
    rm -rf companion && \
# Installation Prep
    curl -L https://yarnpkg.com/latest.tar.gz | tar xvz && mv yarn-v* $HOME/.yarn && \
    apt-get update && apt-get install -y --no-install-recommends apt-utils \ 
     libudev-dev \
     libgusb-dev && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    $APPDIR/tools/update.sh && \
    $APPDIR/tools/build_writefile.sh

EXPOSE 8000/tcp
EXPOSE 51234/tcp
EXPOSE 51235/udp
EXPOSE 12321/udp
EXPOSE 7788/tcp
CMD ["node", "./headless.js", "eth0"]
