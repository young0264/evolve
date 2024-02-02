---
description: maven, jenkins, github(webhook), ngrok(local) 빌드 배포 과정 .(jenkins 설치 이후의 과정)
---

# Settings jenkins CI/CD environment on a local PC

1. **jenkins 설치 및 github integration plugin 설치**
   *   한가지 의문은 기본 Github plugin에 integrates `Github to Jenkins`라고 적혀있다. ‘github integration plugin’를 따로 설치하라는 글들을 많이 봤는데 정작 본인은 설치하지 않고 webhook으로 코드를 polling해오고 있다. (설치 안해도 통합 잘 됨)

       <figure><img src="../.gitbook/assets/Untitled (2).png" alt=""><figcaption></figcaption></figure>

       <figure><img src="../.gitbook/assets/Untitled (3).png" alt=""><figcaption></figcaption></figure>



2. **좌측탭에 ‘+ 새로운 Item’ 을 눌러서 Freestyle project로 item 이름을 정해주고 생성.**

<figure><img src="../.gitbook/assets/Untitled (4).png" alt="" width="563"><figcaption></figcaption></figure>

3.  **item 생성 후, 프로젝트에 해당하는 github 주소를 적는다.**

    *   만약에 private repository라면 ‘소스 코드 관리’의 Git을 체크 후 credential을 설정해줘야한다.

        <figure><img src="../.gitbook/assets/Untitled (5).png" alt="" width="563"><figcaption></figcaption></figure>

        <figure><img src="../.gitbook/assets/Untitled (6).png" alt=""><figcaption></figcaption></figure>



**3-1. (private repository 인 경우) credential 생성**

* credential `+add` 버튼을 누르면 아래와 같은 창이 뜬다.
* Kind : Username with password
* Username : github login Id
* password : github token
* ID : jenkins 내부에서 사용하여 보여질 credential 식별 이름 (자유작성)

<figure><img src="../.gitbook/assets/Untitled (7).png" alt=""><figcaption></figcaption></figure>

**3-2. (private 경우) `**github-token 생성**` 방법**

* **github main page - settings - Developer settings - personal access tokens - Tokens(classic) - Generate new token**
* 토큰 이름과 만료기간을 설정해준다.
*   ‘**`Select scopes`’ 토큰 권한을 주는 부분이다. 아래의 세 부분에 체크해주자.**

    * repo (private repository)
    * admin:org (org,team full control)
    * admin:repo\_hook (repository hooks)

    <figure><img src="../.gitbook/assets/Untitled (9).png" alt="" width="563"><figcaption></figcaption></figure>
*   Branches to build

    레포지토리의 default 브랜치 설정

    <figure><img src="../.gitbook/assets/Untitled (10).png" alt="" width="362"><figcaption></figcaption></figure>

4.  **Jekins로 돌아와서**

    Jenkins 관리 - System 을 클릭 후 ‘Github’ 란을 찾는다.

    이번에는 item 생성시와는 조금 다르다.\
    \- Kind : Secret text\
    \- Secret : 생성한 github token\
    \- ID : github login ID

    <figure><img src="../.gitbook/assets/Untitled (11).png" alt=""><figcaption></figcaption></figure>
5.  **github으로 돌아가서**

    **github - 해당 repository - settings - (좌측탭) Webhooks - Add webhook 버튼 클릭**\
    **-** Payload URL : 로컬주소/github-webhook/\
    \- Content type : applicatioin/json 클릭

```
**주의

- <http://localhost:7777/github-webhook/> 과 같이 localhost로는 설정이 불가능함.
- 외부에서 특정 주소를 통해 local로 접속할 수 있도록 설정이 필요
- ngrok을 사용하여 로컬서버를 접근 가능하게 하자.
참고 : <https://blog.outsider.ne.kr/1159**>
```

<figure><img src="../.gitbook/assets/Untitled (12).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Untitled (13).png" alt=""><figcaption></figcaption></figure>

→ Forwarding 탭을 보면 해당 localhost:port에 접근가능한 외부 주소가 보인다.





6.  \[Jenkins] Dashboard - Jenkins 관리 - Tools

    gradle 혹은 maven 선택 후 버전 설정



7.  \[Jenkins] Dashboard - Item선택 - Configure(구성)\
    ![](<../.gitbook/assets/Untitled (16).png>)



    \- Build Steps에서 Invoke (gradle or maven) 선택 후 build 작성.(아래는 maven 기준)



    <figure><img src="../.gitbook/assets/Untitled (15).png" alt=""><figcaption></figcaption></figure>

***

여기까지 하면 프로젝트에서 github으로 push하면 jenkins에서 build 까지 성공되는 걸 볼 수 있을 것이다.

8.  \[Jenkins]

    \- 'post build task’ 플러그인 설치. - 빌드 후 할 작업(배포)

    \- Dashboard - item클릭 - configuration(구성)

    `- 빌드 후 조치` 에서 local 경로를 바라보는 script를 작성하면 끝.! 프로젝트가 실행되는 것을 볼 수 있을 것 이다. ex) java -jar { ‘.jenkins’ 폴더로 빌드되는 jar파일 경로}

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3867e0bf-d699-47e9-9762-be3836599de7/fa5b1242-b416-4005-9ade-46b069fdd6b7/Untitled.png)

