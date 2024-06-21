# 람다

## 람다식이란?
```
interface MyFunction{
    public abstrac int max(int a, int b);
}

* 익명클래스의 객체
MyFunction f = new MyFunction{
    public int max(int a, int b){

        return a > b? a:b;
    }
}

* 람다식으로 변경
MyFunction f = (a,b) -> {
    return a > b? a:b;
}
```
<br>

- 함수를 하나의 식처럼 표현한 함수형 인터페이스

- 람다식 = 익명메소드
    - 함수를 람다식으로 표현하면 이름이 없다

- 람다식을 다루기 위한 인터페이스를 `함수형 인터페이스`라 함

- 함수형 인터페이스는 `단 하나의 추상 메소드`만 정의되어야 한다.

- 람다식으로 선언된 함수는 1급 객체라서 Stream API의 매개변수 전달이 가능

## 메모리 관점에서의 람다
- 외부 변수를 공유
- 외부 변수의 값을 변경할 수 없다.
- 외부 메소드에 영향줄 수 없다.

## 1급 객체
- 람다식을 참조변수처럼 다룰 수 되면서 `람다식을 변수처럼` 주고받는게 가능하다.


1) 변수나 데이터에 담을 수 있어야 한다
```
* 자바스크립트
const hello = function() {
	console.log("Hello World");
}

* 자바
// 람다식을 인터페이스 타입 변수에 할당
Consumer<String> c = (t) -> System.out.println(t); 
```

2) 함수의 파라메터로 전달 가능해야 한다
```
* 자바스크립트
const hello = function() {
	console.log("Hello World");
}

function print(func) {
	func();
}

print(hello);

* 자바
public static void print(Consumer<String> c, String str) {
    c.accept(str);
}

public static void main(String[] args) {
    //(t) -> System.out.println(t)
    print((t) -> System.out.println(t) ,"Hello World");
}
```

3) 함수의 리턴값으로 사용 가능
```
* 자바스크립트
const hello = function() {
	console.log("Hello World");
    return function() {
    	console.log("Hello World 22");
    }
}

const hello2 = hello();
hello2();

* 자바
public class Main {
    public static Consumer<String> hello() {
        // 람다 함수 자체를 리턴함
        return (t) -> {
            System.out.println(t);
        };
    }

    public static void main(String[] args) {
        Consumer<String> c = hello();
        c.accept("Hello World");
    }
}
```


## 장점
- 코드 간결하게 만들어서 가독성 높인다
- 병렬 프로그래밍 용이
- lazy한 객체

## 단점
- 람다로 만든 익명함수는 재사용불가
- 디버깅이 힘듬
- 람다를 남발하면 중복코드 생길 우려가 있음
- 재귀로 만들경우 부적합???

## @FunctionalInterface
- 구현해야 할 추상 메소드가 하나만 정의된 인터페이스, 함수형 인터페이스

## java.util.function 패키지
자주 사용되는 함수형 인터페이스를 정의함
아래 링크 참고하자
https://hstory0208.tistory.com/entry/Java%EC%9E%90%EB%B0%94-%EB%9E%8C%EB%8B%A4%EC%8B%9DLambda%EC%9D%B4%EB%9E%80-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%82%AC%EC%9A%A9%EB%B2%95

## 람다 활용 예시1
- lazy한 객체 생성
- 문제점
    Heavy 클래스 사용되지 않는데 Holder가 생성될 때 먼저 생성되는 문제가 있다.

[AS-IS]
```
public class Heavy {        
    Heavy() {            
        System.out.println("Heavy created");        
    }    
}        

public class Holder 
{        
    public Holder() {            
        System.out.println("Holder created");  
    }            
        
    Heavy heavy = new Heavy();
    
    public Heavy getHeavy() {            
        return heavy;        
    }    
}        

public static void main(String[] args) {        
    Holder holder = new Holder();        
    System.out.println("Get Heavy instance");   
}

```

[TO-BE]
```
public class Lazy<T> {        
    private Optional<T> instance = Optional.empty();        
    private Supplier<T> supplier;                
    public Lazy(Supplier<T> theSupplier) {
        supplier = theSupplier;        
    }                
    
    public T get() {            
        if(!instance.isPresent()) {
            instance = Optional.of(supplier.get());
        }
        return instance.get();        
    }    
}


public class Holder {        
    public Holder() {            
        System.out.println("Holder created");        
    }            
    Lazy<Heavy> heavy = new Lazy<Heavy> (() -> new Heavy());        
    public Heavy getHeavy() {
        return heavy.get();        
    }
}
```



## 출처
https://wedul.site/334


