# Practice: Redis Data Type Examples  (CLI)



**\[Redis CLI 실행]**

*   Redis CLI 실행

    ```bash
    127.0.0.1:6379> ping
    PONG
    ```



**\[Redis 데이터 타입 실습]**

*   String 데이터 저장 및 조회

    ```bash
    127.0.0.1:6379> SET myKey "Hello Redis!"
    OK
    127.0.0.1:6379> GET myKey
    "Hello Redis!"
    ```


*   TTL 만료시간 설정 및 확인

    ```bash
    127.0.0.1:6379> GET myKey
    "Hello Redis!"
    127.0.0.1:6379> expire myKey 1
    (integer) 1
    127.0.0.1:6379> GET myKey
    ```


*   List 데이터 추가 및 조회(LPUSH, LRANGE)

    ```bash
    127.0.0.1:6379> LPUSH myList "Task1"
    (integer) 1
    127.0.0.1:6379> LPUSH myList "Task2"
    (integer) 2
    127.0.0.1:6379> LPUSH myList "Task3"
    (integer) 3
    127.0.0.1:6379> LRANGE myList 0 -1
    1) "Task3"
    2) "Task2"
    3) "Task1"
    ```


*   완료된 일 제거 (LPOP)

    ```bash
    127.0.0.1:6379> LPOP myList
    "Task3"
    ```


*   SET 데이터 추가 및 조회(SADD, SMEMBERS)

    ```bash
    127.0.0.1:6379> SADD mySet "apple"
    (integer) 1
    127.0.0.1:6379> SMEMBERS mySet
    1) "apple"
    127.0.0.1:6379> SADD mySet "banana"
    (integer) 1
    127.0.0.1:6379> SMEMBERS mySet
    ```


*   Sorted Set 데이터 추가 및 조회(ZADD, ZRANGE)

    ```bash
    127.0.0.1:6379> ZADD mySortedSet 1 "TaskA"
    (integer) 1
    127.0.0.1:6379>
    127.0.0.1:6379> ZADD mySortedSet 2 "TaskB"
    (integer) 1
    127.0.0.1:6379>
    127.0.0.1:6379> ZRANGE mySortedSet 0 -1 WITHSCORES
    1) "TaskA"
    2) "1"
    3) "TaskB"
    4) "2"
    127.0.0.1:6379> ZADD mySortedSet 3 "TaskC"
    (integer) 1
    127.0.0.1:6379> ZADD mySortedSet 2 "TaskD"
    (integer) 1
    127.0.0.1:6379> ZRANGE mySortedSet 0 -1 WITHSCORES
    1) "TaskA"
    2) "1"
    3) "TaskB"
    4) "2"
    5) "TaskD"
    6) "2"
    7) "TaskC"
    8) "3"
    ```


*   Hash 데이터 추가 및 조회(HSET, HGETALL)

    ```bash
    27.0.0.1:6379> HGETALL myHash
    (empty array)
    127.0.0.1:6379> HSET myHash name "Alice"
    (integer) 1
    127.0.0.1:6379> HSET myHash age "30"
    (integer) 1
    127.0.0.1:6379> HGETALL myHash
    1) "name"
    2) "Alice"
    3) "age"
    4) "30"
    127.0.0.1:6379>
    127.0.0.1:6379> HGETALL myHash
    1) "name"
    2) "Alice"
    3) "age"
    4) "30"
    ```



**\[데이터 삭제 및 만료 설정]**

*   DEL을 활용한 삭제

    ```bash
    DEL myKey
    DEL myList
    DEL mySet
    DEL mySortedSet
    DEL myHash
    ```
*   특정 시간 후 자동 삭제(EXPIRE)

    ```bash
    SET tempKey "This will expire soon"
    EXPIRE tempKey 5
    TTL tempKey
    ```
