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
RUN mkdir -p ${HOME}/.cache/luarocks && chown -R phantom:phantom ${HOME}
RUN wget https://luarocks.org/releases/luarocks-3.0.4.tar.gz \
    && tar zxpf luarocks-3.0.4.tar.gz \
    && cd luarocks-3.0.4 \
    && ./configure \
    && make install \
    && rm -rf *

RUN chown -R phantom:phantom ${HOME}

USER phantom
