# jenkins-edu

# Jenkins 설치 

## jenkins Dockerfile
```
FROM jenkins/jenkins:2.375.3
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
RUN jenkins-plugin-cli --plugins "blueocean docker-workflow"
```
## jenkins image build 
docker build -t myjenkins .

## jenkins 컨테이너 network 생성 
```
docker network create jenkins
```

## Jenkins 컨테이너 실행
```
docker run --name jenkins --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins
```

## 비밀번호 가져오기 
```
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

## jenkins 서버 컨테이너 연결 
```
https://192.168.56.101:8080/
```

# 설치 안내 참조:
https://www.jenkins.io/doc/book/installing/docker/


# jenkins 컨테이너 agent

## agent 용 컨테이너 실행 (alpine/socat) 

https://stackoverflow.com/questions/47709208/how-to-find-docker-host-uri-to-be-used-in-jenkins-docker-plugin
```
docker run -d --restart=always \ 
-p 127.0.0.1:2376:2375 \ 
--network jenkins \ 
-v /var/run/docker.sock:/var/run/docker.sock \ 
alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock

docker inspect <container_id> | grep IPAddress
```

# jenkins python agent 사용하기 
```
docker pull devopsjourney1/myjenkinsagents:python
<<<<<<< HEAD
```
=======
```
>>>>>>> 452c6a7bf528bbabef7be145bbfe9eb00e862c81
