FROM nginx:1.25.3

RUN mkdir -p var/www/html && \
  sed -i '6 a\load_module modules/ngx_http_image_filter_module.so;' /etc/nginx/nginx.conf && \
  rm /etc/nginx/conf.d/default.conf
COPY playsdev var/www/html
COPY playsdev-ddns-net.conf etc/nginx/conf.d/
COPY ssl etc/nginx
RUN chmod 600 etc/nginx/key.key && \
  chmod 600 etc/nginx/key.pem
# groupadd -r user && \
# useradd -g user user
#     chown -R user:user /etc/nginx && \
#     chown -R user:user /var && \ 
#     chown -R user:user /run
# USER user
