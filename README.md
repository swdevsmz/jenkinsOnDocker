# jenkinsOnDocker

docker-compose.yml
~~~
version: '3'
services:
  jenkins:
    container_name: jenkins_master
    image: jenkins:latest
    volumes:
      - ./jenkins-data:/var/jenkins_home
    ports:
      - "8080:8080"
    restart: always
 ~~~
