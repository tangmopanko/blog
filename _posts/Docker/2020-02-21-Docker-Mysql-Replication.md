---
title: "Mysql Replication "
layout: posts
excerpt: "Docker을 이용한 Mysql Replication"
search: true
categories:
  - Mysql
tags:
  - Mysql
---

##### 어떻게 사용했는지?

- 이전 회사에서 운용시 master 1대 , slave 3대로 운영을 하였다.  slave1은 Scalability용으로 App서버의 읽기를 담당하였고, slave2는 admin, slave3는 통계디비로 운용하였다. 이는 Scalability, Data Security/Backup / Analytics에 해당한다. 

##### 참고

- https://www.slideshare.net/NHNFORWARD/mysql-nhn-forward-2018

- https://dev.mysql.com/doc/mysql-replication-excerpt/5.6/en/replication.html

- https://wiki.ubuntu-kr.org/index.php/MySQL_Replication

- https://github.com/vbabak/docker-mysql-master-slave

##### Mysql Replication? 

Replication을 사용시 이점

- HA (High Availability_Failover)

  - Replication은 클러스트와 같은 진정한 HA솔루션이 아닙니다. 시스템장애시 데이터의 손실이 일어날 수 있습니다. 

    그러나 Master가 실패할 경우 Standby로 failover할 수 있습니다. 

- Scalability - scale up / out
  - 여러 슬레이브에 로드를 분산시켜 성능을 향상시킵니다. 이 환경에서는 모든 쓰기 및 업데이트가 마스터 서버에서 수행되어야합니다. 그러나 읽기는 하나 이상의 슬레이브에서 발생할 수 있습니다. 이 모델은 쓰기 성능을 향상시킬 수 있으며 (마스터는 업데이트 전용이므로) 점점 더 많은 슬레이브에서 읽기 속도를 크게 향상시킵니다.

- Data Security / Backup
  - 데이터가 슬레이브에 복제되고 슬레이브가 복제 프로세스를 일시 중지 할 수 있으므로 해당 마스터 데이터를 손상시키지 않고 슬레이브에서 백업 서비스를 실행할 수 있습니다.

- Analytics
  - 분석-마스터에서 라이브 데이터를 생성 할 수 있으며 마스터의 성능에 영향을주지 않고 슬레이브에서 정보를 분석 할 수 있습니다.

- Long Distance Data Dstribution
  - 장거리 데이터 배포-지점에서 주 데이터 복사본으로 작업하려는 경우 복제를 사용하여 마스터에 영구적으로 액세스 할 필요없이 로컬 데이터 복사본을 만들어 사용할 수 있습니다.

##### docker Mysql Replication

```java
git clone https://github.com/vbabak/docker-mysql-master-slave.git

./build.sh

➜ docker-mysql-master-slave git:(master) docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                               NAMES
7e91c3a2b545        mysql:5.7           "docker-entrypoint.s…"   About an hour ago   Up 36 minutes       33060/tcp, 0.0.0.0:5506->3306/tcp   mysql_slave
56061af31a19        mysql:5.7           "docker-entrypoint.s…"   About an hour ago   Up About an hour    33060/tcp, 0.0.0.0:4406->3306/tcp   mysql_master

```

##### 작동확인

![rep](/blog/assets/images/docker/mysql_rep_1.png)

![rep](/blog/assets/images/docker/mysql_rep_2.png)

