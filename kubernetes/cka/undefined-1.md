# 기출 정리

*   링크

    [https://peterica.tistory.com/540](https://peterica.tistory.com/540)
*   기출문제 목록

    ```jsx
    * Pod에서 특정 log만 추출하여 파일로 저장
    * ServiceAccount 생성, Role 생성, Role Binding 생성 후 확인
    * ETCD snapshot save & restore
    * Cluster Upgrade (Controlplane Node만 진행)
    * Pod에 nodeSelector (disktype=ssd) 추가
    * Pod를 생성하고 포트는 80, NodePort는 30020인 서비스를 배포
    * Taint가 없는 Node의 개수를 파일로 저장
    * Node의 상태가 ready 개수를 파일로 저장
    * 사용률이 가장 높은 Pod를 특정 label로만 조회해서 파일로 저장
    * 특정 Node를 Pod를 다른 Node로 Reschedule하고 해당 Node는 SchedulingDisabled
    * 특정 Node가 NotReady 상태인데 Ready가 되도록 TroubleShooting
    * 특정 Deployment에 대해 replicas 수정
    * Ingress를 생성해서 이미 생성 되어 있는 서비스와 연결하고 확인
    * Networkpolicy를 생성해서 특정 namespace의 Pod만 특정 경로로 연결
    * PVC 생성 후 Pod와 PVC 연동 (PV는 이미 존재), PVC 용량 수정
    * 기존에 배포된 Pod에 새로운 Container 추가 (Sidecar 패턴)
    * 이미지 nginx 1.16으로 Deployment 생성 후 이미지를 nginx 1.17로 업그레이드 하기
    ```
*   기출 문제 정리

    `약한것`

    * `Networkpolicy. from, to`
    * Troubleshooting
      * Application Failure
        * get pods, svc, deploy 순서보기.
        * pod logs, describe 상태확인하기
        * pod의 label확인 후 pod와 연결되어있는 service에 selector와 일치하는지 확인
        * mysql pod는 그 자체로 실행하는거고 web-app 같은 곳에서 mysql에 접속(접근)할 땐 그 연결을 위한 변수가 필요하기 때문에 web-app 레벨에서만 env를 설정해줌
          * 하지만 mysql pod에서도 비밀번호 설정은 해야함.
        * service의 node port가 사용자 port와 일치하지 않는 상황이 있을 수 있음.
      * Control Plane Failure
        * pod `All` describe.→ controlplane 쪽 error 확인
          * /etc\~/manifests에서 controleplane에 해당하는 yaml 파일 확인 후 수정
        * deploy의 sacle replica 갯수를 조절해도 실질적으로 pod 갯수에 변화가 없는 경우
        * `get pod -A` 에서 에러를 확인 후 manifests 경로의 파일들에서 오탈자들 확인
        * `(실수)` -n 으로 kube-system 네임스페이스에 있는 pod 로그 잘 찍어보기
      * Worker Node Failure
        * \[1] Fix the broken cluster
          *   에러로그

              ```java
              Events:
                Type     Reason                   Age                    From             Message
                ----     ------                   ----                   ----             -------
                Normal   Starting                 9m14s                  kube-proxy       
                Normal   Starting                 9m17s                  kubelet          Starting kubelet.
                Warning  InvalidDiskCapacity      9m17s                  kubelet          invalid capacity 0 on image filesystem
                Normal   NodeHasSufficientMemory  9m17s (x2 over 9m17s)  kubelet          Node node01 status is now: NodeHasSufficientMemory
                Normal   NodeHasNoDiskPressure    9m17s (x2 over 9m17s)  kubelet          Node node01 status is now: NodeHasNoDiskPressure
                Normal   NodeHasSufficientPID     9m17s (x2 over 9m17s)  kubelet          Node node01 status is now: NodeHasSufficientPID
                Normal   NodeAllocatableEnforced  9m17s                  kubelet          Updated Node Allocatable limit across pods
                Normal   RegisteredNode           9m12s                  node-controller  Node node01 event: Registered Node node01 in Controller
                Normal   NodeReady                9m12s                  kubelet          Node node01 status is now: NodeReady
                Normal   NodeNotReady             87s                    node-controller  Node node01 status is now: NodeNotReady

              ```
          *   Solution

              Step1. Check the status of services on the nodes.

              Step2. Check the service logs using

              ```
              journalctl -u kubelet
              ```

              .

              Step3. If it's stopped then start the stopped services.

              Alternatively, run the command:

              ```
              ssh node01 "service kubelet start"
              ```
          * node의 서비스 상태(status) 확인
          * `journalctl -u kubelet`
          * node01이 멈춰있으니 `ssh node01` 로 접근 후
          * `systemctl status containerd` 로 상태 확인
          * `systemctl status kubelet`로 상태 확인
          * kubelet이 inactive 상태였음. →
          *   `systemctl start kubelet`

              혹은 `service kubelet start` 로 재실행
        * \[2]
          *   실시간으로 로그를 계속해서 출력

              `journalctl -u kubelet -f`
          * kubelet이 클라이언트 CA파일을 잘못 지정해서 파일이 존재하지 않는다라고 나오는 이슈임
          *   `sudo vi /var/lib/kubelet/config.yaml`

              해당 파일로 들어가서 ca 관련한 경로를 수정해야함
        * \[3]
          * 로그에서 fail 지점 확인.
          * /var/lib … config 정보와 아래 정보와 비교(port)
          * `/etc/kubernetes/kubelet.conf`
          *   ca 관련한 경로를 수정

              `sudo vi /var/lib/kubelet/config.yaml`
          *   kubelet 재시작

              `sudo systemctl restart kubelet`
      * Network
        * \[1] weave 설치 문제.
          *   service end point 확인

              `kubectl get services -n triton`
          *   kube-proxy 로그 확인

              `kubectl logs -n kube-system -l k8s-app=kube-proxy`
          *   weave 로그 확인

              `kubectl logs -n kube-system -l name=weave-net`
          *   weave 설치

              `curl -L <https://github.com/weaveworks/weave/releases/download/latest_release/weave-daemonset-k8s-1.11.yaml> | kubectl apply -f -`
        * \[2] configuration conf 경로
          * kube-proxy pods 로그 확인
          * daemonsets 확인
          * configmap 확인
    * Services & Networking
    * Workloads & Scheduling
    *   etcd backup & restore(복기) → 검증까지

        [https://peterica.tistory.com/460](https://peterica.tistory.com/460)

        1. ETCD 를 호스팅할 시스템에 ssh 로그인
        2.  동작중인 etcd 버전과 etcdctl 툴 설치여부 확인

            ```bash
            ~~etcd --version~~
            etcdctl --version
            ```
        3.  **\[etcd backup]**

            ```bash
            ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \\
              --cacert=<trusted-ca-file> --cert=<cert-file> --key=<key-file> \\
              snapshot save <backup-file-location>
            ```

            ***
        4.  **\[etcd restore]**

            ```bash
            export ETCDCTL_API=3 etcdctl --data-dir <data-dir-location> snapshot restore snapshot.db
            ```

            * \<data-dir-location> 은 새로운 디렉터리여야함
            *   그리고 /etc/kubernetes/manifests/etcd.yaml 파일에서

                hostPath: path 에 \<data-dir-location> 를 넣어줌
        5.  \[검증] : etcd 두개가 up인지 확인

            * 아래 docker 명령어로. create가 up상태까지 되는걸 봐야함.

            ```bash
            1. sudo docker ps -a | grep etcd
            2. kubectl get pods
            ```
    *   Node가 NotReady인데 Ready가 되도록(복기\_고배점)

        ```bash
        # 문제 노드 확인
        $ k get node

        # ssh 접속
        $ ssh wk8s-node-0

        # 루트 권한 설정
        $ sudo -i

        # kubelet 서비스 상태 확인
        $ systemctl status kubelet
        ... inactive 상태 확인하기

        # 재기동
        $ systemctl restart kubelet

        # 상태 확인
        $ systemctl status kubelet
        ... active 상태 확인
        ```
    *   Node 상태 “Ready” 갯수 저장 `| wc -l`

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3867e0bf-d699-47e9-9762-be3836599de7/be221b50-601c-45ab-b1ae-2790fd8a7462/image.png)
    *   사용률이 가장 높은 Pod 특정 label로 조회해서 파일로 저장

        ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3867e0bf-d699-47e9-9762-be3836599de7/48172a00-22b8-43bf-890a-4d9db56dbf97/image.png)
    *   \[Resource관련] pvc 생성후 pod와 pvc연동(pv는 이미 존재), pvc 용량 수정.storage(복기) `storageclass, standard`

        * storageClass의 allowVolumeExpansion 필드가 true로 설정도니 경우에만 PVC를 확장할 수 있음.
        * storageclass 조회 후 allowVolumeExpansion 수정
          * `pv` 생성시 해당 storageclass를 설정
          * 그렇게 해야 allowVolumeExpansion가 열려있는 해당 storageclass로 pv를 마운트(?) 하는거니깐.
          * `pvc` 생성시에도 해당 storageclass를 설정

        ```bash
        # storageclasses를 지정하지 않으면 기본을 사용함
        $ k get sc

        # 확장 가능 확인, AllowVolumeExpansion:  True
        $ k describe sc {storageclasses명}
        Name:            standard
        IsDefaultClass:  Yes
        Annotations:     kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"storage.k8s.io/v1","kind":"StorageClass","metadata":{"annotations":{"storageclass.kubernetes.io/is-default-class":"true"},"labels":{"addonmanager.kubernetes.io/mode":"EnsureExists"},"name":"standard"},"provisioner":"k8s.io/minikube-hostpath"}
        ,storageclass.kubernetes.io/is-default-class=true
        Provisioner:           k8s.io/minikube-hostpath
        Parameters:            <none>
        MountOptions:          <none>
        ReclaimPolicy:         Delete
        VolumeBindingMode:     Immediate
        Events:                <none>

        # 기본 storageClass 수정
        # allowVolumeExpansion: true  
        $ k edit sc standard
        ..............
        allowVolumeExpansion: true <=== 추가

        # PV 생성, 공식문서 참조(pods/storage/pv-volume.yaml)
        apiVersion: v1
        kind: PersistentVolume
        metadata:
          name: task-pv-volume
          labels:
            type: local
        spec:
          storageClassName: standard <=== 기본으로 수정
          capacity:
            storage: 10Mi
          accessModes:
            - ReadWriteOnce
          hostPath:
            path: "/mnt/data"
            
        $ k apply -f pv.yaml
        persistentvolume/task-pv-volume created

        # PVC 생성, 공식문서 참조(pods/storage/pv-claim.yaml)
        apiVersion: v1
        kind: PersistentVolumeClaim
        metadata:
          name: task-pv-claim
        spec:
          storageClassName: standard <=== 기본으로 수정
          accessModes:
            - ReadWriteOnce
          resources:
            requests:
              storage: 10Mi <== 수정함

        $ k apply -f pvc.yaml
        persistentvolumeclaim/task-pv-claim created

        # 확인
        $ k get pv,pvc
        NAME                              CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                   STORAGECLASS   REASON   AGE
        persistentvolume/task-pv-volume   10Mi       RWO            Retain           Bound    default/task-pv-claim   manual                  30s

        NAME                                  STATUS   VOLUME           CAPACITY   ACCESS MODES   STORAGECLASS   AGE
        persistentvolumeclaim/task-pv-claim   Bound    task-pv-volume   10Mi       RWO            manual         26s

        # POD 생성, 공식문서 참조(pods/storage/pv-pod.yaml)
        apiVersion: v1
        kind: Pod
        metadata:
          name: task-pv-pod
        spec:
          volumes:
            - name: task-pv-storage
              persistentVolumeClaim:
                claimName: task-pv-claim
          containers:
            - name: task-pv-container
              image: nginx
              ports:
                - containerPort: 80
                  name: "http-server"
              volumeMounts:
                - mountPath: "/usr/share/nginx/html"
                  name: task-pv-storage

        $ k apply -f pv-pod.yaml
        pod/task-pv-pod created

        # pvc 10Mi -> 90Mi 용량 변경
        $ k edit pvc task-pv-claim
        ...........
        spec:
          accessModes:
          - ReadWriteOnce
          resources:
            requests:
              storage: 90Mi  <== 변경
          storageClassName: manual
          volumeMode: Filesystem
          volumeName: task-pv-volume
        ...........
        persistentvolumeclaim/task-pv-claim edited

        ```

        ```bash
        [변경 이벤트 검증 확인하기]

        # 변경 이벤트 확인
        #
        $ k describe pvc task-pv-claim
        ..............
          Warning  ExternalExpanding      3m11s  volume_expand                                                           waiting for an external controller to expand this PVC
        ```
    * \[Resource관련] sidecar multi containers로 sidecar의 log 확인하기(복기) **`re머지`**
      * 기존엔 container내 emptyDir 설정하여 /var/log를 통해 sidecar로 로그 확인
    * Ingress 생성 후 테스트
      *   블로그 링크 1번

          [https://velog.io/@kjyggg/쿠버네티스-CKA-시험-복기202312](https://velog.io/@kjyggg/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-CKA-%EC%8B%9C%ED%97%98-%EB%B3%B5%EA%B8%B0202312)
    *   \[네트워크] Ingress를 생성해서 이미 생성되어 있는 서비스와 연결 후 확인 **`re`**

        * ingressClassName를 삭제햇는지는 모르겠네

        ```bash
        apiVersion: networking.k8s.io/v1
        kind: Ingress
        metadata:
          name: minimal-ingress
          annotations:
            nginx.ingress.kubernetes.io/rewrite-target: /
        spec:
          ingressClassName: nginx-example <== 삭제
          rules:
          - http:
              paths:
              - path: /hi <== 수정
                pathType: Prefix
                backend:
                  service:
                    name: my-service <== 수정
                    port:
                      number: 80
                      
        # 적용
        $  k apply -f minimal-ingress.yaml
        ingress.networking.k8s.io/minimal-ingress created

        # 확인
        $ k describe ingress minimal-ingress
        Name:             minimal-ingress
        Labels:           <none>
        Namespace:        default
        Address:
        Default backend:  <default>
        Rules:
          Host        Path  Backends
          ----        ----  --------
          *
                      /hi   my-service:80 (10.244.0.154:8080,10.244.0.155:8080,10.244.1.17:8080 + 2 more...)
        ```
    * \[네트워크] Networkpolicy 생성 후 특정 namespace의 Pod로만 연결(to, from..)
    *   \[권한] ServiceAccount 생성, Role 생성, Role Binding 생성 후 확인

        ```bash
        # serviceaccount 생성
        $ k create serviceaccount john $do > sa.yaml

        # apply
        $ k apply -f sa.yaml

        # role 생성
        $ k -n development create role developer --resource=pods --verb=create 
        $ k -n development create rolebinding developer-role-binding \\
          --role=developer --user=john
        ```
    *   \[스케줄 관련] 특정 Node를 drain하여 해당 노드는 SchedulingDisabled상태로 변경하고 Pod를 다른 Node로 옮기기.(맞춘 것 같은디)

        ```bash
        # 특정노드 이름 조회
        $ k get no --show-labels | grep k8s-node-0

        # # node drain
        # DaemonSet에서 관리하는 포드가 있는 경우 
        # 노드를 성공적으로 비우려면 --ignore-daemonsetswith를 지정해야한다.
        $ k drain --ignore-daemonsets k8s-node-0

        # node 상태 확인
        $ k get no -o wide

        # pod의 위치확인
        $ k get po -o wide
        ```
    *   \[필터\_데이터추출] Node 상태가 ready 갯수를 파일로 저장

        \<TIP>

        **ㄴ 이런 간단한 문제들은** [**kubectl Quick Reference**](https://kubernetes.io/docs/reference/kubectl/quick-reference/)**에서 검색을 해봅니다.**

        **ㄴ ready로 웹 검색을 하면 다음과 같은 방법 확인**

        ```bash
        # Check which nodes are ready
        JSONPATH='{range .items[*]}{@.metadata.name}:{range @.status.conditions[*]}{@.type}={@.status};{end}{end}' \\
         && kubectl get nodes -o jsonpath="$JSONPATH" | grep "Ready=True"

        # Check which nodes are ready with custom-columns
        kubectl get node -o custom-columns='NODE_NAME:.metadata.name,STATUS:.status.conditions[?(@.type=="Ready")].status'
        ```

        ㄴ 응용하여 완성

        ```
        ```

        ```bash
        $ k get node -o custom-columns='NODE_NAME:.metadata.name,STATUS:.status.conditions[?(@.type=="Ready")].status' --no-headers| wc -l > 파일명
        ```
    *   \[필터\_데이터추출] 사용률이 가장 높은 pod를 label로 조회해서 파일로 저장

        ```bash
        # 특정 namespace에 control-plane(샘플)라벨로 필터하여 cpu로 정렬
        k top po -n kube-system -l tier=control-plane --sort-by=cpu

        # 답안작성
        $ echo '노드이름' > 답변 파일 위치
        ```

    ***

    `쉬운것`

    * \[Resource관련] Pod에 nodeSelector 추가
    * \[Resource관련] deploy생성후 image 업그레이드
    *   \[네트워크] Pod(port 80)생성, NodePort 타입 Service 생성

        ```bash
        # pod 생성
        $ k run nginx-resolver --image nginx 

        # pod expose
        $ k expose pod nginx-resolver -name nginx-resolver-service\\
         --port 80 --target-port 80 --type NodePort
         
        # 확인
        $ k get svc nginx-resolver-service -o yaml
        ```
    *   \[스케줄 관련] 특정 Deployment에 대해 replicas 수정

        ```bash
        $ k scale deployment/nginx-deployment --replicas=10

        ```
    *   \[필터\_데이터추출] Pod에서 log grep하여 파일로 추출.

        ```bash
        # ErrorMessage 로그필터
        $ k logs podName | grep ErrorMessage > 답안경로
        ```
    *   \[필터\_데이터추출] Taint가 없는 Node의 갯수를 파일로 저장(혹은 Taint말고 다른게 없는경우)

        ```bash
        # 노드 확인
        $ k get no => 노드 갯수 3개 확인

        # k describe no | grep -i taint
        Taints:             <none>  <== 없는 것은 none으로 확인

        # 답안작성
        echo '2' > 답안파일 경로
        ```
