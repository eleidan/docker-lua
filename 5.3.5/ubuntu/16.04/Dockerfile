FROM eleidan/base:ubuntu-16.04

LABEL maintainer="Oleg Kulyk" \
      description="Lua 5.3.5 based on Ubuntu 16.04" \
      init="docker run -it --rm --user $(id -u) -v $(pwd):/home/phantom/app eleidan/lua:5.3.5-ubuntu-16.04 docker-init.sh" \
      run="docker run -it --rm --user $(id -u) -v $(pwd):/home/phantom/app eleidan/lua:5.3.5-ubuntu-16.04" \
      version="2019-02-18"

WORKDIR $HOME/app

ENV SERVICE_NAME="Lua 5.3.5 on Ubuntu 16.04"
ENV USER=phantom

### Needed for the docker-init.sh script
COPY --chown=phantom:phantom recipes $HOME/recipes

USER root
### Install Lua
ARG LUA_VERSION="5.3.5"
ARG LUA_PACKAGE="lua-${LUA_VERSION}.tar.gz"
RUN curl -R -O http://www.lua.org/ftp/${LUA_PACKAGE} \
    && tar zxf ${LUA_PACKAGE} \
    && cd lua-${LUA_VERSION} \
    && make \
    && make linux install \
    && cd - \
    && rm -rf *

# Install LuaRocks
# RUN mkdir -p ${HOME}/.cache/luarocks && chown -R phantom:phantom ${HOME}
ARG LUA_ROCKS_VERSION="3.0.4"
RUN wget https://luarocks.org/releases/luarocks-${LUA_ROCKS_VERSION}.tar.gz \
    && tar zxpf luarocks-${LUA_ROCKS_VERSION}.tar.gz \
    && cd luarocks-${LUA_ROCKS_VERSION} \
    && ./configure \
    && make install \
    && rm -rf *

ENV PATH=${PATH}:${HOME}/app/lua_modules/bin

RUN chown -R phantom:phantom ${HOME}

USER phantom
