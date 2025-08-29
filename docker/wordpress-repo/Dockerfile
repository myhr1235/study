FROM wordpress:6.5-php8.2-apache

# 🔄 기존과 동일: Apache 경고 제거 + rewrite 모듈
RUN printf "ServerName localhost\n" > /etc/apache2/conf-available/servername.conf \
 && a2enconf servername && a2enmod rewrite \
 && a2enmod headers  # ✅ ECS용 추가: HTTP 헤더 모듈 (보안, 캐싱용)

# ✅ ECS용 추가: PHP 성능 최적화 (Fargate 환경에서 메모리 효율성 개선)
RUN docker-php-ext-install opcache

# ✅ ECS용 추가: OPcache 설정 (컨테이너 재시작 시에도 성능 유지)
RUN { \
    echo 'opcache.memory_consumption=128'; \
    echo 'opcache.interned_strings_buffer=8'; \
    echo 'opcache.max_accelerated_files=4000'; \
    echo 'opcache.revalidate_freq=2'; \
    echo 'opcache.fast_shutdown=1'; \
    } > /usr/local/etc/php/conf.d/opcache-recommended.ini

# ✅ ECS용 추가: Apache 압축/캐싱 모듈 (로드밸런서와 함께 사용)
RUN { \
    echo 'LoadModule deflate_module modules/mod_deflate.so'; \
    echo 'LoadModule expires_module modules/mod_expires.so'; \
    } >> /etc/apache2/apache2.conf

# 🔄 기존과 동일: 커스텀 wp-config.php 복사
COPY wp-config.php /usr/src/wordpress/wp-config.php

# 🔄 기존과 동일: 커스텀 컨텐츠 복사 (테마, 플러그인 등)
COPY wp-content/ /usr/src/wordpress/wp-content/

# 🔄 기존과 동일: 헬스체크 엔드포인트 (하지만 ECS에서 실제로 사용됨)
RUN printf "<?php http_response_code(200); echo 'ok';" > /usr/src/wordpress/healthz.php

# 🔄 기존과 동일: 권한 설정
RUN chown -R www-data:www-data /usr/src/wordpress

# ✅ ECS용 추가: 명시적 포트 노출 (ECS 태스크 정의에서 필요)
EXPOSE 80

# ✅ ECS용 추가: Docker 헬스체크 설정 (ECS 헬스체크와 연동)
HEALTHCHECK --interval=30s --timeout=10s --start-period=60s --retries=3 \
    CMD curl -f http://localhost/healthz.php || exit 1
