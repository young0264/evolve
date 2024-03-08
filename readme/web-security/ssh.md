# SSH

> 참고 \_ ssh 명칭부터 접속(1,2)
>
> (1) [https://library.gabia.com/contents/infrahosting/9002](https://library.gabia.com/contents/infrahosting/9002)
>
> (2) [https://library.gabia.com/contents/9008](https://library.gabia.com/contents/9008)

* 원격 호스트에 접속하기 위해 사용되는 보안 프로토콜
* 기존(telnet)방식은 암호화를 제공하지 않아 보안상 취약 (패킷 분석 프로그램을 이용하면 데이터 탈취가 가능했었음) → 암호화하는 기술인 SSH 등장
* key 방식을 통해 연결 **상대를 인증**한 후 데이터를 주고 받음



*   비대칭키(public key)

    * 서버 또는 사용자가 Key Pair(키 페어, 키 쌍)를 생성함
    * 공개키(id\_rsa.pub)와 개인키(id\_rsa)를 사용
    * key pair 쌍으로 만들어진 key로 인증을 해야함. (공개키에 대응되는 개인키를 소유하고 있으면 ‘내가 아는 사용자가 맞다!’ 라는 식의 인증/판단이 되어 접속을 허용해 줌)
    * 개인키는 유출이 되면 안됨.(사용자가 보관)
    * aws는 pem key를 사용해 접속(ex : ssh -i /Users/young/cicd-project-key.pem2 [ec2-user@3.104.77.14](mailto:ec2-user@3.104.77.14) )
    * 공개키를 다른 서버에 전송함. 다른 서버의 ‘authorized\_keys’ 파일에 기록 **authorized\_keys : ssh 공개키를 저장하는 파일**

    (aws에서는 키 페어를 따로 생성하는 기능이 있다. ec2에서는 ssh 접속할 때 쓸 키페어를 인스턴스 생성 시에 설정한다. → pem key(private key)로 접속 )

    ```bash
    # 키페어 생성
    $ ssh-keygen

    # ~/.ssh 경로에 authorized_keys  id_rsa  id_rsa.pub  known_hosts 파일 생성
    # ssh {서버 사용자 id}@{서버 ip 주소}
    ```



* 대칭키(private key)
  * 정보를 주고 받는 과정에 정보가 새어나가지 않기 위해 정보를 암호화해서 주고받는데 이때 사용하는게 대칭키 방식
  * 한개의 (대칭)키만 사용하여 정보를 주고 받음
  * 사용자와 서버는 하나의 대칭 키를 만들어 서로 공유함
  * 정보 교환이 완료되면 교환 당시 썼던 대칭키는 폐기, 새로 접속할 때 마다 새로운 대칭 키를 생성하여 사용
