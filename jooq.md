# jooq

* dslContext란?
  * jooq의 DSL을 사용하여 SQL을 생성 혹은 native SQL 작성 가능
  * transaction 메서드를 호출하여 트랜잭션 관리 가능
* 특징
  * java 코드 기반의 config 제공
  * type-safe : compile단계에서의 type checking
  * jooq의 dsl을 이용하여 코드작성
    * → 일관성 유지, 가독상 향상
    * DBMS마다 SQL dialect가 다르지만 dsl을 이용해 다양한 DBMS 사용 가능
  * code-generator :
    * db 스캔을 통해 tables, records, sequences, POJOs, DAOs, stored procedures, user-defined types 등 class 생성

Q. jooq로 작성한 native sql은 그냥 sql과 어떤 차이?
