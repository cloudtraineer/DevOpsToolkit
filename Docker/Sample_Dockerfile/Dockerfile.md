FROM amazonlinux
RUN yum update -y
RUN yum install httpd vi -y
ADD index.html /var/www/html
CMD ["httpd", "-D", "FOREGROUND"]
ENV name Dockertest
