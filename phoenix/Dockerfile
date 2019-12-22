# docker file for Phoenix elixir web framework
FROM elixir:alpine

WORKDIR /app

# install hex and phoenix
RUN yes | mix local.hex
RUN yes | mix archive.install https://github.com/phoenixframework/archives/raw/master/phx_new.ez
RUN mix local.rebar --force

# elixirがinstallされているかの確認用(省略可)
RUN echo "install elixir version is " && elixir -v