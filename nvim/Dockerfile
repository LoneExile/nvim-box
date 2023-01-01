FROM debian:latest

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV TZ=Asia/Bangkok

# Update package manager and install dependencies
RUN apt-get update && apt-get upgrade -y
RUN apt update && apt install -y git build-essential 
RUN apt update && apt install -y golang perl ruby luarocks tklib bat wget tree fd-find jq fzf 
RUN apt update && apt install -y xclip tmux zip nnn xsel vim curl ca-certificates coreutils sed

RUN apt install -y zsh \
  && chsh -s /bin/zsh \
  && echo 'export NVM_DIR="$HOME/.nvm"' | tee -a ~/.bashrc ~/.profile ~/.zshrc \
  && echo '[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"' | tee -a ~/.bashrc ~/.profile ~/.zshrc \
  && echo '[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"' | tee -a ~/.bashrc ~/.profile ~/.zshrc \
  && echo 'export PNPM_HOME="$HOME/.local/share/pnpm"' | tee -a ~/.bashrc ~/.profile ~/.zshrc \
  && echo 'export PATH="$PNPM_HOME:$PATH"' | tee -a ~/.bashrc ~/.profile ~/.zshrc \
  && echo 'export PYENV_ROOT="$HOME/.pyenv"' | tee -a ~/.bashrc ~/.profile ~/.zshrc \
  && echo 'export PATH=$PYENV_ROOT/shims:$PATH' | tee -a ~/.bashrc ~/.profile ~/.zshrc \
  && zsh

# Install NVM
RUN export NVM_DIR="$HOME/.nvm" \
  && git clone https://github.com/nvm-sh/nvm.git "$NVM_DIR" \
  && cd "$NVM_DIR" \
  && git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" $(git rev-list --tags --max-count=1)` \
  && \. "$NVM_DIR/nvm.sh" \
  && nvm install $(nvm ls-remote | grep 'v16.' | tail -1 | awk '{print $1}' | sed 's/^v//') \
  && npm install -g neovim pnpm

# Install Pyenv
ENV PYTHON_VERSION=3.10.5
ENV PYENV_ROOT="${HOME}/.pyenv"
ENV PATH="${PYENV_ROOT}/shims:${PYENV_ROOT}/bin:${PATH}"
RUN apt update && apt install -y libssl-dev zlib1g-dev llvm libbz2-dev libreadline-dev libsqlite3-dev libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
RUN curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash \
  && pyenv install $PYTHON_VERSION \
  && pyenv global $PYTHON_VERSION \
  && pyenv rehash \
  && pip install pynvim

# Install Rust
RUN curl https://sh.rustup.rs -sSf | bash -s -- -y

# Install neovim
RUN apt-get install -y ninja-build gettext libtool libtool-bin autoconf automake cmake g++ pkg-config unzip curl doxygen
RUN git clone --depth 1 https://github.com/neovim/neovim /tmp/neovim \
  && cd /tmp/neovim \
  && make CMAKE_BUILD_TYPE=Release \
  && make install \
  && cd / \
  && rm -rf /tmp/neovim

RUN LAZYGIT_VERSION=$(curl -s "https://api.github.com/repos/jesseduffield/lazygit/releases/latest" | grep '"tag_name":' |  sed -E 's/.*"v*([^"]+)".*/\1/') \
  && curl -Lo lazygit.tar.gz "https://github.com/jesseduffield/lazygit/releases/latest/download/lazygit_${LAZYGIT_VERSION}_Linux_x86_64.tar.gz" \
  && tar xf lazygit.tar.gz -C /usr/local/bin lazygit \
  && rm lazygit.tar.gz

RUN git clone --depth 1 https://github.com/aristocratos/btop.git \
  && cd btop \
  && make \
  && make install \
  && make setuid \
  && cd .. \
  && rm -rf btop

# Install .NET
RUN apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
  libc6 libgcc1 libgssapi-krb5-2 libssl1.1 libstdc++6 zlib1g \
  && rm -rf /var/lib/apt/lists/*
RUN curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin -Channel 6.0 -Version latest -InstallDir /usr/share/dotnet \
  && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet

RUN git clone --depth 1 https://github.com/LoneExile/nvim.git $HOME/.config/nvim
# RUN nvim --headless "+Lazy! sync" +qa

CMD ["tail", "-f", "/dev/null"]
