---
layout: post
title:  "Ubuntu 18.04에서 MYSQL 8 설치하기"
categories: etc
tags: ubuntu mysql8 gpgkey
---

`우분투 18.04`에서 `mysql 8.x` 를 설치하려고 할 때 key값 에러로 인하여 실패할 때가 있다. 이를 해결하기 위해 일반적인 mysql 8.x 설치 과정부터 살펴 보려한다.

# Mysql 8.x Install
[A Quick Guide to Using the MySQL APT Repository](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/) 에서 언급한 대로 한다면,

### Step 1: Download the MySQL Repositories
```
$ wget -c https://dev.mysql.com/get/mysql-apt-config_0.8.11-1_all.deb
```

### Step 2: Install the MySQL Repositories
```
$ sudo dpkg -i mysql-apt-config_0.8.11-1_all.deb
```

### Step 3: Refresh the Repositories
```
$ sudo apt-get update
```

### Step 4: Install MySQL
```
$ sudo apt-get install mysql-server
```
일반적으로 이렇게 해서 별 문제가 없다면 mysql 8.x 설치가 가능하다.

## Expired Key (EXPKEYSIG) with APT 문제 발생
일부 ubuntu 머신에서 아래와 같은 문제가 발생하는 경우가 있다.

```shell script
$ sudo apt-get update
Hit:1 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu bionic InRelease
Hit:2 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu bionic-updates InRelease
Hit:3 http://ap-northeast-2.ec2.archive.ubuntu.com/ubuntu bionic-backports InRelease
Get:4 http://repo.mysql.com/apt/ubuntu bionic InRelease [19.4 kB]
Err:4 http://repo.mysql.com/apt/ubuntu bionic InRelease
  The following signatures were invalid: EXPKEYSIG 8C718D3B5072E1F5 MySQL Release Engineering <mysql-build@oss.oracle.com>
Hit:5 http://security.ubuntu.com/ubuntu bionic-security InRelease
Reading package lists... Done
W: GPG error: http://repo.mysql.com/apt/ubuntu bionic InRelease: The following signatures were invalid: EXPKEYSIG 8C718D3B5072E1F5 MySQL Release Engineering <mysql-build@oss.oracle.com>
E: The repository 'http://repo.mysql.com/apt/ubuntu bionic InRelease' is not signed.
N: Updating from such a repository can't be done securely, and is therefore disabled by default.
N: See apt-secure(8) manpage for repository creation and user configuration details.
```

gpg signature key가 expire 되었다는 내용인데 해결책은 아래와 같다.

```shell script
$ sudo apt-key adv --keyserver keys.gnupg.net --recv-keys 8C718D3B5072E1F5
Executing: /tmp/apt-key-gpghome.QD2aYIUvET/gpg.1.sh --keyserver keys.gnupg.net --recv-keys 8C718D3B5072E1F5
gpg: key 8C718D3B5072E1F5: 3 duplicate signatures removed
gpg: key 8C718D3B5072E1F5: 102 signatures not checked due to missing keys
gpg: key 8C718D3B5072E1F5: "MySQL Release Engineering <mysql-build@oss.oracle.com>" 29 new signatures
gpg: Total number processed: 1
gpg:         new signatures: 29
```

이후에는 아래의 명령들이 잘 수행되며
```
$ sudo apt-get update
$ sudo apt-get install mysql-server
```

최종 수행한 결과는 아래처럼 mysql 8.0.19 설치에 성공하였다.
```
$ mysql --version
mysql  Ver 8.0.19 for Linux on x86_64 (MySQL Community Server - GPL)
```
