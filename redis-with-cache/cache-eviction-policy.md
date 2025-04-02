# Cache Eviction Policy

#### \[시간 기반 만료 전략] – **Cache Expiration Strategy**

* 데이터를 저장할 때 함께 설정하는 **TTL (Time-To-Live)** 또는 **TTI (Time-To-Idle)** 값을 기준으로 만료됨.
* 설정 방식: Redis에서는 `SET key value EX 60` → 60초 TTL

#### 주요 방식

| 전략                 | 설명                                                    |
| ------------------ | ----------------------------------------------------- |
| TTL (Time-To-Live) | 저장 시점 기준으로 일정 시간이 지나면 만료됨.                            |
| TTI (Time-To-Idle) | 일정 시간 동안 접근(읽기/쓰기)이 없으면 만료됨. (Redis 기본 미지원, 별도 구현 필요) |
| Absolute Expiry    | 특정 시간(예: 매일 자정)에 만료되도록 설정                             |

***

#### \[메모리 기반 제거 정책] – **Cache Eviction Policy**

1. Noeviction: 레디스에 데이터가 가득 차더라도 임의로 데이터를 삭제하지 않고, 레디스에 데이터를 저장할 수 없다는 에러를 반환
2. LRU eviction (Least-Recently Used): 가장 최근에 사용되지 않은 데이터부터 삭제하는 정책
3. LFU eviction(Least-Frequently Used): 가장 자주 사용되지 않은 데이터부터 삭제하는 정책
4. RANDOM eviction: 레디스에 저장된 키 중 하나를 임의로 골라내 삭제함
5. volatile-ttl: 만료 시간이 가장 작은 키를 삭제 → 삭제 예정 시간이 얼마 남지 않은 키를 추출해 해당 키를 미리 삭제하는 옵션.

Redis의 `maxmemory-policy` 설정을 통해 지정:

#### 주요 정책

| 정책                  | 설명                                    |
| ------------------- | ------------------------------------- |
| **noeviction**      | 메모리가 부족해도 아무 것도 제거하지 않고 에러 반환         |
| **allkeys-lru**     | 모든 키 중에서 가장 오래 사용되지 않은 키를 제거          |
| **volatile-lru**    | TTL이 설정된 키들 중에서 가장 오래 사용되지 않은 키 제거    |
| **allkeys-lfu**     | 가장 적게 사용된 키 제거                        |
| **volatile-lfu**    | TTL이 설정된 키들 중 가장 적게 사용된 키 제거          |
| **allkeys-random**  | 모든 키 중에서 무작위로 제거                      |
| **volatile-random** | TTL이 설정된 키들 중에서 무작위 제거                |
| **volatile-ttl**    | TTL이 가장 적게 남은 키 제거 (즉시 만료 예정 키 먼저 제거) |

> 🔎 volatile-\* 은 TTL이 설정된 키에만 적용됨.
