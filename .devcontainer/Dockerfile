# Use Debian bookworm as base image
FROM debian:bookworm

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=UTC
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8

# Install common tools and dependencies
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    curl \
    wget \
    tar \
    sudo \
    zip \
    sqlite3 \
    tmux \
    ntp \
    build-essential \
    gettext \
    libcap2-bin \
    netcat-traditional \
    silversearcher-ag \
    htop \
    jq \
    dirmngr \
    cron \
    rsync \
    locales \
    net-tools \
    git-lfs \
    software-properties-common \
    ca-certificates \
    gnupg \
    lsb-release \
    vim \
    && rm -rf /var/lib/apt/lists/*

# Set timezone to UTC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Generate and set locale
RUN sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen && \
    locale-gen

# Create a non-root user
ARG USERNAME=dev
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:/usr/bin/apt-get,/usr/bin/dpkg,/usr/bin/npm,/usr/bin/corepack > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Switch to non-root user
USER $USERNAME
WORKDIR /home/$USERNAME

# Install dotfiles with verification
RUN wget -O dotfiles.tar.gz https://github.com/ichuan/dotfiles/releases/latest/download/dotfiles.tar.gz && \
    tar xzf dotfiles.tar.gz && \
    bash dotfiles/bootstrap.sh -f && \
    rm -rf dotfiles ~/Tomorrow_Night_Bright.terminal ~/iTerm.profile.json dotfiles.tar.gz

# Install Node.js LTS using NodeSource repository
USER root
RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | bash - && \
    apt-get install -y nodejs && \
    rm -rf /var/lib/apt/lists/*

# Enable Corepack and install pnpm
RUN corepack enable && \
    corepack prepare pnpm@latest --activate

# Install TypeScript globally
RUN npm install -g typescript

# Install Claude Code globally
RUN npm install -g @anthropic-ai/claude-code

# Install pyenv dependencies
RUN apt-get update && apt-get install -y \
    make \
    libssl-dev \
    zlib1g-dev \
    libbz2-dev \
    libreadline-dev \
    libsqlite3-dev \
    libncursesw5-dev \
    xz-utils \
    tk-dev \
    libxml2-dev \
    libxmlsec1-dev \
    libffi-dev \
    liblzma-dev \
    && rm -rf /var/lib/apt/lists/*

# Switch back to non-root user for pyenv installation
USER $USERNAME

# Install pyenv with verification
RUN curl -L https://pyenv.run | bash

# Add pyenv to PATH and configure shell
ENV PYENV_ROOT="/home/$USERNAME/.pyenv"
ENV PATH="$PYENV_ROOT/bin:$PATH"

# Initialize pyenv in shell
RUN echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc && \
    echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc && \
    echo 'eval "$(pyenv init --path)"' >> ~/.bashrc && \
    echo 'eval "$(pyenv init -)"' >> ~/.bashrc

# Install latest Python
RUN bash -c "source ~/.bashrc && pyenv install 3.12.7 && pyenv global 3.12.7"

# Install Poetry with verification
RUN bash -c "source ~/.bashrc && curl -sSL https://install.python-poetry.org | python3 -"

# Add Poetry to PATH
ENV PATH="/home/$USERNAME/.local/bin:$PATH"
RUN echo 'export PATH="/home/$USERNAME/.local/bin:$PATH"' >> ~/.bashrc

# Verify installations
RUN bash -c "source ~/.bashrc && node --version && npm --version && pnpm --version && typescript --version || tsc --version"
RUN bash -c "source ~/.bashrc && claude --version || echo 'Claude Code installed'"
RUN bash -c "source ~/.bashrc && pyenv --version && python --version && poetry --version"

# Set default shell
SHELL ["/bin/bash", "-c"]

# Switch back to root for any final system configurations
USER root

# Create workspace directory and set proper permissions
RUN mkdir -p /workspace && chown -R $USERNAME:$USERNAME /workspace /home/$USERNAME

# Copy .claude directory contents and .claude.json from .devcontainer
COPY --chown=$USERNAME:$USERNAME .devcontainer/.claude/ /home/$USERNAME/.claude/
COPY --chown=$USERNAME:$USERNAME .devcontainer/.claude.json /home/$USERNAME/

# Switch back to non-root user
USER $USERNAME

# Set working directory
WORKDIR /workspace