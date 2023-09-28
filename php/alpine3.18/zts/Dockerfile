#
# NOTE: THIS DOCKERFILE IS GENERATED VIA "apply-templates.sh"
#
# PLEASE DO NOT EDIT IT DIRECTLY.
#

FROM alpine:3.18

LABEL maintainer="Wy Developers <952020468@qq.com>"

ENV TZ=Asia/Shanghai \
	LANG=C.UTF-8 \
    LANG=zh_CN.UTF-8 \
    LANGUAGE=zh_CN.UTF-8 \
    PHP_INI_DIR="/usr/local/etc/php"

ENV PHP_CFLAGS="-fstack-protector-strong -fpic -fpie -O2 -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64"
ENV PHP_CPPFLAGS="$PHP_CFLAGS"
ENV PHP_LDFLAGS="-Wl,-O1 -pie"
ENV PHP_URL="https://www.php.net/distributions/php-8.1.23.tar.xz"

COPY docker-php-* /usr/local/bin/

RUN set -eux; \
	apk add --update tzdata; \
	cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime; \
    echo "Asia/Shanghai" > /etc/timezone; \
    apk add --update ttf-dejavu fontconfig; \
	apk add --no-cache \
		ca-certificates \
		curl \
		curl-dev \
		tar \
		xz \
		autoconf \
		dpkg-dev \
		dpkg \
		file \
		g++ \
		gcc \
		libc-dev \
		make \
		pkgconf \
		re2c \
		linux-headers \
		openssl \
		openssl-dev \
		pcre \
		pcre-dev \
		pcre2-dev \
		argon2-dev \
		coreutils \
		curl-dev \
		gnu-libiconv-dev \
		libsodium-dev \
		libxml2-dev \
		linux-headers \
		oniguruma-dev \
		readline-dev \
		sqlite-dev \
		freetype \
		freetype-dev \
		libwebp \
		libwebp-dev \
		libpng \
		libpng-dev \
		libjpeg-turbo \
		libjpeg-turbo-dev \
		gettext-dev \
		bzip2-dev \
		libxslt-dev \
		libzip-dev \
		gnupg \
		wget; \
	mkdir -p /usr/src; \
	cd /usr/src; \
	curl -fsSL -o php.tar.xz "$PHP_URL"; \
	export \
		CFLAGS="$PHP_CFLAGS" \
		CPPFLAGS="$PHP_CPPFLAGS" \
		LDFLAGS="$PHP_LDFLAGS" \
	; \
	mkdir -p "$PHP_INI_DIR/conf.d"; \
	docker-php-source extract; \
	cd /usr/src/php; \
	gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)"; \
	./configure \
		--build="$gnuArch" \
		--with-config-file-path="$PHP_INI_DIR" \
		--with-config-file-scan-dir="$PHP_INI_DIR/conf.d" \
		--enable-option-checking=fatal \
		--with-mhash \
		--with-pic \
		--enable-ftp \
		--enable-mbstring \
		--enable-mysqlnd \
		--with-mysqli=mysqlnd \
		--with-pdo-mysql=mysqlnd \
		--with-password-argon2 \
		--with-sodium=shared \
		--with-pdo-sqlite=/usr \
		--with-sqlite3=/usr \
		--with-curl \
		--with-iconv=/usr \
		--with-openssl \
		--with-readline \
		--with-zlib \
		--enable-bcmath \
		--enable-gd \
		--with-webp=/usr/include/webp \
		--with-jpeg=/usr/include \
		--with-freetype=/usr/include/freetype2 \
		--enable-pcntl \
		--enable-sockets \
		--enable-sysvmsg \
		--enable-sysvsem \
		--enable-sysvshm \
		--enable-shmop \
		--enable-soap \
		--enable-xml \
		--enable-opcache \
		--enable-mbregex \
		--with-zip \
		--disable-phpdbg \
		--with-pear \
		$(test "$gnuArch" = 's390x-linux-musl' && echo '--without-pcre-jit') \
		--disable-cgi \
		--enable-embed \
		--enable-zts \
		--disable-zend-signals \
		--enable-zend-max-execution-timers; \
	make -j "$(nproc)"; \
	find -type f -name '*.a' -delete; \
	make install; \
	find \
		/usr/local \
		-type f \
		-perm '/0111' \
		-exec sh -euxc ' \
			strip --strip-all "$@" || : \
		' -- '{}' + \
	; \
	make clean; \
	cp -v php.ini-* "$PHP_INI_DIR/"; \
	cp php.ini-production "$PHP_INI_DIR/php.ini"; \
	cd /; \
	docker-php-source delete; \
	runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	)"; \
	apk add --no-cache $runDeps; \
	pecl update-channels; \
	rm -rf /tmp/pear ~/.pearrc; \
	php --version; \
	docker-php-ext-enable sodium; \
	docker-php-ext-enable opcache; \
	echo "opcache.enable_cli = 'On'" >> "$PHP_INI_DIR/conf.d/docker-php-ext-opcache.ini"; \
	cd /tmp; \
	curl -L -o /tmp/igbinary-3.2.7.tgz http://pecl.php.net/get/igbinary-3.2.7.tgz; \
	tar -zxvf /tmp/igbinary-3.2.7.tgz; \
	docker-php-source extract; \
	cp -r /tmp/igbinary-3.2.7 /usr/src/php/ext/igbinary; \
	cd /usr/src/php/ext/igbinary; \
	docker-php-ext-install igbinary; \
	cd /tmp; \
	curl -L -o /tmp/redis-5.3.7.tgz http://pecl.php.net/get/redis-5.3.7.tgz; \
	tar -zxvf /tmp/redis-5.3.7.tgz; \
	docker-php-source extract; \
	cp -r /tmp/redis-5.3.7 /usr/src/php/ext/redis; \
	cd /usr/src/php/ext/redis; \
	apk add --no-cache alpine-sdk zlib-dev zlib-static xz-dev zstd-dev zstd-static lz4-dev lz4-static; \
	docker-php-ext-configure redis --enable-redis-igbinary --enable-redis-lzf --enable-redis-zstd --enable-redis-lz4 --with-liblz4=/usr/lib/; \
	docker-php-ext-install redis; \
	cd /tmp; \
	curl -L -o /tmp/swoole-5.0.3.tgz http://pecl.php.net/get/swoole-5.0.3.tgz; \
	tar -zxvf /tmp/swoole-5.0.3.tgz; \
	docker-php-source extract; \
	cp -r /tmp/swoole-5.0.3 /usr/src/php/ext/swoole; \
	cd /usr/src/php/ext/swoole; \
	apk add --no-cache nghttp2 c-ares-dev; \
	docker-php-ext-configure swoole --enable-sockets --enable-openssl --with-openssl-dir=/usr --with-nghttp2-dir=/usr/lib --enable-mysqlnd --enable-swoole-curl --enable-cares --enable-brotli; \
	docker-php-ext-install swoole; \
	echo "swoole.use_shortname = 'Off'" >> "$PHP_INI_DIR/conf.d/docker-php-ext-swoole.ini"; \
	cd /tmp; \
	curl -L -o /tmp/parallel-1.2.1.tgz http://pecl.php.net/get/parallel-1.2.1.tgz; \
	tar -zxvf /tmp/parallel-1.2.1.tgz; \
	docker-php-source extract; \
	cp -r /tmp/parallel-1.2.1 /usr/src/php/ext/parallel; \
	cd /usr/src/php/ext/parallel; \
	docker-php-ext-install parallel; \
	touch "$PHP_INI_DIR/conf.d/docker-php-ext-zz-overrides.ini"; \
	echo "upload_max_filesize=512M" >> "$PHP_INI_DIR/conf.d/docker-php-ext-zz-overrides.ini"; \
	echo "post_max_size=512M" >> "$PHP_INI_DIR/conf.d/docker-php-ext-zz-overrides.ini"; \
	echo "memory_limit=1G" >> "$PHP_INI_DIR/conf.d/docker-php-ext-zz-overrides.ini"; \
	echo "date.timezone=Asia/Shanghai" >> "$PHP_INI_DIR/conf.d/docker-php-ext-zz-overrides.ini"; \
    rm -rf /var/cache/apk/* /tmp/*; \
    mkdir -p /opt/www; \
    apk add composer

WORKDIR /opt/www

EXPOSE 9501
EXPOSE 9502

ENTRYPOINT ["php", "/opt/www/bin/hyperf.php", "start"]
