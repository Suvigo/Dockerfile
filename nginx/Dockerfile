FROM centos:7
WORKDIR /opt
ARG Version=1.5.9
ENV Version=${Version} \
    LANG=en_US.utf8

RUN set -ex \
    && yum install -y epel-release wget gmake\
    && yum -y install gcc gcc-c++ make curl libtool zlib zlib-devel openssl openssl-devel pcre pcre-devel \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "LANG=en_US.utf8" > /etc/locale.conf \
    && echo -e "[nginx-stable]\nname=nginx stable repo\nbaseurl=http://nginx.org/packages/centos/\$releasever/\$basearch/\ngpgcheck=1\nenabled=1\ngpgkey=https://nginx.org/keys/nginx_signing.key\nmodule_hotfixes=true" > /etc/yum.repos.d/nginx.repo \
    && yum install -y nginx \
    && echo > /etc/nginx/conf.d/default.conf \
    && wget https://github.com/jumpserver/luna/releases/download/${Version}/luna.tar.gz \
    && wget https://github.com/openresty/openresty/releases/download/v1.15.8.3/openresty-1.15.8.3.tar.gz \
    && wget http://nginx.org/download/nginx-1.18.0.tar.gz \
    && tar -xf openresty-1.15.8.3.tar.gz \
    && cd openresty-1.15.8.3 \
    && ./configure --prefix=/opt/openresty \
            --with-luajit \
            --without-http_redis2_module \
            --with-http_iconv_module \
    && gmake -j2\
    && gmake install \
    && cd bundle/LuaJIT-2.1-20190507/ \
    && make \
    && make install \
    && cd /opt \
    && tar -xf nginx-1.18.0.tar.gz \
    && cd nginx-1.18.0 \
    && export LUAJIT_LIB=/opt/openresty/lualib/ \
    && export LUAJIT_INC=/opt/openresty/luajit/include/luajit-2.1/ \
    && ./configure --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module  --add-module=/opt/openresty-1.15.8.3/bundle/ngx_devel_kit-0.3.1rc1 --add-module=/opt/openresty-1.15.8.3/bundle/echo-nginx-module-0.61 --add-module=/opt/openresty-1.15.8.3/bundle/xss-nginx-module-0.06 --add-module=/opt/openresty-1.15.8.3/bundle/ngx_coolkit-0.2 --add-module=/opt/openresty-1.15.8.3/bundle/set-misc-nginx-module-0.32 --add-module=/opt/openresty-1.15.8.3/bundle/form-input-nginx-module-0.12 --add-module=/opt/openresty-1.15.8.3/bundle/encrypted-session-nginx-module-0.08 --add-module=/opt/openresty-1.15.8.3/bundle/srcache-nginx-module-0.31 --add-module=/opt/openresty-1.15.8.3/bundle/ngx_lua-0.10.15 --add-module=/opt/openresty-1.15.8.3/bundle/ngx_lua_upstream-0.07 --add-module=/opt/openresty-1.15.8.3/bundle/headers-more-nginx-module-0.33 --add-module=/opt/openresty-1.15.8.3/bundle/array-var-nginx-module-0.05 --add-module=/opt/openresty-1.15.8.3/bundle/memc-nginx-module-0.19 --add-module=/opt/openresty-1.15.8.3/bundle/redis2-nginx-module-0.15 --add-module=/opt/openresty-1.15.8.3/bundle/redis-nginx-module-0.3.7 --add-module=/opt/openresty-1.15.8.3/bundle/rds-json-nginx-module-0.15 --add-module=/opt/openresty-1.15.8.3/bundle/rds-csv-nginx-module-0.09 --add-module=/opt/openresty-1.15.8.3/bundle/ngx_stream_lua-0.0.7 --with-ld-opt=-Wl,-rpath,/usr/local/lib/ --with-stream --with-stream_ssl_module --with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -fPIC' --with-ld-opt='-Wl,-rpath,$LUAJIT_LIB,-z,relro -Wl,-z,now -pie' \
    && make \
    && cd objs \
    && cp nginx /usr/sbin/ \
    && ln -s /usr/local/lib/libluajit-5.1.so.2 /lib64/libluajit-5.1.so.2 \
    && cd /opt \
    && tar -xf luna.tar.gz \
    && chown -R root:root luna \
    && rm -rf luna.tar.gz \
    && yum remove -y wget \
    && yum clean all \
    && rm -rf /var/tmp/yum*

COPY nginx/nginx.conf /etc/nginx/

CMD ["nginx", "-g", "daemon off;"]
