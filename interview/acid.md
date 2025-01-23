# ACID란?

**ACID?**

* 원자성(Atomicity), 일관성(Consistency), 격리성(Isolation), 지속성(Durability)의 약자
* 데이터베이스 트랜잭션이 안전하게 수행된다는 것을 보장하기 위한 성질
* 원자성(Atomicity)
  * All or Nothing
  * 내부 연산들이 부분적으로 실행되고 중단되지 않는 것을 보장함
  * 트랜잭션은 전체 성공과 전체 실패중 한가지만 수행함
* 일관성(Consistency)
  *   트랜잭션이 성공적으로 완료되면언제나 일관성 있는 데이터베이스 상태로 유지하는 것

      (예: data type은 integer이여야 한다는 것)
* 격리성(Isolation)
  * 동시에 실행되는 여러 트랜잭션이 서로 독립적임을 보장
  * 트랜잭션을 순차적으로 실행하기도함
  * 다른 트랜잭션이 해당 작업 사이에 끼어들지 못하도록 보장함
  * A와 B의 잔고 총합이 10000원으로 시작할 때, 특정 순간에는 총합이 10000원이 아닌 경우도 있음. 하지만 다른 트랜잭션은 10000원을 볼 수 있도록 보장해야함
* 지속성/내구성(Durability)
  * 성공적으로 수행된 트랜잭션은 영원히 반영되어야함.
  * 장애가 발생해도 성공적으로 수행된 트랜잭션의 결과는 항상 DB에 반영되어야함

> 참고
>
> * [\[10분 테코톡\] 로건의 Transaction](https://youtu.be/taUeIi6a6hk?feature=shared)
> * [데이터베이스 트랜잭션(transaction)을 아십니까? 그리고 트랜잭션의 매우 중요한 속성들인 ACID를 아십니까? 모르신다면 들렀다 가시지요](https://youtu.be/sLJ8ypeHGlM?feature=shared)
> * [심화 - ACID 데이터베이스와 BASE 데이터베이스의 차이점은 무엇인가요?](https://aws.amazon.com/ko/compare/the-difference-between-acid-and-base-database/)
> * [심화 - 분산 데이터베이스 탐구: 데이터 복제와 일관성](https://loosie.tistory.com/886)
