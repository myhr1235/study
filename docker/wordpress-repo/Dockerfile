FROM wordpress:6.5-php8.2-apache

# Apache 경고 제거 + rewrite
RUN printf "ServerName localhost\n" > /etc/apache2/conf-available/servername.conf \
 && a2enconf servername && a2enmod rewrite

# ✅ 커스텀은 소스쪽에 둔다(컨테이너 시작 시 html로 함께 복사됨)
COPY wp-config.php /usr/src/wordpress/wp-config.php
COPY wp-content/   /usr/src/wordpress/wp-content/

# 헬스체크 파일(옵션)
RUN printf "<?php http_response_code(200); echo 'ok';" > /usr/src/wordpress/healthz.php

RUN chown -R www-data:www-data /usr/src/wordpress
