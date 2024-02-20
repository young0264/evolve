---
description: int[] vs Integer[] performance (primitive type vs wrapper type)
---

# int\[] vs Integer\[]

* object type은 비싸다.(그걸 배열로 모아두면 더 비싸겠지?)
  * 10억번의 반복문에서 object type이 43배 느린것을 확인.
* don't care about performance and want to do everything in an object oriented fashion, use `Integer`.

#### 속도 & 메모리 테스트

<figure><img src="../../.gitbook/assets/Untitled1.png" alt=""><figcaption><p>int array test</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/Untitled2.png" alt=""><figcaption><p>Integer array test</p></figcaption></figure>

```java
@Test //usedMemory : 603455488 (6억)
void int_array_billion_init() { // 1억 : 309ms, 1.5억 : 378ms
    int hundredMillion = 150000000;
    int[] intArrays = new int[hundredMillion];
    for (int i = 0; i < hundredMillion; i++) {
        intArrays[i] = i + 1;
    }
}

@Test //usedMemory : 3051880448 (30억)
void integer_array_billion_init() { // 1억 : 356ms, 1.5억 : 944ms
    int hundredMillion = 150000000;
    Integer[] integerArrays = new Integer[hundredMillion];
    for (int i = 0; i < hundredMillion; i++) {
        integerArrays[i] = i + 1;
    }
}
```

> 참고 : [https://stackoverflow.com/questions/4624933/comparing-performance-of-int-and-integer](https://stackoverflow.com/questions/4624933/comparing-performance-of-int-and-integer) [https://web.archive.org/web/20060423040119/http://www.glenmccl.com:80/tip\_016.htm](https://web.archive.org/web/20060423040119/http://www.glenmccl.com:80/tip\_016.htm) [https://openwritings.net/pg/java/performance-primitive-data-type-vs-object-data-type-java](https://openwritings.net/pg/java/performance-primitive-data-type-vs-object-data-type-java)
