---
description: (240907)
---

# Core Concepts



*   DaemonSet

    DaemonSet은 Kubernetes에서 모든 노드(혹은 특정 노드)에 대해 하나의 파드가 실행되도록 보장하는 컨트롤러입니다. 이를 통해 특정 작업이나 서비스를 클러스터의 모든 노드에 일관되게 배포할 수 있습니다.

    DaemonSet의 주요 역할과 특징은 다음과 같습니다

    * `--ignore-daemonsets` 옵션은 `kubectl drain` 명령어에서 사용하는 옵션으로, **DaemonSet**에 의해 관리되는 파드를 제외하고 노드를 드레인할 때 사용됨
* version
  * `k version`
  * `k get nodes` → 버전보기
* configmap ?
  * Kubernetes에서 ConfigMap은 애플리케이션의 설정 데이터를 저장하고 관리하는 리소스입니다. ConfigMap을 사용하면 애플리케이션의 설정을 코드와 분리하여 독립적으로 관리할 수 있으며, 설정을 업데이트할 때 애플리케이션의 재배포 없이도 변경할 수 있습니다.
*   `node ? + cordon, uncordon, drain`

    Kubernetes에서 \*\*노드(Node)\*\*는 클러스터에서 실제로 **애플리케이션을 실행**하는 물리적이거나 가상적인 서버를 의미합니다. 노드는 클러스터의 일부분으로서, **Pod**를 호스팅하고 애플리케이션 컨테이너가 실제로 배포되고 실행되는 장소입니다.

    * pod에 노드 내리기
      * 노드에서 실행 중인 모든 Pod을 안전하게 종료하고, 가능한 경우 다른 노드로 이동시킵니다.
      * `k drain node01`
    * Kubernetes에서 특정 노드를 **스케줄링 대상에서 제외**하는 데 사용
      * 특정 노드를 새로운 Pod의 스케줄링 대상에서 제외
      * `kubectl cordon node01`
    *
      * `k uncordon node01`
    *
      * `drain` 명령어는 기본적으로 `--ignore-daemonsets` 및 `--delete-local-data` 옵션을 통해 DaemonSet에 의해 관리되지 않는 Pod과 로컬 데이터를 가진 Pod을 삭제할 수 있습니다.
* `pod`
  *   env가 dev만 가져오기

      ```jsx
      k get pods --selector env=dev
      ```
  * pod 내 내용 출력
    * `k -n elastic-stack exec -it app -- cat /log/app.log`
*   `kubectl run`

    명령어는 Kubernetes에서 임시로 파드(Pod)를 생성하고 실행하는 데 사용됨

    ```jsx
    // 라벨 label 실행방법
    k run redis -l tier=db  --image=redis:alpine
    ```
*   `kubectl expose`

    `kubectl expose` 명령어는 Kubernetes의 서비스(Service) 객체를 생성합니다. 서비스는 파드와 같은 리소스에 안정적인 네트워크 IP 주소와 포트를 제공하여, 클러스터 내외부에서 트래픽을 전달할 수 있도록 합니다. 서비스는 클러스터가 동적으로 변하는 환경에서도 파드에 일관된 접근 방식을 제공합니다.
* replicaset
  * Kubernetes(K8s)에서 **ReplicaSet**의 역할은 특정 수의 \*\*파드(Pod)\*\*가 항상 실행되도록 보장하는 것입니다. `ReplicaSet`은 파드 복제를 관리하고, 지정된 수의 파드를 유지하기 위해 파드를 자동으로 생성하거나 삭제합니다.
  *   replica-set 버전보기

      ```jsx
      k explain rs
      ```
  *   replicaset. yaml 파일 실행시키기

      ```jsx
      kubectl apply -f /root/replicaset-definition-1.yaml
      ```

      ```jsx
      Run the command: You can check for apiVersion of replicaset by command kubectl api-resources | grep replicaset

      kubectl explain replicaset | grep VERSION and correct the apiVersion for ReplicaSet.

      Then run the command: kubectl create -f /root/replicaset-definition-1.yaml
      ```
* deployments
  * Kubernetes에서 **Deployments**는 애플리케이션을 배포하고 관리하는 중요한 컨트롤러입니다. Deployment는 파드(Pod)의 복제, 배포, 업데이트, 롤백을 자동으로 관리하며, 애플리케이션의 상태를 정의한 대로 유지하도록 보장합니다.
  * k get deploy
  * k get deployment \~\~
  * k create deployment —help
* services
  * Kubernetes에서 **Service**는 클러스터 내에서 파드(Pod)에 대한 네트워크 접근을 추상화하는 리소스입니다. Service는 파드의 접근을 관리하고, 클러스터 내에서 안정적이고 일관된 네트워크 통신을 제공하기 위해 사용됩니다. 주요 역할과 특징은 다음과 같습니다
  * k get svc
  *   target port finding 보기

      ```jsx
      k describe svc
      ```
  *   service를 파일로 뽑아낸 후 작업

      ```jsx
      k get svc redis -o yaml > service-def.yaml
      ```
* **service account**
  * 쿠버네티스에서 Pod와 같은 워크로드가 API 서버와 상호작용할 때 사용하는 계정입니다. 기본적으로 쿠버네티스의 Pod는 어떤 권한도 없이 실행되지만, 필요한 경우 API 호출 등의 작업을 하기 위해 ServiceAccount를 사용하여 권한을 부여할 수 있음
*   **PersistentVolume**

    #### **PersistentVolume (PV)**

    **PersistentVolume**은 쿠버네티스 클러스터에서 사용할 수 있는 스토리지 리소스를 정의한 객체입니다. PV는 노드와 독립적으로 존재하며, 클러스터 내의 모든 Pod에서 접근 가능한 스토리지를 제공합니다.

    * **기본적인 역할**:
      * 클러스터 내에서 외부 스토리지를 추상화하여 제공.
      * 스토리지의 할당, 사용, 해제 등을 관리.
      * `PersistentVolumeClaim`에 의해 요청된 스토리지 리소스를 제공.
*   **ClusterRole**

    **ClusterRole**은 쿠버네티스 클러스터 내에서 특정 리소스에 대해 어떤 작업(예: 읽기, 쓰기 등)을 할 수 있는지를 정의하는 역할입니다. 일반적인 `Role`과 달리, **ClusterRole**은 클러스터 전체에서 동작하며, 네임스페이스 경계에 상관없이 리소스에 접근 권한을 부여할 수 있습니다.

    * **기본적인 역할**:
      * 클러스터 전체에서 리소스(예: PersistentVolume, Node 등)에 대해 권한 부여.
      * 클러스터 관리나 네임스페이스 간의 리소스 접근을 제어.
* **RoleBinding**
  * **RoleBinding**은 Kubernetes에서 **Role** 또는 **ClusterRole**을 사용자나 그룹, 서비스 계정에 바인딩하여 특정 리소스에 대한 접근 권한을 부여하는 리소스입니다. Kubernetes에서 권한 관리는 RBAC(Role-Based Access Control) 시스템을 사용하며, **RoleBinding**은 그 중요한 구성 요소 중 하나입니다.
*   **ClusterRoleBinding**

    **ClusterRoleBinding**은 **ClusterRole**과 사용자를 연결하는 역할을 합니다. 즉, 특정 **ClusterRole**을 특정 사용자(또는 ServiceAccount)에 바인딩하여, 그 사용자가 해당 역할에 정의된 권한을 사용할 수 있게 합니다.

    * **기본적인 역할**:
      * 특정 **ClusterRole**을 서비스 어카운트나 사용자에게 연결하여 권한 부여.
      * 예를 들어, **pvviewer** 서비스 어카운트가 PersistentVolume을 읽을 수 있도록 설정.
* **RoleBinding과 ClusterRoleBinding의 차이**
  * **RoleBinding:** 특정 네임스페이스 내에서만 권한을 부여합니다. 즉, 네임스페이스 범위의 권한을 정의할 때 사용됩니다.
  * **ClusterRoleBinding:** 클러스터 전역에서 권한을 부여합니다. 네임스페이스에 상관없이 모든 리소스에 접근할 수 있는 권한을 설정할 때 사용합니다.
* **Practice Test - Imperative Commands**
  * 마지막문제 어렵네
