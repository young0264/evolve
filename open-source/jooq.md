# jooq

*   데이터베이스 인터페이스란?

    데이터베이스와의 통신을 추상화하고 단순화하여 개발자가 데이터베이스와스 상호작용을 쉽게 할 수 있도록 도와줌. application layer에서 작성한 open SQL 혹은 native SQL을 database layer 데이터베이스에 접근하여 데이터를 읽어오기 위해 사용되는 추상화 인터페이스.



* jooq란?
  * java로 SQL 쿼리를 짤 수 있는 **데이터베이스 인터페이스**로 데이터베이스 테이블과 자바 객체간의 매핑을 지원함.
  * 자바 기반의 데이터베이스 쿼리 빌더
    * 데이터베이스 쿼리 빌더 : 데이터베이스 쿼리를 동적으로 생성하는 데 사용되는 도구나 라이브러리



* dslContext란?
  * SQL을 작성 및 실행하기 위한 메서드들을 정의한 인터페이스
  * jooq의 DSL을 사용하여 SQL을 생성 혹은 native SQL 작성 가능
  * transaction 메서드를 호출하여 트랜잭션 관리 가능



* jooq 특징
  * java 코드 기반의 config 제공
  * DB 스키마를 읽어 도메인 클래스 생성
  * type-safe : compile단계에서의 type checking
  * jooq의 dsl을 이용하여 코드작성
    * → 일관성 유지, 가독상 향상
    * DBMS마다 SQL dialect가 다르지만 dsl을 이용해 다양한 DBMS 사용 가능
  *   code-generator :

      * db 스캔을 통해 tables, records, sequences, POJOs, DAOs, stored procedures, user-defined types 등 class 생성



Q. jooq로 작성한 native sql은 그냥 sql과 어떤 차이?

Q. jooq와 queryDsl의 차이?
