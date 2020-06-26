FROM openjdk:8-jdk

MAINTAINER Data-drone

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y gcc g++ make zip && \
    apt-get clean

ENV THRIFT_VERSION=0.13.0 ANT_VERSION=1.10.8

ADD http://apache.mirrors.pair.com//ant/binaries/apache-ant-${ANT_VERSION}-bin.tar.gz /tmp/ant.tgz
RUN tar -C /opt -xf /tmp/ant.tgz && rm /tmp/ant.tgz && \
    ln -s /opt/apache-ant-${ANT_VERSION}/bin/* /usr/bin/ && /usr/bin/ant -version

ADD http://archive.apache.org/dist/thrift/${THRIFT_VERSION}/thrift-${THRIFT_VERSION}.tar.gz /tmp/thrift.tgz
RUN tar -C /tmp -xf /tmp/thrift.tgz && rm /tmp/thrift.tgz

WORKDIR /tmp/thrift-${THRIFT_VERSION}

RUN apt-get install -y libtool pkgconf && \
    ./bootstrap.sh

RUN ./configure \
    --with-java \
    --without-cpp \
    --without-qt4 \
    --without-qt5 \
    --without-c_glib \
    --without-csharp \
    --without-erlang \
    --without-nodejs \
    --without-lua \
    --with-python \
    --without-perl \
    --without-php \
    --without-php_extension \
    --without-ruby \
    --without-haskell \
    --without-go \
    --without-haxe \
    --without-d \
    && make

RUN make install

WORKDIR /home

RUN rm -rf /tmp/thrift-${THRIFT_VERSION} && apt-get remove -y gcc g++ make

ENTRYPOINT ["/usr/local/bin/thrift"]