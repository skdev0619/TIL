# Annotation

## CustomAnnotaion 만들기 실습
@Inject 만들어서 의존성 주입하는 커스텀 어노테이션 만들기

- 실행파일
```
public class AnnotaionTest {
    public static void main(String[] args) {
        BookService service = ContainerService.getObject(BookService.class);
        System.out.println(service);
        System.out.println(service.repository);
    }
}
```
- 테스트 객체
```
BookRepository.java
public class BookRepository {}

BookService.java
public class BookService {
    @Inject
    BookRepository repository;
}

Inject.java
@Retention(RetentionPolicy.RUNTIME)
public @interface Inject {}
```

- 리플렉션 이용한 의존성 주입
```
ContainerService.java
public class ContainerService {

    public static<T> T getObject(Class<T> classType){
        T instance = createInstance(classType);
        Arrays.stream(classType.getDeclaredFields()).forEach(f -> {
            if(f.getAnnotation(Inject.class) != null){
                Object fieldInstance = createInstance(f.getType());
                f.setAccessible(true);

                try {
                    f.set(instance, fieldInstance);
                } catch (IllegalAccessException e) {
                    throw new RuntimeException(e);
                }
            }
        });
        return instance;
    }

    private static <T> T createInstance(Class<T> classType)  {
        try {
            return classType.getConstructor(null).newInstance();
        } catch (InstantiationException | IllegalAccessException | InvocationTargetException | NoSuchMethodException e) {
            throw new RuntimeException(e);
        }
    }
```







출처 : 백기선님 더 자바, 코드를 조작하는 다양한 방법 참고