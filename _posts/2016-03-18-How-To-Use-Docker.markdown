---
layout: post
title:  도커사용법
subtitle: 요즘 대세 도커를 정리해두자!
description: 서버에 도커기술을 탑재하는 단계를 정리하여 실서버에 적용하는 방법,  운영하는 방법, 복제하는 방법, 서비스를 적용하는 방법등을 전부 정리할 예정이다. 
date:   2016-03-18 15:25:00 +"09:00"
categories: docker
featured-image: https://raw.githubusercontent.com/rilts/rilts.github.io/master/images/docker-wallpaper-black.jpg
thumbnail-image: https://raw.githubusercontent.com/rilts/rilts.github.io/master/images/docker-wallpaper-black.jpg
comments: true
author: June
author-image: https://avatars2.githubusercontent.com/u/14028474?v=3&s=400
author-bio: I'm interested in new stuffs, new technologies 
---

최근에 신기술에 대해서 조사하면서 가장 배우고 싶은 기술로 docker가 되었다. 지금 하고 있는 일에서도 여러 서버를 설정해야 하는 일들이 있어서 일에 바로 적용하려고 공부하면서 익히는 부분에 대해서 바로 정리할 필요가 있어 블로그로 적어 둔다. 

* 반드시 root 권한으로 명령을 시작해야 한다.
* 기본 명령어
  * 이미지 검색
    * sudo docker search ubuntu
  * 이미지 가져 오기
    * sudo docker pull ubuntu:latest
  * 이미지 목록출력
    * sudo docker images
  * 이미지 실행하기
    * sudo dockert run -i -t --name hello ubuntu /bin/bash
    * docker run <option> <image name> <run program>
    * 이미지 안에서 호스트 OS로 빠져 나오기 < exit >
  * Docker 서비스 재시작
    * sudo servie docker restart
  * 컨테이너 목록 출력 
    * duso docker ps -a
  * 컨테이너 재시작
    * sudo docker restart hello
  * 컨테이너에 접속
    * sudo docker attach hello
  * 컨테이너에서 빠져나오는 방법
    * exit (컨테이너 정지)
    * Ctrl + D (컨테이너 정지)
    * Ctrl + P, Ctrl + Q (컨테이너를 정지하지 않고 빠져나오기) 
  * 호스트에서 컨테이너에 명령실행하기
    * sudo docker exec hello echo "Hello World"
    * docker exec <컨테이너> <명령> <매개변수>
  * 컨테이너 정지
    * sudo docker stop hello
  * 컨테이너 삭제
    * sudo docker rm hello
  * 이미지 삭제
    * sudo docker rmi ubuntu:latest
* Dockerfile

{% highlight ruby %}
script
FROM ubuntu:14.04
MAINTAINER rilts@test.com

RUN apt-get update
RUN apt-get install -y nginx
RUN echo"\ndaemon off:" >> /etc/nginx/nginx.conf
RUN chown -R www-data:www-data /var/lib/nginx

VOLUME ["/data", "/etc/nginx/site-neabled", "/var/log/nginx"]

WORKDIR /etc/nginx

CMD ["nginx"]

EXPOSE 80
EXPOSE 443
{% endhighlight %}


    * VOLUME:호스트와 공유할 디렉토리 목록
  * build
    * sudo docker build --tag hello:0.1 .
    * docker build <option> <Dockerfile path>
    * build history 조회
    * duco docker history hello:0.1
  * 현재 디렉토리에 파일 복사
    * sudo docker cp hello-nginx:/etc/nginx/nginx.conf ./
    * docker cp <컨테이너 이름>:<path> <host path>
  * 컨테이너의 변경사항을 이미지파일로 생성
    * sudo docker commit -a "Foo bar" -m "add hello.txt" hello-nginx hello:0.2
    * docker commit <option> <container name> <image name>:<tag>
  * 변경된 파일 목록
    * sudo docker diff hello-nginx
    * docker diff <container name>
  * 컨테이너 세부 정보 출력
    * sudo docker inspect hello-nginx
* 컨테이너 연결하기
  * sudo docker run --name db -d mongo
  * sudo docker run --name web -p 80:80 --link db:db nginx
  * --link <container name>:<alias>

