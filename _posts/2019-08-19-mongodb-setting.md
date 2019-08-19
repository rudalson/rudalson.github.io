---
layout: post
title:  "MongoDB Setting"
categories: database
tags: database nosql mongo
---
# MongoDB 설치
AWS 인스턴스에서 [mongodb를 우분투에 설치](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)하기 위해서는

## mongodb install
```bash
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4

$ echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list

$ sudo apt-get update

$ sudo apt-get install -y mongodb-org
```

## data directory 만들기
```
$ sudo mkdir -p /data/mongodb

$ sudo chown -R ubuntu:ubuntu /data

$ chmod 777 /data/mongodb
```

## 설정 변경
```
$ sudo vi /etc/mongod.conf
...
dbpath=/data/mongodb
```

# Mongodb 기본 운영
## mongodb 서비스 실행
```bash
$ sudo service mongod start
```

## 실행되었는지 확인
```
$ sudo cat /var/log/mongodb/mongod.log
```

## 서비스 등록
```
$ sudo systemctl enable mongod
```

## 서비스 제어
```
sudo service mongod restart  # 재기동
sudo service mongod stop     # 정지
sudo service mongod status   # 상태보기
```

# Mongodb 실행
```
$ mongo

# DB 선택
> use employee
# 유저 생성
> db.createUser( {user: 'kim', pwd: 'kim2019', roles: ["readWrite", "dbAdmin"] } )
```

# 포트 포워딩
응용 프로그램의 포트가 외부로 오픈되야 할 경우 AWS에서는 어떻게 하는지 정확히ㅣ 몰라 아래처럼 임시로 하였다. 가령  3000 port를 80port 로 변경하여야 하는데 우분투에서 아래의 명령을 주었다.
```
$ sudo iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 3000
```
