---
coverY: 0
---

# Quick Started(with Docker)

<figure><img src="../../../.gitbook/assets/스크린샷 2023-12-16 02.47.10.png" alt=""><figcaption></figcaption></figure>





```
version: '3'

services:

  reverse-proxy:
    # The official v2 Traefik docker image
    image: traefik:v2.10
    # Enables the web UI and tells Traefik to listen to docker
    command:
      - "--api.insecure=true"
      - "--providers.docker"

    ports:
      # The HTTP port
      - 80:80
      - 443:443
      # The Web UI (enabled by --api.insecure=true)
      - 8080:8080
    volumes:
      # So that Traefik can listen to the Docker events
      - /var/run/docker.sock:/var/run/docker.sock

  whoami:
    # A container that exposes an API to show its IP address
    image: traefik/whoami
    labels:
      - "traefik.http.routers.whoami.rule=Host(`whoami.docker.localhost`)"

```

docker-compose.yml 파일



```shell
docker-compose up -d reverse-proxy
docker-compose up -d whoami
```

'reverse-proxy'와 'whoami' 를 시작\


```shell
docker-compose up -d --scale whoami=2
```

**스케일링**이란? 동일한 서비스를 여러 복제본으로 확장하는 것을 의미함.&#x20;

아래의 명령어는 whoami 라는 서비스가 2개의 독립된 컨테이너로 실행되도록 Docker Compose에게 지시하는 것!



```shell
curl -H Host:whoami.docker.localhost http://127.0.0.1
```

curl 명령어를 통해 'whoami.docker.localhost'라는 이름의 host name을localhost인 127.0.0.1로 요청을 보내는 명령어 입니다. (테스트용) proxy를 통해 header의 host정보에 'whoami.docker.localhost' 로 설정하여 요청을 보내게 됩니다.

즉  'whoami.docker.localhost'라는 이름으로 host name을 설정해 놓은 whoami 서비스를 2개의 독립된 컨테이너로 스케일링 합니다. 그리고 'whoami.docker.localhost'라는 host name을 localhost(127.0.0.1)로 요청을 보내어 서버가 실행중이라면 응답이 오게 됩니다.



<figure><img src="../../../.gitbook/assets/스크린샷 2023-12-16 03.25.47.png" alt=""><figcaption><p>docker desktop</p></figcaption></figure>

whoami 서비스가 -1, -2로 두개로 스케일링 되어 동작중인 것을 확인할 수 있습니다.



<div>

<figure><img src="../../../.gitbook/assets/스크린샷 2023-12-16 03.23.34.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/스크린샷 2023-12-16 03.23.28.png" alt=""><figcaption></figcaption></figure>

</div>

위의 과정을 거쳐서 Traefik이 서비스의 두 인스턴스간에 부하 분산을 수행하는지 확인할 수 있습니다. 127.0.0.1의 요청을 보내면 도커 컨테이너가 내부적으로 할당받는 IP주소인 127.21.0.4와 127.21.0.2로 서로 다른 IP주소로 라우팅 되는 것을 볼 수 있었습니다.



\| 참고 : [https://doc.traefik.io/traefik/getting-started/quick-start](https://doc.traefik.io/traefik/getting-started/quick-start/)\








