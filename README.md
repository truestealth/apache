# apache
Apache + grack + git

## 1.
Add code to `apache_data/apache/conf/httpd.conf`
```
LoadModule passenger_module /opt/bitnami/ruby/lib/ruby/gems/2.4.0/gems/passenger-5.1.12/buildout/apache2/mod_passenger.so
<IfModule mod_passenger.c>
  PassengerRoot /opt/bitnami/ruby/lib/ruby/gems/2.4.0/gems/passenger-5.1.12
  PassengerDefaultRuby /opt/bitnami/ruby/bin/ruby
</IfModule>

LoadModule perl_module /usr/lib/apache2/modules/mod_perl.so
```

## 2.
Add code to `apache_data/apache/conf/bitnami/bitnami.conf`
```
<VirtualHost *:80>
  PassengerEnabled on
  DocumentRoot "/opt/bitnami/apache/htdocs/grack/public"
  <Directory "/opt/bitnami/apache/htdocs/grack/public">
     Options None
     AllowOverride None
     <IfVersion < 2.3 >
         Order allow,deny
         Allow from all
     </IfVersion>
     <IfVersion >= 2.3>
         Require all granted
     </IfVersion>
 </Directory>

  PerlLoadModule Apache::Redmine

  <Location "/">
    AuthType Basic
    AuthName "Redmine git repositories"
    Require valid-user

    PerlAccessHandler Apache::Authn::Redmine::access_handler
    PerlAuthenHandler Apache::Authn::Redmine::authen_handler
    RedmineDSN "DBI:mysql:database=bitnami_redmine;host=mariadb"
    RedmineDbUser "DB_USERNAME"
    RedmineDbPass "DB_PASSWORD"
    RedmineGitSmartHttp yes
 </Location>
</VirtualHost>
```
