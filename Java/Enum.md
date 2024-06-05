# enum

- 변수에 상태가 정해진 상수 값을 정의할 때 사용
- 특정한 변수가 가진 값을 고정하면서 `정적 타입안정성(type safty)`을 보장
- 상태가 정해진 상수 값을 벗어난 경우 컴파일 단계에서 오류 파악이 가능
    ex) 값 비교의 경우

```
public eunm Status{
    status1,
    , status2
    , status3
}
```
## 일반 클래스와 다른점
- 일반 클래스의 경우, Object를 extends
- enum 클래스의 경우, Object > java.lang.Enum을 extends하고 있다
    => 다중 상속 안되므로 `enum은 extends 할 수 없다.`

## enum 인스턴스
- eum에 열거할 수 있는 값(ex status1, status2)
- 실제 열거된 값(status1)을 싱글톤으로 생성하기 떄문에 객체 인스턴스!!

## enum 왜 싱글톤인가?
- 정적 타입안정성, 값의 비교나 정의에 사용되기 떄문에 값 상태가 변할 일이 없다.
    => 효율적인 측면에서 미리 만들어놓고 계속 참조하는 형태

## enum 메소드 정리
## ordinal() 주의 사항
`그냥 쓰지마라!! 공식문서에서도 쓰지말라 하는데..`
```
ex) 
치킨(1)
피자(2)
```
문제상황1) 신입 개발자가 와서 국밥(0)을 추가한다면
그게 코딩 컨벤션에 맞지않다면?

문제상황2) 비즈니스 로직에 ordinal을 가지고 활용하는 로직이 있다면?
    => 국밥(0)으로 인해 치킨, 피자가 하나씩 밀릴 수 있다.


### Enum을 가지고 있는 값들 전부 조회하는 방법
<font color = 'blue'>`values()를 사용하자!!!`</font>
```
public static void main(String[] args){
    Arrays.stream(Status.values()).forEach(System.out::println);
}

```

## Enum도 생성자, 메소드, 필드를 가질 수 있는지
`물론 가능하다!!`
- 생성자
    - private, package-private만 가능하므로 new를 통해서 생성은 불가능
    - 아래 예시처럼 변수, 메소드 가능하다
```
public enum Status{
    /*
    status1,2 명칭은 바뀌더라도 값 유지하도록 유지보수 하고 싶다일 떄
    주로 사용
    */
    status1(100)
    , status2(200)
    

    private int number;

    Status(int number){
        this.number = number;
    }

}
```
## Enum 비교 시 ==, equals를 사용할지?
`== 사용하자!!`

* 근거
    - JVM 레벨에서 싱글톤으로 생성된다.
        => 클래스 로더에 의해 클래스 파일 로딩되면 인스턴스로 올라간다.
        => <font color = "red">객체 참조 비교가 가능해진다!!</font>
    - nullPointerExcepton을 피할 수 있다.
        => equqls의 경우 nullPointerException이 발생 우려 있음

## Enum를 key로 가지는 있는 Set, Map
`EnumSet, EnumMap를 사용해보자!!`
- ordinal()이 사용되는 예시라는데 확인해볼 것
- 순서가 보장되기 떄문에 조회에 훨씬 효율적라 하는데 확인이 필요하다.

## 참고 문서
[enum 전반적인 설명] https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%97%B4%EA%B1%B0%ED%98%95Enum-%ED%83%80%EC%9E%85-%EB%AC%B8%EB%B2%95-%ED%99%9C%EC%9A%A9-%EC%A0%95%EB%A6%AC

[우아한 형제 기술블로그] https://techblog.woowahan.com/2527/
