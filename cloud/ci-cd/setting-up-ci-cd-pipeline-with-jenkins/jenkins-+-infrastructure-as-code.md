# Jenkins + Infrastructure as Code

CI/CD 도구인 Jenkins와 IaC 라는 프로비저닝 도구와 연동에 대한 학습

Ansible : IaC 도구인 Ansible 설치 및 사용 Ansible Playbook : Ansible에서 실행할 수 있는 명령어의 집합 script 파일. Docker 이미지 배포

>

#### Infrastructure as Code

* 시스템, 하드웨어 또는 인터페이스의 구성 정보를 파일(스크립트)을 통해 관리 및 프로비저닝
* IT 인프라 스트럭처, 베어 메탈 서버 등의 물리 장비 및 가상 머신과 관련된 구성 리소스를 관리
* 버전 관리를 통한 리소스 관리

#### **IaC 예시**

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

* Pulumi
* Terraform
  * 인프라 구축하는데에 많이 사용
  * 자체 DSL 사용
*   ANSIBLE\
    ![](<../../../.gitbook/assets/image (3).png>)

    * 이미 구축되어 있는 서버들에 구성정보를 변경 / 관리 하는데에 많이 사용
    * 여러 개의 서버를 효율적으로 관리할 수 있게 해주는 환경 구성 자동화 도구
      * Configuration Management, Deployment & Orchestration tool
      * IT infrastructure 자동화
    * Push 기반 서비스 : python의 ssh 프로토콜을 통해 데이터를 주고 받음
    * Simple, Agentless 한 특징
    * 할 수 있는 작업들
      * 설치 : apt-get, yum, homebrew
      * 파일 및 스크립트 배포 : copy
      * 다운로드 : get\_url, git
      * 실행 : shell, task
    * AWS 하고 연동 잘 됨
    * Ansible Server 설치 (Linux)
      * $ yum install ansible
      * $ ansible --version
    * 환경 설정 파일 → /etc/ansible/ansible.cfg
    * Ansible에서 접속하는 호스트 목록 → /etc/ansible/hosts

    ***

    ```java
    Apple silicon : $ docker pull edowon0623/ansible-server:m1

    // '\\' : 명령어가 끊기지 않고 한줄로 입력됨
    // 'privileged' : 관리자 권한으로 실행
    $ docker run --privileged \\
    		--name ansible-server -itd \\
    		-p 20022:22 -p 8081:8080 \\
    		-e container=docker \\
    		-v /sys/fs/cgroup:/sys/fs/cgroup --cgroupns=host \\
    		edowon0623/ansible-server:m1 /usr/sbin/init

    $ ssh root@localhost -p 20022 
    	(or docker exec -it ansible-server bash)

    ```
