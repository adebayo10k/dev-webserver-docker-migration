#################################
# Dockerfile to build an httpd web server image 
#################################
# Base image is Apaches' httpd server (based on Redhat (official) Linux)
FROM httpd
# Author: Damola Adebayo
LABEL maintainer="Damola Adebayo <adebayo10k@gmail.com>"
#ENV APACHE_LOG_DIR=/usr/local/apache2/logs
ENV APACHE_LOG_DIR=/var/log/data_access_dir
# Create the mountpoint directory that we configured \
# for webserver logs.
RUN mkdir -p /var/log/data_access_dir
# Define the mountpoint
VOLUME [ "/var/log/data_access_dir" ]
# Copy website source code into image build
COPY ./adebayo10k.github.io/docs/ /usr/local/apache2/htdocs/
# Copy in our customised main httpd configuration file
COPY ./webserver/conf/httpd.conf /usr/local/apache2/conf/httpd.conf
# Expose port 80 to the container network
EXPOSE 80