# Optional
- 있거나 없는 값을 표현
- null을 대체
- 자바 8부터 추가된 기능


## 생성
- Optional.of()
```
Optional.of("value")
Optional.of(null) -> NullPointerException!!
```

- Optional.ofNullable()
    - null이 가능한 값으로 생성
    - NullPointerException 발생하지 않음
    - null이라면 값이 없는 optional 생성 
```
Optional.ofNullable("value")
```
- Optional.empty() : 값이 없는 Optional

## 값 구하기
- get() : 값이 없는 Optional에 get() 실행하면 exception 발생
```
Optional<string> opt1 = Optional.empty();
opt1.get(); //NoSuchElementException!!
```
## 값 있는지 체크
- isPresent() : 값이 있는지 확인
- isEmpty() : 값이 없는지 확인(자바11)

## 값이 있으면 하기
- ifPresent()
```
String val = getValue();
if(value != null){
    doSome(value);
}

Optional<String> val = getValue();
val.ifPresent((value) =>doSome(value));
```

- ifPresentOrElse()
```
String val = getValue();
if(value != null){
    doSome(value);
}else{
    doOther();
}

Optional<String> val = getValue();
val.ifPresentOrElse(
    (value) ->doSome(value)),
    () ->doOhter();

```
## 값이 없을 때 다른 값 사용
```
String val = getValue();
String result = value == null? "default" : value;
```
- orElse : 값 리턴
- orElseGet : 람다식 리턴
- or : Optional 리턴
- orElseThrow : 값이 없으면 exception, 있으면 값 리턴
```
Optional <String> opt = getValue();
opt.orElse("default");
opt.orElseGet(()-> "default");
opt.or(()-> Optional.of("default"));
opt.orElseThrow(() -> new NoMemberException())
```

## map
- map에 전달받는 함수 실행하여 변환한 Optional을 리턴
- 값이 없으면 빈 Optional 리턴
```
Member m = ...;
LocalDate birth = m.getBirthDay();
int passedDays = cal(birth);

[TO-BE 1차]
Optional<Integer> memOpt = ...;
Optional<LocalDate> birthOpt = memOpt.map(mem -> mem.getBirthDay());
Optional<Integer> pdOpt = birthOpt.map((birth) -> cal(birth));

[TO-BE 2차]
Optional<Integer> pdOpt =  memOpt.map(mem -> mem.getBirthDay()).map((birth) -> cal(birth));

//빈 Optional은 map으로 전달한 함수 실행하지 않고 빈 Optional 리턴
Optional<String> empty = Optional.emtpy();
Optional<String> empty2 = empty.map((str)-> str.toUpperCase());

```

## flatMap
- 전달받은 함수 이용 값을 변환한 Optional 리던
```
Optional<Member> mem = ...;
Optional<Optional<LocalDate>> dateOpt = mem.map(
    mem-> Optional.ofNullable(mem.getBirthDay())
)
Optional<LocalDate> dateOpt = mem.flatMap(
    mem->Optional.ofNullable(mem.getBirthDay())
)
```

## filter
- 조건 충족하면 값 그대로 리턴
- 조건 충족하지못하면 빈 Optional 리턴
```
String str = ...;
if(str!=null && str.length()>3){
    sout(str);
}

[TO-BE 1차]
Optional<String> strOpt = ...;
Optional<String> filter strOpt.filter((str) -> str.length()>3);
filter.ifPresent(str -> sout(str));

[TO-BE 2차]
Optional<String> filter strOpt.filter(
    str -> str.length()>3).filter.ifPresent(str -> sout(str)
);
```

## 두 개 Optional 조합1
```
Member m = ...;
if(m == null) return null;

Company c = getCompany(m);
if(c == null) return null;

Card card = createCard(m, c);
return card;
```
[TO-BE 1차]
```
Optional<Member> memOpt = ...;
Optional<Company> compOpt = memOpt.map((mem)-> m.getCompany(mem));

Optional<Card> card = memOpt.flatMap(
        mem-> compOpt.map(
            comp->createCard(mem, comp)
        );
);
```
[TO-BE 2차]
```
Optional<Card> card = memOpt.flatMap(mem -> {
    Optional<Company> comOpt = getCompanyOptional(mem);
    return compOpt.map(comp -> createCard(mem, comp));
});

```
## 두 개 Optional 조합2
[AS-IS]
```
Member m1 = ...;
Member m2 = ...;

if(m1 == null && m2 == null)
    return null;

if(m1 = null) return m2;
if(m2 = null) return m1;

return m1.year > m2.year ? m1:m2;
```
[TO-BE]
```
Optional<Member> mem1Opt = ...;
Optional<Member> mem2Opt = ...;

Optional<Member> result = 
mem1Opt.flatMap(m1 -> {
    //m1 있다면
    return mem2Opt.map(m2 -> {
        //m2 있다면
        return m1.year > m2.year ? m1:m2;
    }).orElse(m1); //m2 없다면 m1
}).or(() ->m2); //flatMap 결과 없다면 m2 사용

return result.orElse(null);

```

## 정리
- ifPresent() 사용하면 null 사용할 때랑 유사한 구조
- map, flatMap, filter, orElse, or, ifPresent 등 익숙해지자!!<br>
`Optional을 조금 더 Optioanl 답게 사용하자`

출저 : https://www.youtube.com/watch?v=RsUTolCVm_E




