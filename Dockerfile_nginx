FROM ubuntu:jammy

WORKDIR /usr/src/
RUN apt update && apt install -y git curl perl build-essential zlib1g-dev libpcre3-dev libssl-dev && mkdir /conf
RUN curl -O https://openresty.org/download/openresty-1.21.4.1.tar.gz && git clone https://github.com/vozlt/nginx-module-vts.git && git clone https://github.com/yaoweibin/nginx_upstream_check_module.git && tar -zxf openresty-1.21.4.1.tar.gz
RUN cd /usr/src/openresty-1.21.4.1 && patch -p1 -d bundle/nginx-1.21.4 < /usr/src/nginx_upstream_check_module/check_1.20.1+.patch && ./configure --prefix=/usr/local/nginx --with-http_ssl_module --with-http_stub_status_module --add-module=/usr/src/nginx-module-vts --add-module=/usr/src/nginx_upstream_check_module && make && make install
RUN mkdir /usr/local/nginx/ssl && openssl req -subj '/CN=www.example.com/O=example example/C=TR' -new -newkey rsa:2048 -sha256 -days 365 -nodes -x509 -keyout /usr/local/nginx/ssl/cert.key -out /usr/local/nginx/ssl/cert.crt

EXPOSE 80
EXPOSE 443

CMD ["/usr/local/nginx/bin/openresty"]

#  && patch -p1 < /usr/src/nginx_upstream_check_module/check_1.20.1+.patch && --add-module=/usr/src/nginx_upstream_check_module --with-cc-opt=-Wno-stringop-overread 
