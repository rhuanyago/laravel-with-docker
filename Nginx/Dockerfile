# From Nginx fixed version
FROM macbre/nginx-http3

# Common labels
LABEL maintainer="desenvolvimento@made4it.com.br"
LABEL br.com.made4it.version="0.0.1-beta"
LABEL br.com.made4it.version.is-production="yes"
LABEL br.com.made4it.release-date="2021-11-25"
LABEL vendor="Made4it"

# Web port
EXPOSE 80

# Web SSL port
EXPOSE 443

# Add local SSL but wait for mounted custom SSL certificate
RUN mkdir -p /etc/ssl/ /etc/ssl-prod/
COPY ssl.key /etc/ssl/ssl.key
COPY ssl.crt /etc/ssl/ssl.crt

# Removing default configuration
RUN rm /etc/nginx/nginx.conf && \
    rm /etc/nginx/conf.d/* && \
    mkdir -p /etc/nginx/conf.d/connections/ && \
    mkdir -p /etc/nginx/conf.d/loadbalancer/ && \
    mkdir -p /etc/nginx/conf.d/proxy/

# Bind error log to stdout
RUN ln -sf /dev/stdout /var/log/nginx/error.log
RUN touch /var/log/nginx/blocked.log

# Add init command
COPY docker-entrypoint.sh /
RUN chmod 774 /docker-entrypoint.sh

# Creating custom errors folder
RUN mkdir -p /usr/local/nginx/html/

# Set master configuration
COPY nginx.conf /etc/nginx/

# Add custom configurations
ADD connections/*.conf /etc/nginx/conf.d/connections/
ADD proxy/* /etc/nginx/conf.d/proxy/
ADD loadbalancer/*.conf /etc/nginx/conf.d/loadbalancer/
RUN chmod 777 /etc/nginx/conf.d/loadbalancer/*

# Adding custom error pages
ADD pages/* /usr/local/nginx/html/

# Setup init commands
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["nginx", "-g", "daemon off;"]
