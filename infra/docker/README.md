# Docker

<figure><img src="../../.gitbook/assets/스크린샷 2023-12-16 21.53.16.png" alt=""><figcaption></figcaption></figure>

*   **What is Docker?**

    Docker는 애플리케이션을 신속하게 구축, 테스트 및 배포할 수 있는 소프트웨어 플랫폼입니다. Docker는 컨테이너라는 표준화된 유닛으로 패키징하며, 이 컨테이너에는 라이브러리, 시스템 도구, 코드, 런타임 등 소프트웨어를 실행하는 데 필요한 모든 것이 포함되어 있습니다. Docker를 사용하면 환경에 구애받지 않아 애플리케이션과 개발환경에 의존할 필요가 없어 느슨한 환경을 구축할 수 있습니다. 작업하는 동안 컨테이너를 공유할 수 있으며 신속하게 배포 및 확장할 수 있습니다.
*   **What is DockerFile?**

    모든 Docker 컨테이너는 Docker 컨테이너 이미지의 빌드 방법에 관한 지시사항이 포함된 단순 텍스트 파일로 시작합니다. DockerFile은 Docker 이미지 생성 프로세스를 자동화합니다. 실제로는 Docker 엔진에서 이미지를 조합하기 위해 실행할 명령행 인터페이스(CLI) 명령어의 목록입니다. 이 Docker 명령어 목록은 방대하지만 표준화 되어있습니다. 즉 Docker 작업은 어플리케이션, 인프라, 기타 환경변수에 상관없이 똑같은 방식으로 이루어집니다.
*   **What is Container?**

    하드웨어 가상화를 제공하는 VM과 달리 컨테이너는 '사용자 공간'을 추상화함으로써 경량의 운영체제 수준의 가상화를 제공합니다. 컨테이너는 호스트 시스템의 커널을 다른 컨테이너와 공유합니다. 호스트 운영체제에서 실행되는 컨테이너는 모든 종속성을 패키지화하여 애플리케이션이 한 환경에서 다른 환경으로 빠르고 안정적으로 실행될 수 있게 해주는 표준 소프트웨어 장치입니다. 컨테이너는 영구적이지 않으며 이미지로부터 생성됩니다.

    컨테이너는 애플리케이션이 한 컴퓨팅 환경에서 다른 컴퓨팅 환경으로 빠르고 안정적으로 실행될 수 있도록 코드와 모든 종속성을 패키지화하는 소프트웨어의 표준 단위입니다. Docker 컨테이너 이미지는 코드, 런타임, 시스템 도구, 시스템 라이브러리 및 설정 등 애플리케이션을 실행하는 데 필요한 모든 것을 포함하는 경량의 독립형 실행 가능 소프트웨어 패키지입니다.

    컨테이너 이미지는 런타임 시 컨테이너가 되며, Docker 컨테이너의 경우 이미지는 Docker 엔진에서 실행될 때 컨테이너가 됩니다. Linux 및 Windows 기반 애플리케이션에서 모두 사용할 수 있는 컨테이너화 된 소프트웨어는 인프라에 관계없이 항상 동일하게 실행됩니다. 컨테이너는 소프트웨어를 해당 환경에서 격리하고 소프트웨어가 균일하게 작동하도록 보장합니다.&#x20;
*   **What is Docker Engine?**

    컨테이너를 구축 및 실행하는 오픈 소스 호스트 소프트웨어입니다. Docker Engine은 Oracle Linux, CentOS, Debian, Fedora, RHEL, SUSE, Ubuntu 등 다양한 Windows 서버 및 Linux 운영체제에서 컨테이너를 지원하는 클라이언트 서버 애플리케이션의 역할을 합니다.
*   **What is docker Image?**

    컨테이너로 실행될 소프트웨어 모음입니다. 이미지는 변경할 수 없으며 이미지를 변경하려면 새로운 이미지를 생성해야 합니다.&#x20;
*   **What is Docker Registry?**

    이미지를 저장 및 다운로드 할 수 있는 공간입니다. 레지스트리는 무상태성을 갖춘 확장 가능한 서버측 애플리케이션으로 Docker 이미지를 저장 및 배포합니다.
*   **What is Docker Build?**

    Docker Build는 Docker Engine에서 가장 많이 사용되는 기능 중 하나입니다. Docker build 를 사용해 이미지를 생성합니다.

    docker build 명령어는 Dockerfile과 "context"로부터 Docker image를 가져와 빌드합니다.



    ```console
    docker build [OPTIONS] PATH | URL | -
    ```



    기본적으로 docker build 명령은 루트에서 Dockerfile을 찾습니다. 빌드 컨텍스트의 -f, --file 옵션을 사용하면 특정 경로를 지정할 수 있습니다. 이 옵션은 같은 파일 더미를 여러개의 빌드에서 사용할 때 유용합니다.

    * [https://docs.docker.com/engine/reference/commandline/build](https://docs.docker.com/engine/reference/commandline/build/)

    \
    위의 링크를 통해 docker build에 대한 자세한 정보를 얻어볼 수 있습니다.
*   **What is Docker Run?**

    docker run 명령어는 이미지를 당겨와서 새로운 컨테이너를 실행하는 명령어입니다.  docker start 명령어를 통해 중지되어 있는 컨테이너를 재시작 할 수 있습니다. docker ps -a 명령어를 통해 정지된 컨테이너를 포함해 모든 컨테이너를 볼 수 있습니다.



