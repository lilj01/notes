Dockerfile

```nginx
RUN yum install pcre pcre-devel zlib zlib-devel gcc gcc-c++ automake autoconf libtool make openssl openssl-devel -y 
WORKDIR /usr/local
ADD nginx-1.16.1.tar.gz .
WORKDIR /usr/local/nginx-1.16.1/
RUN ./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
RUN make && make install
ENV PATH /usr/local/nginx/sbin:$PATH
EXPOSE 80
ENTRYPOINT ["nginx"]
CMD ["-g","daemon off;"]
```



start image:

```nginx
docker run --name ng_01 -d -v/usr/local/docker/mynginx/web:/usr/local/nginx/html -p 80:80  lilj/nginx:01
```

