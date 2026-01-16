install for jenkins:
1) jenkins - https://hub.docker.com/r/jenkins/jenkins ( docker pull jenkins/jenkins:latest-jdk25 )
2) https://www.jenkins.io/doc/book/installing/ ( manual )
3) docker network create jenkins
4) docker run --name jenkins-docker --rm --detach \
   --privileged --network jenkins --network-alias docker \
   --env DOCKER_TLS_CERTDIR=/certs \
   --volume jenkins-docker-certs:/certs/client \
   --volume jenkins-data:/var/jenkins_home \
   --publish 2376:2376 \
   docker:dind --storage-driver overlay2
5) docker build -t myjenkins-for-study:jdk25 -f Dockerfile .
6) docker run --name jenkins-study-jdk25 \
--restart=on-failure \
--detach \
--network jenkins \
--env DOCKER_HOST=tcp://docker:2376 \ 
--env DOCKER_CERT_PATH=/certs/client \
--env DOCKER_TLS_VERIFY=1 \
--publish 8080:8080 \ 
--publish 50000:50000 \
--volume jenkins-data:/var/jenkins_home \
--volume jenkins-docker-certs:/certs/client:ro myjenkins-for-study:jdk25
7) 