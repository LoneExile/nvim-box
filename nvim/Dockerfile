FROM debian:latest

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV TZ=Asia/Bangkok

# Update package manager and install dependencies
RUN apt-get update && apt-get upgrade -y
RUN apt update && apt install -y git build-essential 
RUN apt update && apt install -y golang perl ruby luarocks tklib bat wget tree fd-find jq fzf 
RUN apt update && apt install -y xclip tmux zip nnn xsel vim curl ca-certificates
RUN apt update && apt install -y libssl-dev zlib1g-dev llvm libbz2-dev libreadline-dev libsqlite3-dev libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

# Install NVM
ENV NVM_DIR /root/.nvm
ENV NODE_VERSION 16
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash \
    && . $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default \
    && npm install -g neovim pnpm

# Install Pyenv
ENV PYTHON_VERSION=3.10.5
ENV PYENV_ROOT="${HOME}/.pyenv"
ENV PATH="${PYENV_ROOT}/shims:${PYENV_ROOT}/bin:${PATH}"
RUN curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash \
    && pyenv install $PYTHON_VERSION \
    && pyenv global $PYTHON_VERSION \
    && pyenv rehash \
    && pip install pynvim

# Install Rust
RUN curl https://sh.rustup.rs -sSf | bash -s -- -y

# Install neovim
RUN apt-get install -y ninja-build gettext libtool libtool-bin autoconf automake cmake g++ pkg-config unzip curl doxygen
RUN git clone https://github.com/neovim/neovim /tmp/neovim \
    && cd /tmp/neovim \
    && make CMAKE_BUILD_TYPE=Release \
    && make install \
    && cd / \
    && rm -rf /tmp/neovim

CMD ["tail", "-f", "/dev/null"]
