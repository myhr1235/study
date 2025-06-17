# WordPress 최신 이미지를 기반으로
FROM wordpress:latest

# WP 설정파일과 wp-content 디렉토리를 컨테이너로 복사
COPY wp-config.php /var/www/html/wp-config.php
COPY wp-content /var/www/html/wp-content

# 권한 설정 (optional)
RUN chown -R www-data:www-data /var/www/html

