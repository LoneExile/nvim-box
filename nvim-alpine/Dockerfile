FROM node:16-alpine

# TODO: change path python dinamically (use which)
# make nvim config git URL configurable (use ARG/ENV)

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV TZ=Asia/Bangkok

# NOTE: nvm kinda works with alpine, but not really. took forever to build and it's not really worth it
# https://github.com/nvm-sh/nvm#installing-nvm-on-alpine-linux

# Update package manager and install dependencies
RUN apk add -U --no-cache curl bash ca-certificates openssl ncurses coreutils \
  python3 make gcc g++ libgcc linux-headers grep util-linux binutils findutils git \
  build-base icu-dev btop nnn luarocks go lazygit wget gzip xclip ripgrep\
  && npm i -g neovim pnpm\
  && pip3 install --upgrade pip \
  && pip3 install pynvim

# Set ZSH as default shell
RUN apk add -U --no-cache zsh shadow \
  && echo "auth       sufficient   pam_shells.so" >> /etc/pam.d/chsh \
  && chsh -s /bin/zsh

# Install Rust
RUN curl https://sh.rustup.rs -sSf | bash -s -- -y

# Install dotnet
RUN curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin -Channel 6.0 -Version latest -InstallDir /usr/share/dotnet \
  && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet

# install neovim
# https://github.com/neovim/neovim/wiki/Building-Neovim#alpine-linux
RUN apk add -U --no-cache build-base cmake automake autoconf libtool pkgconf coreutils curl unzip gettext-tiny-dev
RUN git clone --depth 1 https://github.com/neovim/neovim /tmp/neovim \
  && cd /tmp/neovim \
  && make CMAKE_BUILD_TYPE=Release \
  && make install \
  && cd / \
  && rm -rf /tmp/neovim

RUN git clone --depth 1 https://github.com/LoneExile/nvim.git $HOME/.config/nvim

# keep the container running, but it will not do anything else
CMD ["tail", "-f", "/dev/null"]
