---
title: "ORACLE Ords+Apex 18.1 to 18.2"
layout: posts
excerpt: "OracleApex 업그레이드"
search: true
categories: 
  - Etc
tags: 
  - OracleApex
---

### ORDS & APEX 업그레이드 

### 참고만하세요!

```shell
download후 upload. (aws환경)
scp -i ./pem_file ./apex_18.2.zip opc@xxx.xxx.xx.xx:~/
scp -i ./pem_file ./ords-18.2.0.zip opc@xxx.xxx.xx.xx:~/

mkdir /u01/media

cp /home/opc/*.zip /u01/media

apex_18.2.zip 압축해제_경로 -> /u01/app/oracle/product/apex/18.2.0.00.12
cd /u01/app/oracle/product/apex/18.2.0.00.12

sqlplus 'sys as sysdba'
alter session set container=pdb1;
@apexins.sql sysaux sysaux temp /i/
@apxchpwd.sql
@apex_rest_config

cd /u01/app/oracle/product/apex/18.2.0.00.12/utilities
sqlplus 'sys as sysdba'
alter session set container=pdb1;
@reset_image_prefix

alter user apex_public_user identified by PASSWORD;
alter user apex_listener identified by PASSWORD;
alter user apex_rest_public_user identified by PASSWORD;

grant create session to apex_public_user;
grant create session to apex_listener;
grant create session to apex_rest_public_user;

alter user apex_public_user account unlock;
alter user apex_listener account unlock;
alter user apex_rest_public_user account unlock;

select profile,resource_name,limit from dba_profiles
where profile='DEFAULT' and RESOURCE_NAME='PASSWORD_LIFE_TIME';

ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;

ORDS

/etc/init.d/ords status
/etc/init.d/ords stop

ords-18.2.0.zip 압축해제_경로 -> /u01/app/oracle/product/ords/ords-18.2.0
기존 ords backup

ords.war 최신버전으로 교체.

java -jar ./ords.war install advanced
설정시 configdir -> /u01/app/oracle/product/params/ ( prefix /ords/ 가 자동으로 붙음 * 유의 )
기존 params/* 데이터 -> /u01/app/oracle/product/params/로 복사

cp -R /u01/app/oracle/product/apex/18.2.0.00.12/image/* /u01/app/oracle/product/apex/latest/images

vi /u01/app/oracle/product/ords/params/standard.properties
standalone.static.path=/u01/app/oracle/product/apex/latest/images # static images location추가. 

 
############################

http://apexugj.blogspot.com/2019/01/dbcsapex.html       # 구버전 신버전 설치방법

https://docs.oracle.com/en/database/oracle/application-express/19.1/htmig/upgrading-to-latest-AE-release.html#GUID-4B4A2BDC-11AA-4ACF-88B2-E320D6361596

https://s3.amazonaws.com/questoracle-staging/wordpress/uploads/2019/09/26131613/Listing_1.pdf                    # dbcsins이용


```



