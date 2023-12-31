# what is database ?

* 파일시스템과 데이터베이스의 차이점에 대해서 설명해주세요.
  * 파일시스템은 데이터에 대한 동기화가 되지 않는다는 단점이 있습니다. 반면 데이터베이스를 바탕으로 데이터를 관리하게 되면 데이터를 중앙집중식으로 관리하기 때문에 동기화를 통해 여러사람들이 접근하여 데이터를 관리할 수 있는 장점이 있습니다.
  * 실시간 접근성
  * 각 프로그램마다 데이터를 가지고 있음
  * 데이터 공간 절약
* 데이터베이스의 특징에 대해 설명해주세요.
  * 무결성 / 일관성.
  * 데이터에 실시간적으로 접근하여 동기화가 가능하다는 장점. (통합된 데이터 Real-Time Accessibility)
  * 여러 사용자에게 동시에 같은 내용의 데이터를 보여줄 수 있음 (동시공용 Concurrent Sharing)
* 데이터무결성 : **모든 데이터가 얼마나 완전하고, 일관되며, 정확한지를 나타내는 정도**
* DBMS는 뭘까요? 특징에 대해 설명해주세요.
  * **DataBase Management System**
  * 다수의 사용자들이 사용하는 데이터베이스에 접근할 수 있도록 도와주는 시스템으로 그 안에 있는 데이터들의 조회 수정 삭제 등록 등등을 할 수 있는 기능을 제공해주는 시스템입니다.
*   스키마가 뭘까요? 3단계 데이터베이스 구조에 대해 설명해주세요.

    [https://gunjoon.tistory.com/73](https://gunjoon.tistory.com/73)

    [https://velog.io/@yoonuk/데이터베이스-데이터-독립성과-스키마](https://velog.io/@yoonuk/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%8F%85%EB%A6%BD%EC%84%B1%EA%B3%BC-%EC%8A%A4%ED%82%A4%EB%A7%88)

    * 스키마란?
      * 데이터베이스의 구조(데이터 객체, 관계)와 제약조건의 명세
      * 데이터베이스의 골격으로 속성(Attribute), 개체(Entity), 제약조건등에 정의한 것을 말합니다.
    *   외부, 내부, 개념 스키마가 있음.

        **외부** : 사용자나 관리자(프로그래머)가 접근하는 데이터베이스를 정의한 것

        **개념** ( = 스키마) :

        * 개념 스키마는 DB에 저장되는 모든 데이터의 논리적 구조를 결정한다.
        * 개념 스키마는 개체 간의 관계(Relationship)와 제약 조건을 나타내고 데이터베이스의 접근 권한, 보안 및 무결성 규칙에 관한 명세를 정의합니다.
        * **데이터베이스 파일에 저장되는 데이터의 형태를 나타내는 것**으로, 단순히 스키마라고 하면 개념 스키마를 의미합니다.

        **내부** : 데이터가 **물리적**으로 저장되는 방법을 명세한 것.

        ex) 내부 레코드형식, 인덱스유무 등
    *   **결국 외부, 개념, 내부 스키마는 같은 데이터베이스의 데이터 구조와 제약 조건을 정의한 것으로 내용이 같다. 단지 상위 단계의 스키마일수록 구체적인 구조가 감춰져 있을뿐이다.**

        ![스크린샷 2023-11-05 16.01.34.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/23aed921-524e-4217-81d5-08fa853d720e/e5b44d54-6e45-4204-a212-cd4d31f3ab67/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA\_2023-11-05\_16.01.34.png)
    * 왜 이렇게 나누냐 ? :
      * 사용자 편의성 증가 : 사용자는 외부만 알면 됨.
      * 데이터 독립성 :
*   데이터 독립성에 대해서 설명해주세요.

    * 데이터의 논리적 구조나 물리적 구조가 변경되더라도 응용 프로그램이 영향을 받지 않는 것
    * **데이터 베이스의 구조나 데이터의 내용이 서로 다른 부분에 영향을 미치지 않는 것**을 뜻함. 데이터 독립성은 논리적 데이터 독립성과 물리적 데이터 독립성으로 나눌 수 있습니다.
    * 논리적 데이터 독립성
      *   논리적 구조가 변경되어도 사용자의 요구에 영향을 주지 않는 것

          테이블의 속성이나 관계가 추가되거나 삭제되어도 기존의 쿼리나 트랜잭션에는 영향이 없는 것
      * 개념 스키마가 변경되어도 외부 스키마에는 영향이 미치지 않도록 하는 것
    * 물리적 데이터 독립성
      * 물리적 데이터 독립성은 db의 물리적구조. 즉, 인덱스의 구성이 바뀌어도 데이터베이스의 스키마나 데이터는 그대로 유지되는 특성
      * 내부 스키마가 변경되어도 외부나 개념 스키마는 영향을 받지 않도록 지원하는 것
    * 장점
      * 유지보수와 확장성에 있어서 장점을 가지고, 데이터 베이스의 구조가 변경되더라도 응용 프로그램이나 사용자에게 영향을 주지 않으므로, 시스템의 신뢰성을 높일 수 있음. 또한 db의 성능이나 보안을 개선하기 위해 필요한 조치를 쉽게 적용할 수 있음.

    ***

    → 이렇게 데이터 독립성을 보장하기 위해. 데이터베이스의 구조는 3단계로 나뉨

    → 스키마란 무엇인가

    → 데이터 베이스의 3단계구조란 무엇인가
* RDBMS(관계형 데이터베이스 관리시스템)는 뭘까요?
  * 테이블의 행과 열과 정보를 구조화하는 방식. 테이블간 관계를 연결하고 조인해서 데이터를 목적에 맞게 가져올 수 있습니다.
* **릴레이션 스키마와 릴레이션 인스턴스에 대해서 설명해주세요. : `릴레이션의 정의`**
  * 릴레이션이란 : 엔티티
  * 릴레이션 스키마는 데이터베이스의 **구조**나 규약을 정해놓은 컬럼title 값이고
  * 릴레이션 인스턴스는 릴레이션 스키마의 조건에 맞춰 삽입되어 있는 데이터 **값**들입니다.
* 릴레이션의 차수와 카니덜리티에 대해 설명해주세요.
  * 차수 : 속성의 갯수 (애트리뷰트의 수)
  * 카디널리티(cardinality) :
    * 수학에서cardinality는 집합(중복이 X)의 요소 수 입니다.
    *   데이터베이스에서 cardinality는 컬럼에 대응되어 식별되는 row의 수입니다.

        그렇기 때문에 cardinality가 높으면 중복도가 낮은것이고, cardinality가 낮으면 중복도가 높다고 말합니다.
    * 해당 열에 대한 서로 다른 값을 가지는 레코드의 수
* 데이터베이스에서 사용되는 키(Key)에 대해서 설명해주세요. (슈퍼키, 후보키, 기본키, 대리키, 외래키)
  * 슈퍼키 : 각 행을 유일하게 식별 가능한 키. 하나 이상의 속성의 집합
  * 기본키(primary key) : 오직 한개의 키로 row를 식별 가능한 키. 그렇기에 최소성과 유일성을 만족시킴
  * 복합키(composite key) : table에서 row를 식별할 수 있는 2개이상의 컬럼(속성)으로 이루어진 키의 집합니다. 즉 primary키로 쓸수있는 column들을 말합니다.
  * 후보키(candidate key) : 기본키가 될 수 있는 후보들
  * 대리키(대체키 Alternate) : 기본키로 지정하고 남은 후보키
  * 외래키(foreign key) : 다른 테이블의 값을 참조하기 위해 다른 테이블의 식별 가능한 기본키.
*   무결성 제약조건에 대해서 설명해주세요. (도메인 무결성, 개체 무결성, 참조 무결성)

    [https://jerryjerryjerry.tistory.com/50](https://jerryjerryjerry.tistory.com/50)

    * 도메인 무결성 : check, default, not null 제약조건이 있음. 속성이 정의되어 있는 영역을 벗어나지 않도록 규정하는 것
    * 개체 무결성 : 하나의 테이블에 중복된 행이 존재하지 않도록 규정하는 것 : unique, primary key등이 있음. 기본키 제약조건이라고도 함. 모든 테이블이 기본키로 선택된 컬럼을 가져야 하는것
    * 참조 무결성 : 입력, 수정, 삭제시 연관되는 다른 테이블과의 데이터가 정확하게 유지되도록 규정하는 것. 참조 무결성에는 foreign key가 있다. 즉 다른 테이블의 참조하기 위해 놓은 외래키의 속성이 다른 테이블의 row를 식별하는 데에 있어서 유일성과 최소성을 만족하는 기본키여야 하는 것이다.
* 사용했던 데이터베이스에 대해서 설명해주세요. (오라클DB, MySQL, MariaDB, MongoDB 등)
  *   oracle과 mysql db를 사용해 본 경험이 있습니다.

      oracle과 mysql모두 관계형 데이터 베이스의 일종으로 목적에 테이블간 관계를 연결하여 데이터를 관리한다는 점에서는 공통점이 있습니다. oracle의 경우 개인은 무료지만 유로로 제공되며 mysql은 무료로 제공됩니다. 그리고 oracle은 내부적으로 rownum같은 편리한 함수들을 제공해줌
