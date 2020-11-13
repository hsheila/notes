FROM wordpress:latest
COPY ./wordpress /usr/src/wordpress-zh

RUN set -ex; \
        rm -r /usr/src/wordpress; \
        mv /usr/src/wordpress-zh /usr/src/wordpress; \

# wp-content/cache: https://github.com/docker-library/wordpress/issues/534#issuecomment-705733507
# 删除原来的wp-content，从中文包中拷贝新的文件，这里只拷贝wp-content是有风险的，也可以考虑整包替换
        rm -r wp-content; \
        mkdir wp-content; \
        for dir in /usr/src/wordpress/wp-content/*/ cache; do \
	dir="$(basename "${dir%/}")"; \
	mkdir "wp-content/$dir"; \
        done; \
        chown -R www-data:www-data wp-content; \
        chmod -R 777 wp-content

VOLUME /var/www/html

COPY docker-entrypoint.sh /usr/local/bin/

ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["apache2-foreground"]