FROM php:apache

COPY ./ports.conf /etc/apache2/

RUN echo "<?php phpinfo(); ?>" > /var/www/html/index.php

CMD ["apache2-foreground"]