# From MySQL fixed version
FROM mariadb:10.1.48

# Expose default MySQL port
EXPOSE 3306/tcp

# Setting root password
ENV MYSQL_ROOT_PASSWORD Rhuanyago@123
ENV MYSQL_DATABASE rhshop

# Add the content of the scripts/ directory to your image
# All scripts in docker-entrypoint-initdb.d/ are automatically
# executed during container startup
# COPY ./scripts/ /docker-entrypoint-initdb.d/

# Install basic dependencies
RUN apt-get -o Acquire::ForceIPv4=true update && \
    apt-get install -o Acquire::ForceIPv4=true \
    --no-install-recommends -y \
    default-libmysqlclient-dev \
    pkg-config libbsd-dev \
    wget ca-certificates \
    zip unzip \
    tcpdump \
    vim

# Binding all addresses to MySQL listen
RUN echo "bind-address = 0.0.0.0" >> /etc/mysql/my.cnf

# Changing permission of these scripts 
RUN chown -R mysql:mysql /docker-entrypoint-initdb.d/

# Creating MySQL featured folder
RUN mkdir -p /var/lib/mysql-files && \
    chown -R mysql:mysql /var/lib/mysql-files && \
    chmod 700 /var/lib/mysql-files