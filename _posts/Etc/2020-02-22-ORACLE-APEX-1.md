---
title: "Oracle apex & ords upgrade apache+tomcat+ssl"
layout: posts
excerpt: "ords 업그레이드및 셋팅및 apache tomcat ssl셋팅"
search: true
categories: 
  - Etc
tags: 
  - OracleApex
---

### ORDS 업그레이드및 apache tomcat ssl셋팅. 

### 참고만하세요!

```shell
Tip. 
oracle cloud database service을 생성할때 admin이메일 주소는 추후 변경이 없는 이메일 주소로 하여야한다. 변경이 있을경우, backup관련 문제를 일으킬 수 있다. 
instance을 생성할때, 기존오라클 라이센스 or 클라우드에 포함되는 라이센스를 선택해야 하는데, 라이센스가 없다면 후자를 선택하면 된다. 

//1. ORDS
참고       : https://blog.yannickjaquier.com/oracle/oracle-rest-data-services-ords.html

          , https://oracle-base.com/articles/misc/oracle-rest-data-services-ords-installation-on-tomcat

ords_download : https://www.oracle.com/database/technologies/appdev/rest-data-services-v192-downloads.html


su - oracle
sqlplus 'sys as sysdba'
password enter.

#ords apex - 유저별 패스워드설정및 권한설정

alter user ords_public_user identified by #password;
grant create session to ords_public_user;
alter user ords_public_user account unlock;

alter session set container=pdb1;

alter user apex_public_user identified by #password;
alter user apex_listener identified by #password;
alter user apex_rest_public_user identified by #password;

grant create session to apex_public_user;
grant create session to apex_listener;
grant create session to apex_rest_public_user;

alter user apex_public_user account unlock;
alter user apex_listener account unlock;
alter user apex_rest_public_user account unlock;

apex_download : https://www.oracle.com/technetwork/developer-tools/apex/downloads/apex-182-archive-5440684.html


기존 ords 중지및 삭제
새로다운받은 ords.war copy -> tomcat/webapp/
java -jar ords.war configdir /u01/ords/conf      # config dir 재설정
java -jar ords.war install advanced              # 고급설정으로 install
* 설정시 SERVICE_NAME 확인 (tnsnames.ora)파일 중 다음과 같은 pdb1.xxxxxx.xxxxxxxxx.internal 형태의 정보값을 지닌 네임으로 입력해야함. (중요)
Enter 1 if you wish to start in standalone mode or 2 to exit [1]:   1


/u01/apex                               # 복사및 압축해제
su - oracle
cd /u01/apex

sqlplus 'sys as sysdba' 
@apexins.sql sysaux sysaux temp /i/     # tablesspace 이름및 url 주소를 지정하는 script실행
@apex_epg_config.sql /u01/apex
@apex_reset_config

#### WAS apex images dir인 i folder생성후 이미지내역 복사

mkdir $CATALINA_HOME/webapps/i/
cp -R /tmp/apex/images/* $CATALINA_HOME/webapps/i/

1.1. APEX 
download : https://www.oracle.com/technetwork/developer-tools/apex/downloads/apex-182-archive-5440684.html
```



```shell
2. Tomcat
download : https://tomcat.apache.org/ 

install  : mkdir -p /u01/tomcat (create directory),
          , groupadd tomcat
          , useradd -s /bin/false -g tomcat -d /u01/tomcat tomcat
          , wget http://apache.tt.co.kr/tomcat/tomcat-9/v9.0.26/bin/apache-tomcat-9.0.26.zip
          , unzip apache-tomcat-8.5.30.zip
          , mv apache-tomcat-8.5.30/* /u01/tomcat
          , chmod -Rf 755 /u01/tomcat/bin/     
          , chown -hR tomcat:tomcat /u01/tomcat

setting  : server.xml (ajp port, URIEncoding="UTF8")

3. Apache
3.1. install
참고  : https://hreeman.tistory.com/m/85

install : wget http://archive.apache.org/dist/tomcat/tomcat-connectors/jk/tomcat-connectors-1.2.41-src.tar.gz
         , 압축풀은위치/native -> ./configure --with-apxs=/usr/sbin/apxs -> 오류발생시 dependency에 맞게 선행 설치후 재실행
         , make && make install (compiling)
         , 성공 후 mod_jk.so file copy -> httpd/module

3.2 setting 
apache+tomcat(mod_jk)+ssl
httpd.conf ######################################

LoadModule jk_module modules/mod_jk.so            # 주석제거
LoadModule ssl_module modules/mod_ssl.so          # 주석제거
LoadModule alias_module modules/mod_alias.so      # 주석제거
LoadModule rewrite_module modules/mod_rewrite.so  # 주석제거

Include conf/mod_jk.conf                          # 추가
Include conf/extra/httpd_vhost.conf               # 주석제거


<IfModule ssl_module>
Listen 443
SSLPassPhraseDialog exec:/usr/local/httpd/conf/ssl/ssl_password.sh #SSL_PASSWORD_FILE@1
SSLRandomSeed startup builtin
SSLRandomSeed connect builtin
</IfModule>


##################################################
#SSL_PASSWORD_FILE@1 ( ssl_password.sh 755 permission )
    #!/bin/sh
     echo "ssl password"
#SSL_PASSWORD_FILE@1
##################################################

 
mod_jk.conf ######################################

<IfModule mod_jk.c>
  JkWorkersFile conf/workers_jk.properties
  # Where to put jk shared memory
  # JkShmFile run/mod_jk.shm
  # Where to put jk logs
  JkLogFile logs/mod_jk.log
  # Set the jk log level [debug/error/info]
  JkLogLevel info
  # Select the timestamp log format
  JkLogStampFormat "[%a %b %d %H:%M:%S %Y] "
  JkMountFile conf/uriworkermap.properties
</IfModule>

##################################################


worker_jk.properties ############################
worker.list=server1, server2

worker.server1.port=8009                 # 동일한 was의 ajp포트를 바라보고 있음.
worker.server1.host=localhost
worker.server1.type=ajp13
worker.server1.lbfactor=1
worker.server1.connection_pool_timeout=60
worker.server1.socket_keepalive=1
worker.server2.host=localhost
worker.server2.type=ajp13
worker.server2.lbfactor=1
worker.server2.port=8009                 # 동일한 was의 ajp포트를 바라보고 있음.
worker.server2.connection_pool_timeout=60
worker.server2.socket_keepalive=1
##################################################

 
uriworkermap.properties #########################

/*=server1                              # /* url pattern에 해당되는 요청은 /server1 or server2에 할당.
/*=server2
##################################################


httpd-vhosts.conf #######################################
<VirtualHost *:80>
       ServerName www.domain.kr
       ServerAlias domain.kr
       RewriteEngine on
       RewriteCond %{HTTPS} off [OR]           
       RewriteCond %{HTTP:X-Forwarded-Proto} !https [OR]
       RewriteRule ^/(.*) https://domain.kr/ords/f?p=1$1 [NC,R=301,L]      #80port진입시 https해당경로 redirect
</VirtualHost>

<VirtualHost *:80>
       ServerName domain2.io
       RewriteEngine on
       RewriteCond %{HTTPS} off [OR]
       RewriteCond %{HTTP:X-Forwarded-Proto} !https [OR]
       RewriteRule ^/(.*) https://domain2.io/ords/$1 [NC,R=301,L]
</VirtualHost>

<VirtualHost *:443>
        ServerAdmin serveradmin@example.co.kr
        ServerName domain1.kr
        JkMount /* server1
        ErrorLog /u01/log/ssl_domain1_error_log
        CustomLog /u01/log/ssl_domain1_access_log common        SSLEngine on
        SSLOptions +StrictRequire +ExportCertData +StdEnvVars

        <Directory />
        AllowOverride All
        Order allow,deny
        Allow from all
        Options Indexes FollowSymLinks
        </Directory>

        RewriteEngine On
        RewriteCond %{REQUEST_URI} ^/$
        RewriteRule (.*) /ords/f?p=100 [R=301]
        SSLProtocol -all +TLSv1 +SSLv3
        SSLCipherSuite ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP
        SSLCertificateFile /u01/ssl/domain1/domain1_kr.crt                     
        SSLCertificateKeyFile /u01/ssl/domain1/domain1_kr.key
        SSLCACertificateFile /u01/ssl/domain1/ChainCA.crt

</VirtualHost>

<VirtualHost *:443>

        ServerAdmin serveradmin@example.co.kr
        ServerName dev2.domain2.io
        JkMount /* server2
        ErrorLog /u01/log/ssl_el_error_log
        CustomLog /u01/log/ssl_el_access_log common
        SSLEngine on
        SSLOptions +StrictRequire +ExportCertData +StdEnvVars

        <Directory />

        AllowOverride All
        Order allow,deny
        Allow from all
        Options Indexes FollowSymLinks
        </Directory>

        RewriteEngine On
        RewriteCond %{REQUEST_URI} ^/$
        RewriteRule (.*) /ords [R=301]

        SSLProtocol -all +TLSv1 +SSLv3
        SSLCipherSuite ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP
        SSLCertificateFile /u01/ssl/epiclive/star_domain2_io.crt
        SSLCertificateKeyFile /u01/ssl/domain2/star_domain2_io.key
        SSLCACertificateFile /u01/ssl/domain2/ChainCA.crt

</VirtualHost>

##################################################

```



 ```shell
4. Devops
4.1 apache
start  : /usr/local/httpd/bin/httpd -k start
stop   : /usr/local/httpd/bin/httpd -k stop
status : ps -ef | grep httpd

log    : tail -15f /usr/local/httpd/logs/acess.log
                   /usr/local/httpd/logs/error.log
                   /usr/local/httpd/logs/mod_jk.log
4.2 tomcat
start  : /u01/tomcat/bin/startup.sh
stop   : /u01/tomcat/bin/shutdown.sh
status : ps -ef | grep tomcat
log    : tail -15f /u01/tomcat/logs/catalina.out

 ```









