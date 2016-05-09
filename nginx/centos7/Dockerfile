# "ported" by Adam Miller <maxamillion@fedoraproject.org> from
#   https://github.com/fedora-cloud/Fedora-Dockerfiles
#
# Originally written for Fedora-Dockerfiles by
#   scollier <scollier@redhat.com>
#
# Enriched by patterns found at https://github.com/openshift/postgresql/blob/master/9.4/Dockerfile.rhel7 by
#   Christoph Görn <goern@redhat.com>

FROM centos:centos7
MAINTAINER The CentOS Project <cloud-ops@centos.org>

# Labels consumed by Red Hat build service
LABEL Component="nginx" \
      Name="centos/nginx-180-centos7" \
      Version="1.8.0" \
      Release="3"

# Labels could be consumed by OpenShift
LABEL io.k8s.description="NGINX is a free, open-source, high-performance HTTP server and reverse proxy, as well as an IMAP/POP3 proxy server. NGINX is known for its high performance, stability, rich feature set, simple configuration, and low resource consumption." \
      io.k8s.display-name="NGINX 1.8.0" \
      io.openshift.expose-services="80:http" \
      io.openshift.tags="nginx"

RUN yum -y install --setopt=tsflags=nodocs centos-release-scl-rh && \
    yum -y update --setopt=tsflags=nodocs && \
    yum -y install --setopt=tsflags=nodocs scl-utils rh-nginx18 && \
    yum clean all && \
    mkdir -p /usr/share/nginx/html

# Get prefix path and path to scripts rather than hard-code them in scripts
ENV CONTAINER_SCRIPTS_PATH=/usr/share/container-scripts/nginx \
ENABLED_COLLECTIONS=rh-nginx18

# When bash is started non-interactively, to run a shell script, for example it
# looks for this variable and source the content of this file. This will enable
# the SCL for all scripts without need to do 'scl enable'.
ENV BASH_ENV=${CONTAINER_SCRIPTS_PATH}/scl_enable \
    ENV=${CONTAINER_SCRIPTS_PATH}/scl_enable \
    PROMPT_COMMAND=". ${CONTAINER_SCRIPTS_PATH}/scl_enable"

ADD root /

# ADD https://git.centos.org/sources/httpd/c7/acf5cccf4afaecf3afeb18c50ae59fd5c6504910 /usr/share/nginx/html/
# RUN sed -i -e 's/Apache/nginx/g' -e '/apache_pb.gif/d' /usr/share/nginx/html/index.html
RUN echo "nginx on CentOS7" > /usr/share/nginx/html/index.html && \
    mkdir -p /var/opt/rh/rh-nginx18/lib/nginx/tmp/ && \
    chmod 777 /var/opt/rh/rh-nginx18/lib/nginx/tmp/
# FIXME this last two commands is to get around some OpenShift problems:
# 2016/05/09 18:45:02 [emerg] 1#0: mkdir() "/var/opt/rh/rh-nginx18/lib/nginx/tmp/proxy" failed (13: Permission denied)

EXPOSE 80

ENTRYPOINT ["container-entrypoint"]
CMD [ "nginx18" ]
