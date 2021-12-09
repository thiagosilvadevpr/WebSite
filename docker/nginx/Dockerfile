FROM nginx:1.21

RUN apt-get update && apt-get install -y \
	nano

RUN ln -snf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime \
  && echo America/Sao_Paulo > /etc/timezone

COPY ./vhost/default.conf /etc/nginx/conf.d/default.conf
#COPY ./vhost/nginx.conf /etc/nginx/nginx.conf

WORKDIR /var/www

EXPOSE 80
