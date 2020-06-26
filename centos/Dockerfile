FROM centos:7.7.1908
MAINTAINER Jason Harrelson <jason@lookforwardenterprises.com>

RUN yum clean all && yum -y install epel-release && yum -y update
RUN yum -y group install "Development Tools"
RUN yum -y install glibc-devel make ncurses-devel openssl-devel autoconf git wget #wxBase.x86_64 java-1.8.0-openjdk-devel

WORKDIR /tmp/erlang-build

ARG ERLANG_VERSION

ENV REFRESHED_AT=2019-07-17 \
    LANG=en_US.UTF-8 \
    HOME=/opt/app/ \
    # Set this so that CTRL+G works properly
    TERM=xterm

RUN \
    # Shallow clone Erlang/OTP
    git clone -b OTP-$ERLANG_VERSION --single-branch --depth 1 https://github.com/erlang/otp.git . && \
    # Erlang/OTP build env
    export ERL_TOP=/tmp/erlang-build && \
    export PATH=$ERL_TOP/bin:$PATH && \
    export CPPFlAGS="-D_BSD_SOURCE $CPPFLAGS" && \
    # Configure
    ./otp_build autoconf && \
    ./configure --prefix=/usr \
      --build="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)" \
      --sysconfdir=/etc \
      --mandir=/usr/share/man \
      --infodir=/usr/share/info \
      --without-javac \
      --without-wx \
      --without-debugger \
      --without-observer \
      --without-jinterface \
      --without-cosEvent\
      --without-cosEventDomain \
      --without-cosFileTransfer \
      --without-cosNotification \
      --without-cosProperty \
      --without-cosTime \
      --without-cosTransactions \
      --without-et \
      --without-gs \
      --without-ic \
      --without-megaco \
      --without-orber \
      --without-percept \
      --without-typer \
      --enable-threads \
      --enable-shared-zlib \
      --enable-ssl=dynamic-ssl-lib \
      --enable-hipe && \
    # Build
    make -j4 && make install
    # Cleanup

RUN rm -rf /tmp/erlang-build

ARG ELIXIR_VERSION

# Elixir
RUN curl -sSL https://github.com/elixir-lang/elixir/releases/download/v${ELIXIR_VERSION}/Precompiled.zip -o Precompiled.zip && \
    ls -al && \
    mkdir -p /opt/elixir-${ELIXIR_VERSION} && \
    unzip Precompiled.zip -d /opt/elixir-${ELIXIR_VERSION} && \
    rm Precompiled.zip

ENV PATH $PATH:/opt/elixir-${ELIXIR_VERSION}/bin

RUN mix local.hex --force && \
    mix local.rebar --force

WORKDIR ${HOME}

CMD ["/bin/sh"]
