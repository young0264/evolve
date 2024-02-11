# Docker Commands

```docker
docker run -p 8080:8080 -p 50000:50000 -v /your/home:/var/jenkins_home jenkins

//docker image로 jenkins 실행
docker run -d --name jenkins-server -p 8080:8080 -p 50000:50000 -v /Users/young/docker/var/jenkins_home jenkins/jenkins

```

* jenkins:lts-jdk11 (lts-jdk11 : tag의 이름)
* \-v (volume) : 내가 사용중인 pc에서의 어떤 디렉터리하고 도커 내부에 있는 디렉터리하고 마운트(연결) 작업을 할건지에 대한 설정.
  * 마운트 작업을 하지 않으면 docker내부에서 발생한 데이터는 docker가 삭제되면 해당 데이터가 같이 없어짐.
* \-d (detach) : 실행하는 console/terminal과 분리해서 demon형태로 기동하겠다라는 뜻.
* \-p : -p \[호스트 포트]/\[컨테이너 포트]
* \-f : 도커파일을 알아서 찾지만, 명시적으로 나타내려하면 -f 적어줌.
* ‘ .’ : 현재 디렉터리



*   도커 컨테이너 내 파일 확인

    ```docker
    docker exec [옵션] [컨테이너 이름 또는 ID] [실행할 명령]
    ex) docker exec <CONTAINER_NAME> cat /var/jenkins_home/secrets/initialAdminPassword
    ```



*   docker server 접속 (docker에서 기동되는 jenkins 내부 파일)

    ```docker
    docker exec -it {container name} bash
    ex) docker exec -it jenkins-server bash
    ```



*   docker build (docker image 생성)

    ```bash
    docker build -t docker-server -f Dockerfile .
    docker run -p 8080:8080 --name mytomcat docker-server:latest
    ```



*   docker inspect : 해당 이미지가 가지고 있는 세부내용을 볼 수 있음

    ```bash
    ex) docker image inspect cicd-project:latest
    ```



* systemctl status docker : docker 상태 확인하기
* systemctl enable docker : docker 작동으로 만들기
* systemctl start docker : docker 시작하기
* docker network inspect bridge : 현재 시스템에서 실행 중인 Docker 컨테이너를 위한 기본 네트워크인 Docker 브릿지 네트워크의 구성 및 설정에 대한 정보를 제공
* hostname -i : container ip 확인

