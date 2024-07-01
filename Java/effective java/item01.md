### 자바가 지원하는 타입
- primitive
- reference
    - Class
        - Enum
    - Interface
        - Annotation
    - Array

## Item1 생성자 대신 정적 팩터리 메서드를 고려하자

## 장점

- 이름을 가질 수 있다

ex) 생성자
```
new BigInteger(int, int, Random)
```

ex) 정적 팩터리 메서드
```
BigInteger.probablePrime
```
정적 팩터리 메소드가 메소드 이름을 가지면서 `반환될 객체의 특성`을 잘 살린다.

- 호출될 때마다 인스턴스 생성이 필요 없다.
    - 불변 클래스는 인스턴스 미리 생성하거나 새로 생성한 인스턴스를 캐싱하는 용도로 사용
    - 생성 비용이 큰 객체가 자주 요청되는 상황에서는 성능에 이점 있음
    - 플라이트웨이터 패턴과 비슷
    - 언제 어느 인스턴스를 살아있게 할지 통제 가능
        - `인스턴스 통제 클래스`
    - ex) Boolean.valueOf
    ```
    @IntrinsicCandidate
    public static Boolean valueOf(boolean b) {
        return (b ? TRUE : FALSE);
    }
    ```
- 반환 타입의 하위 타입의 객체를 반환 가능
    - api 생성 시, 구현 클래스 공개가 필요 없어서 api를 작게 유지 가능

자바 8 전에는 인터페이스에 정적 메소드 불가
인스턴스화 불가 동반 클래스

자바 8부터는 public 정적 멤버, 메소드 가능

자바 9에서는 private 정적 메소드
정적 필드와 정적 멤버 클래스는 public

- 입력 매개변수에 따라 매번 다른 클래스의 객체 변환 가능
    - 반환 타입의 하위타입이면 어떤 클래스 반환하든 상관 없음

- 정적 팩터리 메소드 작성 시점에 반환할 객체의 클래스가 존재안해도 됨

## 단점
- 상속하려면 public, protected 생성자가 필요하기에 정적 팩토리만 제공하면 하위클래스 만들 수가 없음
- 정적 팩토리는 프로그래머가 찾기 어려워서 흔히 사용하는 명명 방식이 존재
    - from, of, valueOf, getInstance, newInstance, getType, newType, type

## 정리
- 정적 팩토리 메소드와 public 생성자는 각각의 쓰임새가 있으니 장단점 고려하여 사용할 것
- 정적 팩토리를 사용하는게 유리한 경우가 많으니 무조건 public 생성자를 제공하던 습관을 고치자


