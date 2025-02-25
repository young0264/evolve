# 연결 리스트에 대해서 설명

연결 리스트(Linked List)는 노드(요소)들을 포인터로 연결하여 관리하는 선형 자료구조.

* 각 노드는 데이터와 다음 요소에 대한 포인터를 가지고 있음. 이때 첫번째 노드를 HEAD, 마지막 노드를 TAIL.
* 메모리가 허용하는 한 요소를 계속 삽입할 수 있음. 시간 복잡도에는 탐색에는 O(N), 노드 삽입과 삭제는 O(1),
* 배열은 순차적인 데이터가 들어가기 때문에 메모리 영역을 연속적으로 사용함. 반면 연결리스트는 메모리 공간에 흩어져서 존재한다는 점에서 배열과 차이가 있음.

```java
class SinglyLinkedList {

    public Node head;
    public Node tail;

    public Node insert(int newValue) {
        Node newNode = new Node(null, newValue);
        if (head == null) {
            head = newNode;
        } else {
            tail.next = newNode;
        }
        tail = newNode;

        return newNode;
    }

    public Node find(int findValue) {
        Node currentNode = head;
        while (currentNode.value != findValue) {
            currentNode = currentNode.next;
        }

        return currentNode;
    }

    public void appendNext(Node prevNode, int value) {
        prevNode.next = new Node(prevNode.next, value);
    }

    public void deleteNext(Node prevNode) {
        if (prevNode.next != null) {
            prevNode.next = prevNode.next.next;
        }
    }
}

```

* 단일 연결 리스트에서 탐색은 최악의 경우에 O(N), 노드를 탐색하기 위해 HEAD의 포인터부터 데이터를 찾을 때까지 다음 요소를 반복적으로 탐색하기 때문.
*   삽입의 경우, 삽입될 위치 이전에 존재하는 노드와 신규 노드의 포인터를 조작하면 됨.

    3번 위치에 신규 노드를 삽입해야하면, ‘2번 → 신규 → 기존3번‘으로 수정하면 됨
* 삭제의 경우, ‘이전노드 → 다음노드’ 를 가르키도록 하여 수정 가능
* 삽입과 삭제의 경우 O(1)의 시간복잡도. 하지만 탐색의 경우엔 선형 시간이 걸릴 수 있음

> \[참고]
>
> * [\[10분 테코톡\] 🐻 중간곰의 선형 자료구조](https://youtu.be/xnURecIJk4g?feature=shared)
> * [널널한 개발자 - 단일 연결 리스트 구현 첫 번째](https://youtu.be/i_rONJmWeKY?feature=shared)
