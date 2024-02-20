# Ansible

*   ansible 서버에서 다른 서버로 접속시 비밀번호 없이 접속



    ```java
    ssh-keygen
    ssh-copy-id root@{비밀번호 없이 접속하고자 하는 ip}
    ```

    <figure><img src="../../../.gitbook/assets/Untitled (17).png" alt="" width="563"><figcaption></figcaption></figure>
* Ansible 명령어
  *   실행옵션

      ```bash
      -i (--inventory-file) : 적용 될 호스트들에 대한 파일 정보
      -m (--module-name) : 모듈 선택
      -k (--ask-pass) : 관리자 암호 요청
      -K (--ask-become-pass) : 관리자 권한 상승
      --list-hosts : 적용되는 호스트 목록
      ```
  * 멱등성 : 같은 설정을 여러번 적용하더라도 결과가 달라지지 않는 성질
*   Ansible 모듈 + Playbook

    Ansible 서버에서 한번만 실행하면, 동일한 target 서버에 실행/제어할 수 있다는 특징

    단순한 설치, 환경설정, 상태값 부여 모두 가능해짐

    > [https://docs.ansible.com/ansible/2.9/modules/list\_of\_all\_modules.html](https://docs.ansible.com/ansible/2.9/modules/list\_of\_all\_modules.html)

    #### Ansible Test

    *   ansible 서버와의 연결상태 확인하기

        ```bash

        cat /etc/ansible/hosts
        #ex)
        #[devops]
        #172.17.0.2
        #172.17.0.3

        #devops 그룹만 실행
        # $ ansible devops 

        $ ansible all -m ping
        ```
    *   ansible로 연결된 서버 메모리 확인하기

        ```bash
        ansible all -m shell -a "free -h"
        ```
    *   ansible 을 이용한 파일 복사

        ```bash
        ansible all -m copy -a "src=./test.txt dest=/tmp"
        ```
    *   centOS , 설치된 패키지 있는지 확인

        ```bash
        yum list installed | grep httpd
        ```
    *   ansible 을 이용한 devops 그룹명에 yum으로 httpd를 지금 바로 다운로드

        ```bash
        ansible devops -m yum -a "name=httpd state=present"
        ```
    *   Ansible Playbook

        * Ansible Playbook은 Ansible을 사용하여 원격 시스템에 대한 자동화 작업을 정의하는 YAML 파일
          * 시스템설정, 패키지 설치, 파일 전송, 서비스 재시작/관리 등 여러가지 정의
          * 다수의 서버에 반복 작업을 처리하는 경우
        * Playbook
          * $ vi first-playbook.yml 작성
          * $ ansible-playbook first-playbook.yml
          * $ cat /etc/ansible/hosts

        ansible yml 파일 실행 : `ansible-playbook first-playbook.yml`

        *   특정 경로의 특정 파일에 데이터 추가

            ```yaml
            ---
            - name: Add an ansible hosts
              hosts: localhost
              tasks:
                - name: Add an ansible hosts
                  blockinfile:
                    path: /etc/ansible/hosts
                    block: |
                      [mygroup]
                      172.17.0.5
            ```

        [https://downloads.apache.org/tomcat/tomcat-9/v9.0.85/bin/apache-tomcat-9.0.85.tar.gz.sha512](https://downloads.apache.org/tomcat/tomcat-9/v9.0.85/bin/apache-tomcat-9.0.85.tar.gz.sha512)

        *   특정 경로에 현재 ansible서버 디렉토리의 특정 파일을 copy해서 등록

            ```yaml
            - name: Ansible Copy Example Local to Remote
              hosts: devops
              tasks:
                - name: copying file with playboook
                  copy:
                    src: ~/sample.txt
                    dest: /tmp
                    owner: root
                    mode: 0644
            ```
        * 특정 경로에 특정 인터넷 경로 다운로드
        *   전 : ssh copy를 통해서 복사한 후 도커에서 컨테이너를 실행하는 방법

            → 단점 : docker 에 직접적으로 이런 작업을 하면 기존 container 재실행시 문제발생(이미지, 컨테이너 삭제 후 다시 실행)
        *   후 : ansible을 통해서 배포하기. (멱등성)

            * jenkins war파일 → ansible로 복사
            * target을 host파일에 등록
            * 복사 war파일을 제어, jenkins로
            *   하지만 테스트시 delete image, container 후 ansible 실행 해봐야함.

                (두번째 컨테이너는 동작x, 첫번째 컨테이너만 동작하는 경우 발생 가능.)

                container - stop & rm , image - rmi

            ```yaml
            # 전 : 복사
            [root@cf3bc7d6a2c6 ~]# cat Dockerfile
            FROM tomcat:9.0

            LABEL org.opencontainers.image.authors="edowon0623@gmail.com"

            COPY ./hello-world.war /usr/local/tomcat/webapps

            #후 : command에 해당하는 script를 실행하겠다는 의미
            [root@cf3bc7d6a2c6 ~]# cat first-devops-playbook.yml
            - hosts: all
            # become: true

              tasks:
              - name: build a docker image with deployed war file
                command: docker build -t cicd-project-ansible .
                args:
                  chdir: /root
            ```
*   Ansible을 이용한 로그인 관리(24.02.14)

    ```yaml
    # cicd-project-ansible를 뒤의 name으로 변경(docker login-id)
    docker tag cicd-project-ansible ny2485/cicd-project-ansible

    # docker login
    docker login

    #docker push
    docker push ny2485/cicd-project-ansible

    **<https://hub.docker.com/>
    들어가서 login 후 repository를 보면 해당 image가 올라와 있는 걸 볼 수 있음**
    ```

    ```yaml
    [root@cf3bc7d6a2c6 ~]# cat create-cicd-devops-image.yml
    - hosts: all
    # become: true

      tasks:
      - name: build a docker image with deployed war file
        command: docker build -t ny2485/cicd-project-ansible .
        args:
          chdir: /root

      - name: push the image on Docker Hub
        command: docker push ny2485/cicd-project-ansible

      - name: remove the docker image from the ansible server
        command: docker rmi ny2485/cicd-project-ansible
        ignore_errors: yes

    #ansible 실행 -> docker hub보면 새롭게 업데이트 되어있음
    ansible-playbook -i hosts create-cicd-devops-image.yml
    ```

    *   ansible-playbook로 hosts파일을 실행하면서 create-cicd-devops-image.yml 파일을 실행함. 다만 172.17.0.4 실행시 중복 실행(리소스낭비)하니까 limit으로 172.17.0.2 이것만 실행

        ```yaml
        [root@cf3bc7d6a2c6 ~]# cat hosts
        172.17.0.2
        172.17.0.4

        $ ansible-playbook -i hosts create-cicd-devops-image.yml --limit 172.17.0.2

        #실행 중 : 172.17.0.2 이것만 실행
        #이미지 ok, build, push, remove까지 완료

        PLAY [all] ***************************************************************************************************************************************************************

        TASK [Gathering Facts] ***************************************************************************************************************************************************
        ok: [172.17.0.2]

        TASK [build a docker image with deployed war file] ***********************************************************************************************************************
        changed: [172.17.0.2]

        TASK [push the image on Docker Hub] **************************************************************************************************************************************
        changed: [172.17.0.2]

        TASK [remove the docker image from the ansible server] *******************************************************************************************************************
        changed: [172.17.0.2]

        PLAY RECAP ***************************************************************************************************************************************************************
        172.17.0.2                 : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

        # docker-hub을 보면 새로 이미지가 올라옴을 알 수 있음

        ```
    *   새로운 item 생성

        ```yaml
        ansible-playbook -i hosts create-cicd-devops-image.yml --limit 172.17.0.2
        ansible-playbook -i hosts create-cicd-devops-container.yml --limit 172.17.0.4
        ```

    ***

    * 정리
      *   생성, 업로드, 삭제

          <figure><img src="../../../.gitbook/assets/Untitled1 (1).png" alt=""><figcaption></figcaption></figure>
      *   create-cicd-devops-container.yml 은 docker-server에서만 실행했었음.

          * 컨테이너 중지,삭제, image 삭제, docker-hub image download, container image 생성

          <figure><img src="../../../.gitbook/assets/스크린샷 2024-02-14 02.01.53.png" alt=""><figcaption></figcaption></figure>
