# SQL Mapper vs Java Persistence Api

\[공통점]

* DB와의 연동 저장을 위한 기술

\[차이점]

* SQL Mapper(ex : Mybatis)
  * **개발자가 작성한 SQL 실행 결과를 객체에 매핑** 시켜주는 프레임워크
* ORM(Object Relational Mapping), (ex : JPA)
  * 객체와 DB의 데이터를 **자동으로 매핑** 시켜주는 프레임워크



### \[Mybatis]

\[특징]

* Mybatis에서 Java메서드와 SQL간에 매핑을 시켜서 개발자는 Java 메서드 선언과 SQL문만 만들면 Mybatis가 자동으로 그 둘 간을 연결시켜 줌
* SQL문을 별도의 Java 코드로 분리, 분리된 SQL문을 Mybatis가 찾아서 실행
* 동적인 SQL 생성 기능을 제공.(Dynamic SQL)
  * 프로그램 실행중에 입력되는 파라미터에 따라 서로 다른 SQL문을 동적으로 생성
  * if문 choose, when, otherwise, foreach등의 문법을 지원해서 동적인 SQL문 생성 가능
  * SQL문을 Java 객체로 자동으로 매핑시켜주는 프레임워크

\[장점]

1. SQL 직접 제어
   * 복잡한 쿼리 작성 및 DB 최적화 쿼리 작성시 적합
2. 낮은 학습곡선
   * SQL을 알면 손쉽게 접근 가능

\[단점]

1. CRUD 단순 작업일지라도 번거로운 수작업 설정이 많음
   * 생산성 저하 및 코드 유지보수 떨어짐
2. 데이터베이스에 종속적
   * 특정 DB를 기준으로 SQL문이 작성되어 있어서, DB 변경시 SQL 전체 확인 후 수정



### \[JPA]

* Java 객체와 관계형 데이터베이스 간의 매핑을 위한 API
* ORM을 구현하는 자바 표준 스펙으로, 객체지향 프로그래밍 언어에서 사용하는 객체 모델과 관계형 데이터베이스의 테이블 간의 매핑을 자동으로 처리
* Java 객체와 DB 엔티티(테이블) 자체를 그대로 매핑해서 처리할 수 있는 접근 방식 (객체와 데이터베이스 간의 매핑 기술)
* SQL문을 자동 생성, DB데이터와 Java객체를 매핑시켜주는 기술
* SQL 작성이 필요 없어짐, ORM 내부적으로 java 메서드에 적합한 SQL 문이 자동으로 생성되어 실행되게 됨
* jpa.persist(member) → insert SQL문이 자동생성, DB저장
* jpa.find(memberId) → Select SQL문이 자동으로 생성, DB 조회

\[장점]

1. 표준화된 인터페이스 → java 표준 이용, 제품종속x, 일관된 방식 개발 가능
   * JPA는 자바에서의 ORM을 위한 표준 인터페이스를 제공함. Hibernate, EclipseLink, Apache OpenJPA등 구현체가 JPA 표준을 따름
   * JPA가 표준. 다른 프레임웍들이 표준을 따라 만듦
2. 자동 매핑 개발 유지보수성 향상
   * 객체와 DB간 자동 매핑 지원 → 유지보수 향상
   * 객체 지향적 개발에 중점 →테이블과 객체간 연관관계를 쉽게 다룰 수 있음
   * DBMS에 독립적 → JPA에서 DB종류에 따라 SQL Dialect(방언) 생성.

\[단점]

1. 높은 학습곡선
2. 간단한 매핑 및 객체 지향적인 접근은 JPA가 sql 작성등 반복적인 부분 해줌.
3. 통계, 분석과 같은 경우 복잡한 SQL 생성시 어려움
4. 성능에 대한 고려 필요(자동화된 SQL문으로 인해 데이터 조회 성능이 떨어질 가능성 높음)
