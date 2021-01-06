FROM nginx:1.17.8-alpine

# Copy the public directory
COPY ./public/ /app/public/

# Copy the nginx config file
COPY ./docker/nginx/nginx.conf /etc/nginx/conf.d/default.conf

RUN chown -R www-data:www-data /var/log/nginx;
RUN chmod -R 755 /var/log/nginx;

RUN /bin/sh -c "apk add --no-cache bash"
