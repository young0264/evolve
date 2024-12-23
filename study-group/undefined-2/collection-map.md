# Collection, Map

<figure><img src="../../.gitbook/assets/Untitled (1).png" alt="" width="375"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Untitled (1) (1).png" alt="" width="214"><figcaption></figcaption></figure>

* Collection : 가장 상위 인터페이스
* List : 순서가 있는 집합을 처리하기 위한 인터페이스, 인덱스 O, 중복 허용, List interface중 ArrayList를 가장 많이 사용
* Queue : FIFO 구조로 여러개의 객체를 처리하기 전에 담아놓고 처리하는 인터페이스
* Set : 중복을 허용하지 않는 집합을 처리하기 위한 인터페이스
* SortedSet : 오름차순을 갖는 Set 인터페이스
* Map : 키와 값의 쌍으로 구성된 객체의 집합을 처리하기 위한 interface, key에 대해 중복을 허용하지 않음
* SortedMap : 키를 오름차순으로 정렬하는 Map interface

***

#### Set

* HashSet : 데이터를 해쉬 테이블에 담는 클래스로 순서 없이 저장됨
* LinkedHashSet : 해쉬 테이블에 데이터를 담음. 저장된 순서에 따라 순서가 결정됨
*   TreeSet : red-black 이라는 tree에 담음. 값에 따라 순서가 정해짐. 데이터를 담으면서 동시에 정렬하여 HashSet보다 성능상 느림

    red-black tree 특징

    * 각각의 노드는 검은색이나 붉은색이어야 함
    * 가장 상위(root)노드는 검은색
    * 가장 말단(leaves)노드는 검은색
    * 붉은 노드는 검은 하위 노드만을 가짐(따라서 검은 노드는 붉은 상위 노드만을 가짐)
    * 모든 말단 노드로 이동하는 경로의 검은 노드 수는 동일함

#### List

* Vector : 객체 생성시 크기를 지정할 필요가 없는 배열 클래스
* ArrayList : Vector와 비슷 but 동기화 X
* LinkedList : ArrayList와 동일하지만 Queue 인터페이스를 구현해서 FIFO 큐 작업을 수행함

#### Map

* HashTable : 해쉬 테이블에 담는 클래스, 동기화 O
* HashMap : 해쉬 테이블에 담는 클래스, HashTable과 다른점은 null 허용 및 동기화 X
* TreeMap : red-black tree에 데이터를 담음. TreeSet과 다른점은 key에 의해 순서가 정해짐
* LinkedHashMap : HashMap과 거의 동일하며 이중 연결리스트(doubly-linkedlist) 방식을 사용해 데이터를 담음

#### Queue

* PriorityQueue : 큐에 추가도니 순서와 상관없이 먼저 생성된 객체가 먼저 나오도록 설계된 큐
* LinkedBlockingQueue : 저장할 데이터 크기를 선택적으로 지정 가능, FIFO기반의 링크 노드를 사용하는 Blocking Queue
* ArrayBlockingQueue : 저장되는 데이터의 크기가 정해져 있는 FIFO 기반의 Blocking Queue
* PriorityBlockingQueue : 저장되는 데이터의 크기가 정해져 있지 않고, 객체의 생성 순서에따라 순서가 저장되는 Blocking Queue
* DelayQueue : 큐가 대기하는 시간을 지정, 처리하도록 되어있는 큐
* SynchronousQueue : put() 메서드를 호출하면 다른 스레드에서 take()메서드가 호출될 때까지 대기하도록 되어 있는큐, 0이나 null을 리턴

***

#### Set의 성능 비교

* HashSet
  * 저장시 순서를 보장하지 않음
*   LinkedHashSet

    * 저장시 순서를 보장한다는 점에서 HashSet과 차이

    > This technique is particularly useful if a module takes a set on input, copies it, and later returns results whose order is determined by that of the copy.
* TreeSet
  * 데이터의 저장하면서 동시에 정렬

→ HashSet과 LinkedHashSet 은 구현에 따라서 약간의 성능차이는 있을 수 있음.

하지만 TreeSet은 눈에 띄게 엄청 느림

***

#### List의 성능 비교

* ArrayList
  * 보통 셋 중 가장 빠름(넣는건 비슷, 꺼내는데에서 차이가 큼)
  * 동기화 관리 X (그래서 빠름)
  * 첫번째 값 삭제시 마지막 값 삭제시보다 많이 느림(배열 사용)
* Vector
  * 동기화 관리 O(synchronized 선언) → 성능 저하
  * 첫번째 값 삭제시 마지막 값 삭제시보다 많이 느림(배열 사용)
* LinkedList
  * Queue 인터페이스를 상속받음 → peek() , poll() 메서드 사용

***

#### Map의 성능 비교

* HashMap
  * 동기화 X
* HashTable
  * 동기화 O
  * HashTable은 싱글 스레드로 동작 → 성능 이슈
  * ConcurrentHashMap의 사용을 권장
* LinkedHashMap
  * 입력된 key의 순서가 보장됨. (입력한 순서를 기억하는 자료구조)
* TreeeMap
  * 트리 형태로 정렬하여 데이터를 저장하는 TreeMap 클래스가 가장 느린 것을 알 수 있음.
