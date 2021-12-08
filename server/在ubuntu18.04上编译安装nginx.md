## 在ubuntu18.04上编译安装nginx

### 源码准备
#### 下载源码

```bash
mkdir /opt/nginx
cd /opt/nginx
# 下载
wget https://nginx.org/download/nginx-1.21.4.tar.gz
# 解压
tar -xvf nginx-1.21.4.tar.gz
cd /opt/nginx/nginx-1.21.4
# 创建安装目录
mkdir /opt/nginx/1.21.4
```

### 安装依赖

```bash
sudo apt-get install libgd-dev libxml2-dev zlib1g-dev libxslt1-dev libgeoip-dev
```

#### 编译安装

```bash
DIST_PATH=/opt/nginx/1.21.4
./configure --prefix=$DIST_PATH \
	--with-cc-opt='-g -O2 -fdebug-prefix-map=/build/nginx-GkiujU/nginx-1.14.0=. -fstack-protector-strong -Wformat -Werror=format-security -fPIC -Wdate-time -D_FORTIFY_SOURCE=2' \
	--with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro -Wl,-z,now -fPIC' \
	--conf-path=$DIST_PATH/conf/nginx.conf \
	--http-log-path=/var/log/nginx/access.log \
	--error-log-path=/var/log/nginx/error.log \
	--lock-path=/var/lock/nginx.lock \
	--pid-path=/run/nginx.pid \
	--modules-path=$DIST_PATH/lib/nginx/modules \
	--http-client-body-temp-path=/var/lib/nginx/body \
	--http-fastcgi-temp-path=/var/lib/nginx/fastcgi \
	--http-proxy-temp-path=/var/lib/nginx/proxy \
	--http-scgi-temp-path=/var/lib/nginx/scgi \
	--http-uwsgi-temp-path=/var/lib/nginx/uwsgi \
	--with-debug --with-pcre-jit \
	--with-http_ssl_module --with-http_stub_status_module \
	--with-http_realip_module --with-http_auth_request_module \
	--with-http_v2_module --with-http_dav_module \
	--with-http_slice_module --with-threads --with-http_addition_module \
	--with-http_geoip_module=dynamic --with-http_gunzip_module \
	--with-http_gzip_static_module --with-http_image_filter_module=dynamic \
	--with-http_sub_module --with-http_xslt_module=dynamic \
	--with-stream=dynamic --with-stream_ssl_module --with-mail=dynamic --with-mail_ssl_module \
	--with-stream
make
# 安装到目录: /opt/nginx/1.21.4
make install
```





### 参考

* [Nginx下载地址: https://nginx.org/en/download.html](https://nginx.org/en/download.html)
* 
