---
description: Overall cloud terminology description
---

# Cloud

* **가상화(Virtualization)란?**
  * server, storage, network 와 여러 물리적 시스템에 대해서 가상표현을 생성하여 사용하는 기술입니다. 하나의 물리적 머신에서 여러 가상 시스템을 동시에 실행할 수 있습니다. 이로써 리소스를 효율적으로 사용할 수 있는 장점이 있습니다.
* **클라우드(Cloud)란?**
  * 인터넷 네트워크를 통해 접근할 수 있는 서버와 서버에서 동작하는 program, application, database등을 제공하는 IT 환경을 의미합니다. 인터넷을 통해 컴퓨팅 자원을 손쉽게 이용 가능한 장점이 있습니다.
* **클라우드 네이티브(Cloud Native)란?**
  * 클라우드 컴퓨팅 모델의 이점을 활용하는 애플리케이션 구축 방법론을 말함.
  * 클라우드 네이티브의 4가지 구성요소 :
    * **Container** : 컨테이너 환경을 통한 개발의 유연성
    * **MSA** : micro service architecture를 통한 서비스 안정성 증대
    * **CI/CD** : 개발-운영간 업무 속도 증가(시각화, 자동화, 프로세스 단순화)
    * **Devops** : App 서비스 개선 속도 증가
* **CNCF(Cloud Native Computing Foundation)란?**
  * 리눅스 재단 소속의 비영리 단체.
* **클라우드 가상화 기술(Three types of virtualization)**
  * 서버 가상화(Server)
    * 물리 서버를 여러대의 가상 서버로 분할하는 프로세스로 서버 리소스를 효율적으로 사용할 수 있어 경제적입니다. 가상화된 서버를 사용하지 않고 물리 서버를 사용하면 처리 용량만 사용하고 나머지는 사용하지 않는 상태로 남겨지는 단점이 있습니다. ex) VMwar, VirtualBox
  * 네트워크 가상화(Network)
    * 네트워크 리소스를 결합하여 관리 태스크를 중앙 집중화하는 프로세스로 물리적 구성 요소를 건드리지 않고 가상화하여 조정 및 제어할 수 있어 네트워크 관리가 간소화됩니다. ex)VPN, VLAN
  * 스토리지 가상화 (Storage)
    * 네트워크 연결 스토리지(NAS)와 스토리지 영역 네트워크(SAN)와 같은 물리적 스토리지 디바이스의 기능을 결합합니다. 즉 물리적 디스크 드라이브를 논리적으로 mapping시키는 것을 의미합니다. 리소스 풀링의 이점이 있다.
* **클라우드의 종류 (Three main cloud models)**
  * 퍼블릭 클라우드(public cloud)
    * **인터넷을 통해 누구나 접근할 수 있는 클라우드 컴퓨팅의 형태.** 일반 대중, 개인, 기업 등에서 사용하기 위한 클라우드 인프라를 말함.
  * 프라이빗 클라우드(private cloud)
    * 제한된 공간, 사용자만이 접근 가능한 컴퓨팅 환경으로 방화벽으로 보호됨. 흔히 기업들에서 주로 사용하는 방
  * 멀티 클라우드(multi cloud)
    * 여러 클라우드 공급업체들의 서비스를 동시에 이용하는 컴퓨팅 환경
  * 하이브리드 클라우드(hybrid cloud)
    * 퍼블릭과 프라이빗을 결합하 **서로 다른 클라우드 환경 간에 데이터와 애플리케이션을 공유할 수 있는 클라우드 컴퓨팅 환경**이다.
*   **클라우드 서비스의 종류 3가지(Cloud Solutions)**

    * IaaS(Infrastructure as a Service) : 인프라 구축만을 제공하는 서비스
      * 서버와 저장소 등 인프라 서비스를 제공하는 빈 방을 내어주는 형태로 클라우드에 종속되지 않고 물리적 환경의 비용이 들지 않기에 운영비 절감의 장점이 있음.
    * PaaS(Platform as a Service) : 플랫폼을 제공하여 인프라를 구축하는 서비스
      * 설치가 쉬운 반면 버전선택등의 제약때문에 확장과 이식성이 떨어지며 platform이라는 서비스를 추가로 받기에 비용이 상대적으로 더 비쌈
    * SaaS(Software as a Service) : 사용하는 어플리케이션 및 인프라 모두를 제공하는 서비스
      * 프로그램 및 인프라, 플랫폼을 사용자에게 제공하는 클라우드 컴퓨팅 형태

    <figure><img src="../.gitbook/assets/스크린샷 2023-11-10 13.40.06.png" alt=""><figcaption></figcaption></figure>
