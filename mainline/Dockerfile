FROM debian:jessie
MAINTAINER Matthew Vance

# Set versions of each component to build from source
ENV VERSION_PCRE pcre-8.40
ENV VERSION_ZLIB zlib-1.2.11
ENV VERSION_OPENSSL openssl-1.1.0c
ENV VERSION_NGINX nginx-1.11.13

# Set expected checksums versions
ENV SHA256_PCRE=1d75ce90ea3f81ee080cdc04e68c9c25a9fb984861a0618be7bbf676b18eda3e
ENV SHA256_ZLIB=c3e5e9fdd5004dcb542feda5ee4f0ff0744628baf8ed2dd5d66f8ca1197cb1a1
ENV SHA256_OPENSSL=fc436441a2e05752d31b4e46115eb89709a28aef96d4fe786abe92409b2fd6f5
ENV SHA256_NGINX=360b601ef8ed2998c80fa56a27bf3cd745c3ce18c5fb10892e6595467b1415d3

# Set GPG keys used to sign downloads
ENV GPG_OPENSSL=8657ABB260F056B1E5190839D9C4D26D0E604491
ENV GPG_NGINX=B0F4253373F8F6F510D42178520A9993A1C052F8

# Set URLs to the source directories
ENV SOURCE_DYNAMIC_TLS_RECORDS=https://raw.githubusercontent.com/cloudflare/sslconfig/master/patches/nginx__1.11.5_dynamic_tls_records.patch
ENV SOURCE_PCRE=ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/
ENV SOURCE_ZLIB=http://zlib.net/
ENV SOURCE_OPENSSL=https://www.openssl.org/source/
ENV SOURCE_NGINX=https://nginx.org/download/

# Set where OpenSSL and NGINX will be built
ENV BPATH /usr/local/src/nginx

# Set variable so ngx_brotli static module can be compiled into nginx by itself
ENV NGX_BROTLI_STATIC_MODULE_ONLY=1

RUN \
    BUILD_DEPS='build-essential ca-certificates curl git libssl-dev' && \
    set -x && \
    DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y --no-install-recommends \
      $BUILD_DEPS && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir $BPATH && \
    cd $BPATH && \
    curl -fSL $SOURCE_PCRE$VERSION_PCRE.tar.gz -o pcre.tar.gz && \
    echo "${SHA256_PCRE} $BPATH/pcre.tar.gz" | sha256sum -c - && \
    tar xzf pcre.tar.gz && \
    curl -fSL $SOURCE_ZLIB$VERSION_ZLIB.tar.gz -o zlib.tar.gz && \
    echo "${SHA256_ZLIB} $BPATH/zlib.tar.gz" | sha256sum -c - && \
    tar xzf zlib.tar.gz && \
    curl -fSL $SOURCE_OPENSSL$VERSION_OPENSSL.tar.gz -o openssl.tar.gz && \
    echo "${SHA256_OPENSSL} $BPATH/openssl.tar.gz" | sha256sum -c - && \
    curl -fSL $SOURCE_OPENSSL$VERSION_OPENSSL.tar.gz.asc -o openssl.tar.gz.asc && \
    export GNUPGHOME="$(mktemp -d)" && \
    gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$GPG_OPENSSL" && \
    gpg --batch --verify openssl.tar.gz.asc openssl.tar.gz && \
    tar xzf openssl.tar.gz && \
    git clone https://github.com/google/ngx_brotli --depth=1 && \
    cd ngx_brotli && \
    git submodule update --init && \
    cd .. && \
    addgroup --system nginx && \
    adduser --disabled-password --system --home /var/cache/nginx --shell /sbin/nologin --group nginx && \
    curl -fSL $SOURCE_NGINX$VERSION_NGINX.tar.gz -o nginx.tar.gz && \
    echo "${SHA256_NGINX} $BPATH/nginx.tar.gz" | sha256sum -c - && \
    curl -fSL $SOURCE_NGINX$VERSION_NGINX.tar.gz.asc -o nginx.tar.gz.asc && \
    export GNUPGHOME="$(mktemp -d)" && \
    gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$GPG_NGINX" && \
    gpg --batch --verify nginx.tar.gz.asc nginx.tar.gz && \
    tar xzf nginx.tar.gz && \
    cd $BPATH/$VERSION_NGINX && \
    curl -fSL $SOURCE_DYNAMIC_TLS_RECORDS -o nginx__dynamic_tls_records.patch && \
    patch -p1 < nginx__dynamic_tls_records.patch && \
    ./configure \
    --prefix=/etc/nginx \
    --sbin-path=/usr/sbin/nginx \
    --with-cc-opt='-O3 -fPIE -fstack-protector-strong -Wformat -Werror=format-security' \
    --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro' \
    --with-pcre=$BPATH/$VERSION_PCRE \
    --with-zlib=$BPATH/$VERSION_ZLIB \
    --with-openssl-opt="no-weak-ssl-ciphers no-ssl3 no-shared enable-ec_nistp_64_gcc_128 -DOPENSSL_NO_HEARTBEATS -fstack-protector-strong" \
    --with-openssl=$BPATH/$VERSION_OPENSSL \
    --modules-path=/usr/lib/nginx/modules \
    --conf-path=/etc/nginx/nginx.conf \
    --error-log-path=/var/log/nginx/error.log \
    --http-log-path=/var/log/nginx/access.log \
    --pid-path=/var/run/nginx.pid \
    --lock-path=/var/run/nginx.lock \
    --http-client-body-temp-path=/var/cache/nginx/client_temp \
    --http-proxy-temp-path=/var/cache/nginx/proxy_temp \
    --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \
    --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
    --http-scgi-temp-path=/var/cache/nginx/scgi_temp \
    --user=nginx \
    --group=nginx \
    --with-file-aio \
    --with-http_auth_request_module \
    --with-http_gunzip_module \
    --with-http_gzip_static_module \
    --with-http_mp4_module \
    --with-http_realip_module \
    --with-http_secure_link_module \
    --with-http_slice_module \
    --with-http_ssl_module \
    --with-http_stub_status_module \
    --with-http_sub_module \
    --with-http_v2_module \
    --with-ipv6 \
    --with-pcre-jit \
    --with-stream \
    --with-stream_ssl_module \
    --with-threads \
    --without-http_empty_gif_module \
    --without-http_geo_module \
    --without-http_split_clients_module \
    --without-http_ssi_module \
    --without-mail_imap_module \
    --without-mail_pop3_module \
    --without-mail_smtp_module \
    --add-module=$BPATH/ngx_brotli && \
    make && \
    make install && \
    rm -rf /etc/nginx/html/ && \
    mkdir /etc/nginx/conf.d/ && \
    mkdir -p /etc/nginx/sites-available/ && \
    mkdir -p /etc/nginx/sites-enabled/ && \
    mkdir -p /usr/share/nginx/html/ && \
    install -m644 html/index.html /usr/share/nginx/html/ && \
    install -m644 html/50x.html /usr/share/nginx/html/ && \
    strip /usr/sbin/nginx* && \
    apt-get purge -y --auto-remove $BUILD_DEPS && \
    rm -fr /tmp/* /var/tmp/* "$BPATH" "$GNUPGHOME" && \
    \
    # Forward request and error logs to docker log collector
    ln -sf /dev/stdout /var/log/nginx/access.log && \
    ln -sf /dev/stderr /var/log/nginx/error.log

COPY nginx.conf /etc/nginx/nginx.conf
COPY nginx.vh.default.conf /etc/nginx/sites-available/default.conf

RUN ln -s /etc/nginx/sites-available/default.conf /etc/nginx/sites-enabled/default.conf

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
