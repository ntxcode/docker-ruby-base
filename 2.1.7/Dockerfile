FROM ntxcode/ubuntu-base:14.04
MAINTAINER Nathan Ribeiro, ntxdev <nathan@ntxdev.com.br>

ENV RUBY_MAJOR 2.1
ENV RUBY_VERSION 2.1.7
ENV RUBY_DOWNLOAD_SHA256 f59c1596ac39cc7e60126e7d3698c19f482f04060674fdfe0124e1752ba6dd81

RUN apt-get update \
    && apt-get install -y bison libgdbm-dev ruby libgeos-dev ruby-geos git-core nodejs \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir -p /usr/src/ruby \
    && curl -fSL -o ruby.tar.gz "http://cache.ruby-lang.org/pub/ruby/$RUBY_MAJOR/ruby-$RUBY_VERSION.tar.gz" \
    && echo "$RUBY_DOWNLOAD_SHA256 *ruby.tar.gz" | sha256sum -c - \
    && tar -xzf ruby.tar.gz -C /usr/src/ruby --strip-components=1 \
    && rm ruby.tar.gz \
    && cd /usr/src/ruby \
    && autoconf \
    && ./configure --disable-install-doc \
    && make -j"$(nproc)" \
    && make install \
    && apt-get purge -y --auto-remove bison libgdbm-dev ruby \
    && rm -r /usr/src/ruby \

    # Setup Rubygems
    && cd /tmp \
    && echo 'gem: --no-document' > /etc/gemrc \
    && gem install bundler && gem update --system

RUN rm -rf /var/lib/apt/lists/* && \
    truncate -s 0 /var/log/*log && \
    cd /usr/share && rm -fr doc/* man/* info/* lintian/* && \
    mkdir man/man1

RUN cd /usr/lib && ln -s libgeos-3.4.2.so libgeos.so

CMD ["/usr/bin/ruby"]
