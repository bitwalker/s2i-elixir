FROM bitwalker/s2i-alpine-base:1.0

MAINTAINER Paul Schoenfelder <paulschoenfelder@gmail.com>

EXPOSE 5000

ENV \
  PORT=5000 \
  MIX_ENV=prod \
  NPM_VERSION=3 \
  NODE_ENV=production \
  NPM_CONFIG_LOGLEVEL=info \
  CONFIG_FLAGS="--fully-static --without-npm" \
  DEL_PKGS="libgcc libstdc++" \
  RM_DIRS=/usr/include \
  REFRESHED_AT=2016-04-8

LABEL \
  io.k8s.description="Platform for building and running Elixir applications" \
  io.k8s.display-name="Elixir v1.2.4, Node.js v4.4.2" \
  io.openshift.expose-services="5000:http" \
  io.openshift.tags="builder,elixir,elixir-$ELIXIR_VERSION" \
  com.redhat.deployments-dir="$HOME"

RUN apk add --no-cache --update \
  ncurses-libs erlang erlang-dev erlang-kernel erlang-hipe erlang-compiler \
  erlang-stdlib erlang-erts erlang-tools erlang-syntax-tools erlang-sasl \
  erlang-crypto erlang-public-key erlang-ssl erlang-ssh erlang-asn1 erlang-inets \
  erlang-mnesia erlang-odbc erlang-xmerl erlang-erl-interface erlang-parsetools erlang-eunit \
  nodejs=4.4.2-r0 elixir=1.2.4-r0

ENV PATH=${HOME}/node_modules/.bin:/opt/elixir-${ELIXIR_VERSION}/bin:$PATH

RUN mix local.hex --force && \
    mix local.rebar --force

# Copy the S2I scripts
COPY ./bin/ $STI_SCRIPTS_PATH

# Drop the root user and make the content of $HOME owned by user 1001
RUN chown -R 1001:0 /opt/app-root $STI_SCRIPTS_PATH
USER 1001

# Set the default CMD to print the usage for this iamge
CMD $STI_SCRIPTS_PATH/usage
