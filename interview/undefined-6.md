# 교착 상태에 대해서 설명

#### **교착상태(dead lock)**

두 개 이상의 작업이 서로 상대방의 작업이 끝나기만을 기다리고 있어 결과적으로 아무것도 완료되지 못하는 상태를 의미.

ex)

*   A프로세스가 자원a를 가지고 자원b를 필요

    B프로세스가 자원b를 가지고 자원a를 필요

    → 두 개의 프로세스는 교착 상태에 빠져 어느 작업도 진행할 수 없음.

#### **교착 상태 발생 조건**

4가지 조건(상호배제, 점유대기, 비선점, 원형대기)이 모두 만족하는 경우, 교착 상태에 빠질 수 있음

*   **상호배제(mutual exclusion)**

    : 한 프로세스가 사용하는 자원을 다른 프로세스가 사용할 수 없는 경우를 의미함

    (프로세스끼리 자원 공유 X, 자원은 critical section, lock, CPU, Memory, SSD, 프린터 등 다양)
*   **점유대기(hold and wait)**

    : 자원을 할당받은 상태에서 다른 자원을 할당받기를 기다리는 상태를 의미

    (1개 이상의 리소스를 취한 상태에서, 다른 프로세스가 사용중인 리소스를 추가로 기다림)
*   **비선점(non-preemption)**

    : 자원이 강제적으로 해제될 수 없으며 점유하고 있는 프로세스의 작업이 끝난 이후에만 해제되는 것

    (리소스 반환은 그 리소스를 점유하고 있는 프로세스만 할 수 있음. 다른 프로세스가 뺏을 수 없음)
*   **원형대기(circular wait)**

    : 프로세스들이 원의 형태로 자원을 대기하는 상황.(내부 순환, 빙글빙글)

#### JAVA에서 교착상태 해결

```java
// thread 1
synchronized (resource1) { 
  synchronized(resource2) { ... }
}

// thread 2
synchronized (resource2) { 
  synchronized(resource1) { ... }
}
```

synchronized 블록 내부에 synchronized 블록을 포함하지 않도록 개선하여 점유 대기 조건을 제거하여 교착 상태를 해결할 수 있음

또한 ReentrantLock을 사용하는 경우에는 tryLock() 메서드를 사용하여 타임아웃을 설정하거나, lockInterruptibly()메서드를 사용하여 데드락이 발생하는 경우, 인터럽트를 통해 스레드를 깨울 수 있음.

#### 데드락 방지(Deadlock Prevention)

즉, 교착 상태가 발생하는 **4가지 조건 중 하나를 충족하지 못하게** 하거나, 대기하는 경우 \*\*무한정 기다리지 않는 방식(timeout 설정)\*\*으로 교착 상태를 풀 수 있음.

#### 데드락 회피(Deadlock Avoidance)

실행환경에서 현재 사용 가능한 리소스, 이미 사용중인 리소스 등을 활용해 데드락이 발생할 것 같은 상황을 예측해 미리 회피. 시스템 상태를 모니터링하고 데드락이 발생할 것 같으면 자원을 할당하지 않거나, 이미 할당된 자원을 해제해야함.

Banker Algorith은 리소스 요청을 허락했을 때 데드락 발생 가능성이 있으면, 리소스를 할당해도 안전할 때까지 계속 요청을 거절하는 알고리즘

#### 데드락 감지와 복구(Deadlock Detection and Recovery)

회복 작업은 데드락을 일으킨 프로세스 중 하나 혹은 여러 개를 중단시키거나, 일시적으로 선점하는 것을 허용해 데드락을 해결할 수 있음.

#### 데드락 무시(Do Nothing)

많은 운영체제에서 이 방식을 선택함

> \[참고]
>
> * [\[OS\] 데드락(Deadlock)은 언제 발생하고, OS, Java에서 어떻게 해결할까?](https://engineerinsight.tistory.com/290)
> * [\[JAVA\] 자바 쓰레드 동기화(1) - synchronized, wait()/notify()](https://jhkimmm.tistory.com/34)
> * [\[JAVA\] 자바 쓰레드 동기화(2) - ReentrantLock과 Condition](https://jhkimmm.tistory.com/36)
