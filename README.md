---
layout: default
title: Today I Learned
nav_order: 1
has_children: true
child_nav_order: desc
permalink: /
---

# Today I Learned <!-- omit in toc -->

* [23.1.12](#23112)
  * [bash 쉘의 환경 변수](#bash-쉘의-환경-변수)
* [23.1.11](#23111)
  * [VirtualBox 설정](#virtualbox-설정)
    * [복사, 붙여넣기 설정](#복사-붙여넣기-설정)
    * [docker, docker-compose](#docker-docker-compose)
* [23.1.10](#23110)
  * [env file](#env-file)
* [23.1.9](#2319)
  * [volume](#volume)
  * [bind mount](#bind-mount)
* [23.1.7](#2317)
  * [docker-compose volume configuration reference](#docker-compose-volume-configuration-reference)
    * [driver](#driver)
    * [driver\_opts](#driver_opts)
* [23.1.5](#2315)
  * [docker-compose depends\_on](#docker-compose-depends_on)
* [23.1.4](#2314)
  * [php extentions](#php-extentions)
  * [wordpress cli](#wordpress-cli)
* [23.1.3](#2313)
  * [wordpress 설정파일](#wordpress-설정파일)

---

## 23.1.12

### bash 쉘의 환경 변수

bash 쉘에는 전역 변수와 지역 변수가 있다. 전역 환경 변수는 쉘 세션 및 그로부터 파생된 자식 서브쉘에서 볼 수 있다. 하지만 지역 변수는 이를 만든 쉘에서만 사용할 수 있다.  

https://aroundck.tistory.com/6920  

## 23.1.11

### VirtualBox 설정

#### 복사, 붙여넣기 설정

[Install the VirtualBox Guest Additions in Debian 11 bullseye](https://www.pragmaticlinux.com/2021/09/install-the-virtualbox-guest-additions-in-debian-11-bullseye/)  
[Unable to insert the virtual optical disk 해결 방법](https://kldp.org/node/156897)  

shared folder를 설정하는 도중에 `usermod: command not found`가 발생되면 `/usr/sbin/usermod`를 사용하면 된다.  

#### docker, docker-compose 

[Install docker-compose](https://developer-eun-diary.tistory.com/138)  



## 23.1.10

### env file

https://seongjin.me/environment-variables-in-docker-compose/  

환경변수를 별도의 파일로 구성할 수 있다. 이는 간단하게 docker-compose.yaml 파일이 위치한 경로에 `.env` 파일을 추가하면 된다. `.env`는 다음과 같은 문법을 따라서 작성해야 한다.  

- `변수명=값` 형태로 한 줄씩 작성
- `#` 문자는 주석 
- 빈 줄은 무시된다.
- 따옴표 처리는 불필요

`.env` 파일은 다른 설정없이 자동으로 docker-compose 파일에 반영된다.  

```
# .env
MYSQL_PASSWORD=1234
```

```
# docker-compose.yaml
version: '3'
services:
  mysql:
    image: mysql
    environment:
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
```

`.env` 파일을 이용하는 방식은 간편하지만 `docker-compose up` 명령어를 수행하는 경우에만 적용된다.  



## 23.1.9

https://medium.com/dtevangelist/docker-%EA%B8%B0%EB%B3%B8-5-8-volume%EC%9D%84-%ED%99%9C%EC%9A%A9%ED%95%9C-data-%EA%B4%80%EB%A6%AC-9a9ac1db978c  

### volume

Docker가 volume을 생성하고 관리하는 방식이다. docker volume create 명령으로 volume을 생성하거나, container 생성 중에 volume을 생성할 수 있다.  

- volume이 생성되면 host의 디렉토리에 저장된다. 
- 생성된 volume을 container에 mount하면, host의 디렉토리가 mount된다.
- volume은 Docker에 의해 관리되며, host 시스템과 분리된다.
- docker volume 명령어로 볼륨을 생성하거나 제거할 수 있다. 

### bind mount

host 시스템의 파일 또는 디렉토리가 container에 mount된다. 

- 파일 또는 디렉토리가 host 시스템의 전체 경로로 참조된다.
- 만약에 파일 또는 디렉토리가 존재하지 않는 경우에는 참조된 경로로 생성된다. 
- bind mount는 host의 파일 시스템 디렉토리 구조에 의존적이다.
- Docker CLI 명령어로 관리할 수 없다.

> bind mount는 container에서 실행 중인 프로세스가 host 파일 시스템의 중요한 시스템 파일이나 디렉토리를 생성, 수정, 삭제 명령을 통해 변경할 수 있다. 이는 host 시스템에서 Non-Docker 프로세스와 충돌을 일으키거나, 보안에 큰 영향을 줄 수 있다. 그러므로 bind mount보다 volume을 사용해야 한다.  




## 23.1.7

### docker-compose volume configuration reference

https://docs.docker.com/compose/compose-file/compose-file-v3/#volume-configuration-reference  

서비스 선언의 일부로 볼륨을 선언할 수 있지만, 여러 서비스에서 재사용할 수 있고, docker 명령을 사용하여 쉽게 검색할 수 있는 named 볼륨을 생성할 수 있다. 다음은 데이터 디렉터리를 볼륨으로 두개의 서비스가 공유하는 설정의 예이다.  

```
version: "3.9"

services:
  db:
    image: db
    volumes:
      - data-volume:/var/lib/db
  backup:
    image: backup-service
    volumes:
      - data-volume:/var/lib/backup/data

volumes:
  data-volume:
```

볼륨 키 아래의 항목은 비어있을 수 있으며, 이 경우 기본 드라이버를 사용한다. 선택적으로 다음 키를 사용하여 볼륨을 구성할 수 있다.  

#### driver

볼륨에 사용할 볼륨 드라이버를 지정한다. 기본적으로 Docker 엔진이 사용하도록 구성된 모든 드라이버가 사용되며 대부분의 경우 로컬이다. 드라이버를 사용할 수 없는 경우 엔진이 오류를 반환한다.

```
driver: foobar
```

#### driver_opts

볼륨의 드라이버에 전달할 키-값 쌍으로 옵션 목록을 지정한다. 옵션은 드라이버에 따라 다르다. 자세한 내용은 드라이버 설명서를 참조하면 된다.  

```
volumes:
  example:
    driver_opts:
      type: "nfs"
      o: "addr=10.40.0.199,nolock,soft,rw"
      device: ":/docker/example"
```


## 23.1.5

### docker-compose depends_on

[Docker Compose를 사용한 여러 컨테이너의 구성 관리](https://nomad-programmer.tistory.com/317), [Docker Compose에서 컨테이너 startup 순서 컨트롤하기](https://jupiny.com/2016/11/13/conrtrol-container-startup-order-in-compose/)  

depends_on은 서비스간의 의존관계를 지정하여 순서대로 서비스를 시작하기 위해 사용된다. 하지만 depends_on은 순서만 제어할 뿐, 컨테이너의 어플리케이션이 이용 가능할 때까지 기다리지 않는다. 그렇기 때문에 이에 대한 조치가 필요하다면 대책이 필요하다. 만약에 다른 서비스가 실행 가능한 상태가 될 때까지 기다리길 원한하면 dockerize라는 툴을 사용하면 된다.  


## 23.1.4

### php extentions

https://make.wordpress.org/hosting/handbook/server-environment/#php-extensions  

### wordpress cli

[wp-cli 공식 페이지](https://wp-cli.org/)  
[wp-cli wordpress 페이지](https://make.wordpress.org/cli/handbook/guides/installing)

https://www.lesstif.com/system-admin/wp-cli-22643947.html  

```
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
chmod +x wp-cli.phar
mv wp-cli.phar /usr/local/bin/wp

cd /var/www/html
wp core download --locale=ko_KR
wp core install --url=$WP_URL  --title=$WP_TITLE --admin_user=$WP_ADMIN_USER --admin_password=$WP_ADMIN_PWD --admin_email=$WP_ADMIN_EMAIL
```


## 23.1.3

### wordpress 설정파일

wordpress의 설정 파일은 wp-config.php이다. 그러나 wordpress를 설치한 직후에는 파일이 존재하지 않고, wp-config-sample.php 파일을 제공한다. 이를 복사하여 wp-config.php 파일을 만들고, 파일을 수정하면 된다. 수정할 사항은 다음과 같다.   

- Database 정보 값 수정
- Auth key, salts 값 입력

```
# wp-config.php 생성
cp -p /var/www/html/wordpress/wp-config-sample.php /var/www/html/wordpress/wp-config.php
```

```
# Database 정보 값 수정
sed -i "s|define( 'DB_NAME', 'database_name_here' );|define( 'DB_NAME', '$DB_NAME' );|g" /var/www/html/wp-config.php
sed -i "s|define( 'DB_USER', 'username_here' );|define( 'DB_USER', '$DB_USER' );|g" /var/www/html/wp-config.php
sed -i "s|define( 'DB_PASSWORD', 'password_here' );|define( 'DB_PASSWORD', '$DB_PWD' );|g" /var/www/html/wp-config.php
sed -i "s|define( 'DB_HOST', 'localhost' );|define( 'DB_HOST', '$DB_HOST' );|g" /var/www/html/wp-config.php
```

```
# wordpress 랜덤 문자열 파일 생성
curl https://api.wordpress.org/secret-key/1.1/salt/ > salt
```

salt 파일의 문자열을 sed로 치환하기 어렵다. sed 명령어 치환 문자열의 구분자를 정하기 어렵기 때문이다. salt 문자열에 없는 문자를 구분자로 사용해야 하지만 거의 모든 문자를 사용하여 문자열을 만들어 준다. [sed: bad option in substitution expression](https://superuser.com/questions/1015234/sed-command-returning-sed-bad-option-in-substitution-expression)  

그러므로 wp-config.php 파일을 미리 생성하여 /var/www/html/wordpress에 복사하는 방법을 사용하기로 한다.  

참조 : [wordpress wp-config.php](https://swiftcoding.org/wp-config-file)  
