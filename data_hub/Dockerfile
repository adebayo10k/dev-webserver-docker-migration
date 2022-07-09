#################################
# Dockerfile to build a dedicated data-only, log data \
# access volume for all log generating services
#################################
# Base image is busybox
FROM busybox:latest
# Author: Damola Adebayo
LABEL maintainer="Damola Adebayo <adebayo10k@gmail.com>"
# Create the mountpoint directory
RUN mkdir -p /var/log/data_access_dir
# Create a data-only volume as mountpoint 
VOLUME [ "/var/log/data_access_dir" ]
# Execute /bin/true for this data-only container to exit without a shell, \
# without anything but a zero exit code.
CMD [ "/bin/true" ]