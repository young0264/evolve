# Call By Value와 Call By Reference에 대해 설명

메서드 호출시 인자로 전달하는 방법은 크게 2가지. Call By Value, Call By Reference

#### Call By Value

* 메서드 호출시 값 자체를 넘겨주는 방식.
* 메서드를 호출하는 함수의 변수와 호출된 함수의 파라미터는 서로 다른 변수

#### Call By Reference

* 메서드를 호출할 때, 참조를 직접 전달하는 방식
* 호출하는 변수와 호출된 함수의 파라미터는 동일한 변수임

#### JAVA의 호출방식? → Call By Value

* JAVA는 Call By Value 만 존재함
* 자바 변수는 스택 영역에 할당.
* 변수가 **원시 타입**인 경우에는 값 또한 스택 영역에 저장됨
  * 호출 메서드의 파라미터가 변경되어도 원본은 수정 안됨.
  * 호출된 메서드의 스택 프레임에 인자로 주어진 변수의 값이 복사되어 사용되기 때문
* **참조 타입**인 경우 객체 자체는 힙 영역에 저장되고 스택 영역에 존재하는 변수가 객체의 주소를 가지고 있음
  * 호출된 메서드 내부에서 인자를 수정하면 원본이 수정될 수 있음
  *   스택 프레임에 참조타입 변수를 중복해서 생성하기 때문에 Call By Value로 판단

      (같은 참조를 바라보는 다른 스택 변수)
  * foo 메서드의 스택 프레임과 var 메서드의 스택프레임에 각각 같은 student 객체 주소를가진 참조 타입 변수인 student가 존재.

```java
public void foo() {
    Student student = new Student();
    var(student);
}

public void var(Student student) {
    student.study();
}
```

exam code : [https://github.com/young0264/java-playground/commit/77628ae03a8263fd4834ec67ade55ed75055fadf](https://github.com/young0264/java-playground/commit/77628ae03a8263fd4834ec67ade55ed75055fadf)&#x20;

> \[참고]
>
> * [코드라떼 - 자바 메모리 모델 기초](https://www.codelatte.io/courses/java_programming_basic/IGO4YNAECLV8MSVF)
> * [코드라떼 - Java == Call By Value](https://www.codelatte.io/courses/java_programming_basic/R842DJSB0BTCRLKM)
