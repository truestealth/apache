FROM bitnami/minideb-extras:jessie-r22
LABEL maintainer "Bitnami <containers@bitnami.com>"

# Install required system packages and dependencies
RUN install_packages mc build-essential wget git libapache2-mod-passenger libdbd-mysql-perl libapache2-mod-perl2 libapache-dbi-perl libapr1 libaprutil1 libc6 libexpat1 libffi6 libgmp10 libgnutls-deb0-28 libhogweed2 libldap-2.4-2 libnettle4 libp11-kit0 libpcre3 libsasl2-2 libssl1.0.0 libtasn1-6 libuuid1 zlib1g
RUN bitnami-pkg unpack apache-2.4.29-0 --checksum 38af9f5ee6088655536238d9f70f4ce7fd6047e1c84fd5b02fa351efbc4f60c6
RUN ln -sf /opt/bitnami/apache/htdocs /app

RUN curl -sSL https://raw.githubusercontent.com/redmine/redmine/master/extra/svn/Redmine.pm > /usr/share/perl5/Apache/Redmine.pm

# Ruby
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/ruby-2.4.2-0-linux-x64-debian-8.tar.gz && \
    echo "947456b3ef36072599c35e16777240eb25eb88c6d60216f538e396d7fc8fc5a5  /tmp/bitnami/pkg/cache/ruby-2.4.2-0-linux-x64-debian-8.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/ruby-2.4.2-0-linux-x64-debian-8.tar.gz -P --transform 's|^.*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/ruby-2.4.2-0-linux-x64-debian-8.tar.gz
RUN /opt/bitnami/ruby/bin/gem install bundler

#Grack
RUN git clone https://github.com/schacon/grack.git /opt/bitnami/apache/htdocs/grack
RUN mv /opt/bitnami/apache/htdocs/grack/config.ru /opt/bitnami/apache/htdocs/grack/config.ru_old
COPY conf/config.ru /opt/bitnami/apache/htdocs/grack/config.ru
RUN mkdir -p /opt/bitnami/apache/htdocs/grack/public
RUN mkdir -p /opt/bitnami/apache/htdocs/grack/tmp
RUN cd /opt/bitnami/apache/htdocs/grack/
RUN /opt/bitnami/ruby/bin/bundle install --gemfile=/opt/bitnami/apache/htdocs/grack/Gemfile


COPY rootfs /

ENV APACHE_HTTPS_PORT_NUMBER="443" \
    APACHE_HTTP_PORT_NUMBER="80" \
    BITNAMI_APP_NAME="apache" \
    BITNAMI_IMAGE_VERSION="2.4.29-r0" \
    PATH="/opt/bitnami/apache/bin:/opt/bitnami/ruby/bin/:$PATH"

EXPOSE 80 443

WORKDIR /app
ENTRYPOINT ["/app-entrypoint.sh"]
CMD ["nami","start","--foreground","apache"]
