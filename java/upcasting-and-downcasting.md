# Upcasting & Downcasting

*   업 캐스팅

    * 자식 클래스가 부모 클래스 타입으로 캐스팅 되는 것
    * 업캐스팅은 캐스팅 연산자 괄호 생략 가능
    * 자식 클래스에서만 있는 속성과 메서드는 사용 불가능.
    * 업캐스팅 했지만 자식클래스일 때 오버라이딩한 메서드가 있으면, 부모 메서드가아닌 오버라이딩 메서드가 실행됨

    ```java
    class Unit {
        public void attack() {
            System.out.println("유닛 공격");
        }
    }

    class Zealot extends Unit {
        public void attack() {
            System.out.println("찌르기");
        }

        public void teleportation() {
            System.out.println("프로토스 워프");
        }
    }

    public class Main {
        public static void main(String[] args) {
        
            Unit unit_up;
            Zealot zealot = new Zealot();
            
            // * 업캐스팅(upcasting)
    		unit_up = (Unit) zealot;
    		unit_up = zealot; // 업캐스팅은 형변환 괄호 생략 가능
        }
    }
    ```



*   다운 캐스팅

    * 다운 캐스팅은 업캐스팅한 객체를 되돌리는데 있음 (ClassCastException)

    ```java
    Unit unit = new Unit();

    // * 다운캐스팅(downcasting) 예외 - 다운캐스팅은 업스캐팅한 객체를 되돌릴때 적용 되는것이지, 오리지날 부모 객체를 자식 객체로 강제 형변환은 불가능
    Zealot unit_down2 = (Zealot) unit; //! RUNTIME ERROR - Unit cannot be cast to Zealot
    unit_down2.attack(); //! RUNTIME ERROR
    unit_down2.teleportation(); //! RUNTIME ERROR
    ```

> 참고 :[https://inpa.tistory.com/entry/JAVA-☕-업캐스팅-다운캐스팅-한방-이해하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%97%85%EC%BA%90%EC%8A%A4%ED%8C%85-%EB%8B%A4%EC%9A%B4%EC%BA%90%EC%8A%A4%ED%8C%85-%ED%95%9C%EB%B0%A9-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)
