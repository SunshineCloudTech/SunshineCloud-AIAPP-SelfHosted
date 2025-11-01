# [Choice] Debian version (use bullseye on local arm64/Apple Silicon): bookworm, bullseye, buster
ARG VARIANT="bookworm"
ARG CUSTOM_PACKAGE_LIST="openssl"
# FROM buildpack-deps:${VARIANT}
FROM mambaorg/micromamba:2.3-debian12
# Set environment variables early
ENV DEBIAN_FRONTEND=noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN=true
ENV MAMBA_ROOT_PREFIX="/opt/micromamba"
ENV PATH="/SunshineCloud/Homebrew/bin:${PATH}"
ENV HOMEBREW_PREFIX=/SunshineCloud/Homebrew
ENV HOMEBREW_CACHE=/SunshineCloud/Homebrew/cache
# Locale environment variables
ENV LANG=zh_CN.UTF-8
ENV LANGUAGE=zh_CN:en_US
ENV LC_ALL=zh_CN.UTF-8
USER root
# Install all packages in a single layer to reduce image size
RUN apt-get update && apt-get -y install --no-install-recommends ${CUSTOM_PACKAGE_LIST} && \
    # Install Distrobox dependencies and extra packages
    apt-get install -y --no-install-recommends \
        apt-utils bash-completion bc bzip2 curl dialog diffutils findutils \
        gnupg gnupg2 gpgsm hostname iproute2 iputils-ping keyutils less \
        libcap2-bin libkrb5-3 libnss-mdns libnss-myhostname \
        locales lsof man-db manpages mtr ncurses-base \
        openssh-client passwd pigz pinentry-curses procps rsync sudo \
        tcpdump time traceroute tree tzdata unzip util-linux wget \
        xz-utils zip \
        # Extra dependencies
        git git-lfs gcc-multilib g++-multilib gcc-x86-64-linux-gnu \
        g++-x86-64-linux-gnu progress debootstrap dbus-daemon sysvinit-utils \
        software-properties-common lsb-release ca-certificates locales-all \
        build-essential nano \
        # Additional packages for later use
        apt-transport-https jq supervisor pkgconf file \
        ca-certificates crudini dbus \
        libasound2-dev libglib2.0-dev libglib2.0-0 \
        openssh-server uuid-runtime vim \
        python3-pip systemd systemd-sysv && \
    # Clean up in the same layer
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Configure locales
RUN echo "配置区域设置..." && \
    # 配置需要的 locale
    echo "zh_CN.UTF-8 UTF-8" >> /etc/locale.gen && \
    echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && \
    # 生成指定的区域设置
    locale-gen zh_CN.UTF-8 en_US.UTF-8 && \
    # 设置系统默认 locale 为简体中文
    echo "LANG=zh_CN.UTF-8" > /etc/default/locale && \
    echo "LANGUAGE=zh_CN:en" >> /etc/default/locale && \
    echo "LC_ALL=zh_CN.UTF-8" >> /etc/default/locale && \
    update-locale LANG=zh_CN.UTF-8 LC_ALL=zh_CN.UTF-8 && \
    echo "区域设置配置完成"

# Create users and configure sudo
RUN useradd -m "Administrator" -s /bin/bash && \
    echo "Administrator:123456789" | chpasswd && \
    echo "root:123456789" | chpasswd && \
    usermod -d /home/Administrator/ Administrator && \
    usermod -aG sudo Administrator && \
    useradd -m "matrix0523" -s /bin/bash && \
    echo "matrix0523:123456789" | chpasswd && \
    usermod -d /home/matrix0523/ matrix0523 && \
    usermod -aG sudo matrix0523 && \
    echo 'matrix0523 ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/matrix0523 && \
    # Create ollama user
    useradd -r -s /bin/false -U -m -d /usr/share/ollama ollama && \
    usermod -a -G ollama root && \
    usermod -a -G ollama Administrator && \
    usermod -a -G ollama matrix0523 && \
    # Setup Ollama directories and permissions
    sudo mkdir -p /var/lib/ollama/models && \
    sudo chown -R ollama:ollama /var/lib/ollama && \
    sudo chmod -R 755 /var/lib/ollama

# Configure user locale settings
RUN echo "# Locale settings for Chinese/English support" >> /home/matrix0523/.bashrc && \
    echo "export LANG=zh_CN.UTF-8" >> /home/matrix0523/.bashrc && \
    echo "export LANGUAGE=zh_CN:en" >> /home/matrix0523/.bashrc && \
    echo "export LC_ALL=zh_CN.UTF-8" >> /home/matrix0523/.bashrc && \
    echo "# Locale settings for Chinese/English support" >> /home/Administrator/.bashrc && \
    echo "export LANG=zh_CN.UTF-8" >> /home/Administrator/.bashrc && \
    echo "export LANGUAGE=zh_CN:en" >> /home/Administrator/.bashrc && \
    echo "export LC_ALL=zh_CN.UTF-8" >> /home/Administrator/.bashrc && \
    # Copy locale settings to profile
    echo "export LANG=zh_CN.UTF-8" >> /etc/profile && \
    echo "export LANGUAGE=zh_CN:en" >> /etc/profile && \
    echo "export LC_ALL=zh_CN.UTF-8" >> /etc/profile

# Create standard user directories for all users
RUN mkdir -vp /home/Administrator/Documents /home/Administrator/Downloads && \
    mkdir -vp /home/matrix0523/Documents /home/matrix0523/Downloads && \
    # Set ownership for Administrator directories
    chown -R Administrator:Administrator /home/Administrator/Documents /home/Administrator/Downloads && \
    # Set ownership for matrix0523 directories
    chown -R matrix0523:matrix0523 /home/matrix0523/Documents /home/matrix0523/Downloads && \
    # Set appropriate permissions
    chmod 755 /home/Administrator/Documents /home/Administrator/Downloads && \
    chmod 755 /home/matrix0523/Documents /home/matrix0523/Downloads

# Configure user directories
RUN mkdir -p /home/matrix0523/.local/share && \
    chown -R matrix0523:matrix0523 /home/matrix0523/.local

# Create directory structure in a single layer
RUN mkdir -vp /runner/Linux/x86_64 /runner/Linux/ARM64 /runner/Linux/Power \
        /runner/macOS/Intel/x86_64 \
        /runner/macOS/Silicon/M1/ARM64 \
        /app /SunshineCloud /lost+found \
        /DATA/ \
        /SunshineCloud/ \
        /data/ \
        /etc/supervisor/conf.d \
        /var/run/supervisor \
        /var/log/supervisor \
        /usr/share/keyrings && \
    chmod -R 755 /app && \
    chown -R matrix0523:matrix0523 /app

# Download and install micromamba and other tools in one layer
RUN cd /runner/Linux/x86_64 && curl -Ls https://micro.mamba.pm/api/micromamba/linux-64/latest | tar -xj && \
    cd /runner/Linux/ARM64 && curl -Ls https://micro.mamba.pm/api/micromamba/linux-aarch64/latest | tar -xj && \
    cd /runner/Linux/Power && curl -Ls https://micro.mamba.pm/api/micromamba/linux-ppc64le/latest | tar -xj && \
    cd /runner/macOS/Intel/x86_64 && curl -Ls https://micro.mamba.pm/api/micromamba/osx-64/latest | tar -xj && \
    cd /runner/macOS/Silicon/M1/ARM64 && curl -Ls https://micro.mamba.pm/api/micromamba/osx-arm64/latest | tar -xj && \
    # Install filebrowser
    curl -fsSL https://raw.githubusercontent.com/filebrowser/get/master/get.sh | bash && \
    # Clean up
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Configure repositories and install Java
RUN curl -fsSL https://pkg.cloudflare.com/cloudflare-main.gpg | tee /usr/share/keyrings/cloudflare-main.gpg >/dev/null && \
    echo 'deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] https://pkg.cloudflare.com/cloudflared any main' | tee /etc/apt/sources.list.d/cloudflared.list && \
    wget -qO - https://packages.adoptium.net/artifactory/api/gpg/key/public | gpg --dearmor | tee /etc/apt/trusted.gpg.d/adoptium.gpg > /dev/null && \
    echo "deb https://packages.adoptium.net/artifactory/deb/ bookworm main" | tee /etc/apt/sources.list.d/adoptium.list && \
    apt-get update && \
    apt-get install -y --no-install-recommends cloudflared && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install Homebrew
RUN mkdir -vp /SunshineCloud/Homebrew/ && \
    git clone --depth=1 https://github.com/Homebrew/brew /SunshineCloud/Homebrew && \
    chown -R matrix0523:matrix0523 /SunshineCloud/Homebrew && \
    echo 'export PATH="/SunshineCloud/Homebrew/bin:$PATH"' >> /home/matrix0523/.bashrc

# Switch to matrix0523 user for configuration
USER matrix0523

# Configure basic user settings
RUN mkdir -p /home/matrix0523/.config && \
    chown -R matrix0523:matrix0523 /home/matrix0523/.config

USER root   
# System optimization configuration
RUN echo "配置系统优化..."
# 系统级性能优化
RUN echo "应用系统级性能优化..." && \
    # 优化内核参数
    echo "# 系统性能优化配置" >> /etc/sysctl.conf && \
    echo "# 网络缓冲区优化" >> /etc/sysctl.conf && \
    echo "net.core.rmem_default = 262144" >> /etc/sysctl.conf && \
    echo "net.core.rmem_max = 16777216" >> /etc/sysctl.conf && \
    echo "net.core.wmem_default = 262144" >> /etc/sysctl.conf && \
    echo "net.core.wmem_max = 16777216" >> /etc/sysctl.conf && \
    echo "" >> /etc/sysctl.conf && \
    echo "# TCP 优化" >> /etc/sysctl.conf && \
    echo "net.ipv4.tcp_rmem = 4096 87380 16777216" >> /etc/sysctl.conf && \
    echo "net.ipv4.tcp_wmem = 4096 65536 16777216" >> /etc/sysctl.conf && \
    echo "net.ipv4.tcp_window_scaling = 1" >> /etc/sysctl.conf && \
    echo "net.ipv4.tcp_timestamps = 1" >> /etc/sysctl.conf && \
    echo "net.ipv4.tcp_sack = 1" >> /etc/sysctl.conf && \
    echo "" >> /etc/sysctl.conf && \
    echo "# 文件系统优化" >> /etc/sysctl.conf && \
    echo "fs.file-max = 2097152" >> /etc/sysctl.conf
# Configure basic user directories
USER root
RUN mkdir -p /home/Administrator/.config && \
    chown -R Administrator:Administrator /home/Administrator/.config

USER root
# Create basic autostart directories
RUN mkdir -p /home/matrix0523/.config/autostart /home/Administrator/.config/autostart && \
    chown -R matrix0523:matrix0523 /home/matrix0523/.config/autostart && \
    chown -R Administrator:Administrator /home/Administrator/.config/autostart

# Install fastfetch as matrix0523 user
USER matrix0523
RUN curl -fsSL https://raw.githubusercontent.com/ValkyrieNexus/fastfetch-universal-installer/main/install-fastfetch-universal.sh | bash
USER root

# Configure MOTD and bash settings
RUN echo "███████╗██╗   ██╗███╗   ██╗███████╗██╗  ██╗██╗███╗   ██╗███████╗ ██████╗██╗      ██████╗ ██╗   ██╗██████╗ " > /etc/motd && \
    echo "██╔════╝██║   ██║████╗  ██║██╔════╝██║  ██║██║████╗  ██║██╔════╝██╔════╝██║     ██╔═══██╗██║   ██║██╔══██╗" >> /etc/motd && \
    echo "███████╗██║   ██║██╔██╗ ██║███████╗███████║██║██╔██╗ ██║█████╗  ██║     ██║     ██║   ██║██║   ██║██║  ██║" >> /etc/motd && \
    echo "╚════██║██║   ██║██║╚██╗██║╚════██║██╔══██║██║██║╚██╗██║██╔══╝  ██║     ██║     ██║   ██║██║   ██║██║  ██║" >> /etc/motd && \
    echo "███████║╚██████╔╝██║ ╚████║███████║██║  ██║██║██║ ╚████║███████╗╚██████╗███████╗╚██████╔╝╚██████╔╝██████╔╝" >> /etc/motd && \
    echo "╚══════╝ ╚═════╝ ╚═╝  ╚═══╝╚══════╝╚═╝  ╚═╝╚═╝╚═╝  ╚═══╝╚══════╝ ╚═════╝╚══════╝ ╚═════╝  ╚═════╝ ╚═════╝ " >> /etc/motd && \
    echo "                                                                                                          " >> /etc/motd && \
    echo "if [ -t 1 ]; then" >> /etc/bash.bashrc && \
    echo "  if command -v run-parts >/dev/null 2>&1 && [ -d /etc/update-motd.d ]; then" >> /etc/bash.bashrc && \
    echo "    run-parts /etc/update-motd.d > /tmp/_motd" >> /etc/bash.bashrc && \
    echo "    cat /etc/motd" >> /etc/bash.bashrc && \
    echo "    cat /tmp/_motd" >> /etc/bash.bashrc && \
    echo "    rm -f /tmp/_motd" >> /etc/bash.bashrc && \
    echo "  else" >> /etc/bash.bashrc && \
    echo "    cat /etc/motd" >> /etc/bash.bashrc && \
    echo "  fi" >> /etc/bash.bashrc && \
    echo "fi" >> /etc/bash.bashrc && \
    # Create welcome message for MOTD
    echo '#!/bin/sh' > /etc/update-motd.d/10-uname && \
    echo 'echo "👋 Welcome to SunshineCloud Codespaces! You are on our default image."' >> /etc/update-motd.d/10-uname && \
    echo 'echo "   - It includes runtimes and tools for Python, Node.js, Docker, and more. See the full list here: https://aka.ms/ghcs-default-image"' >> /etc/update-motd.d/10-uname && \
    echo 'echo "   - Want to use a custom image instead? Learn more here: https://aka.ms/configure-codespace"' >> /etc/update-motd.d/10-uname && \
    echo 'echo ""' >> /etc/update-motd.d/10-uname && \
    echo 'echo "🔍 To explore VS Code to its fullest, search using the Command Palette (Cmd/Ctrl + Shift + P or F1)."' >> /etc/update-motd.d/10-uname && \
    echo 'echo ""' >> /etc/update-motd.d/10-uname && \
    echo 'echo "📝 Edit away, run your app as usual, and we'"'"'ll automatically make it available for you to access."' >> /etc/update-motd.d/10-uname && \
    echo 'echo ""' >> /etc/update-motd.d/10-uname && \
    chmod +x /etc/update-motd.d/10-uname

WORKDIR /SunshineCloud/
USER matrix0523

# Complete basic setup

WORKDIR /SunshineCloud/
USER matrix0523    
    # Clone essential repositories
RUN sudo git clone --recursive https://github.com/SunshineCloudTech/SunshineCloud-Universal-Utils.git /SunshineCloud/SunshineCloud-Universal-Utils

USER root
# Configure SunshineCloud directories
RUN chown -R matrix0523:matrix0523 /SunshineCloud

# Copy system files and configure permissions
ADD environments/environment.yml /tmp/micromamba/environment.yml
# ADD system/bin /usr/bin
ADD config/*.conf /etc/supervisor/conf.d/
RUN mkdir -vp /var/run/dbus
# Set final permissions
RUN chown -R matrix0523:matrix0523 /home/matrix0523/.config /SunshineCloud && \
    chmod 755 /home/matrix0523/.config

WORKDIR /tmp

USER matrix0523
WORKDIR /home/matrix0523
# Copy and setup the entrypoint script
COPY scripts/docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh
USER root
RUN chmod +x /usr/local/bin/docker-entrypoint.sh
USER matrix0523
WORKDIR /home/matrix0523
# 暴露 MySQL 和 Redis 端口
EXPOSE 3306 6379 3389 22
EXPOSE 3306/udp 6379/udp 3389/udp 22/udp

ENTRYPOINT ["/usr/bin/bash", "-c"]
CMD ["/usr/local/bin/docker-entrypoint.sh"]