#### 3.7 Avoid type introspection and casting

type introspection은 reflection이라는 더 포괄적인 용어로 불리는 여러 가지 기법들 중 하나.
리플렉션은 매우 강력한 기법이지만 동시에 코드를 유지보수하기 어렵게 만드는 매우 너저분한 기법이다.

``` java
public <T> int size(Iterale<T> items) {
    if (items instanceof Collection) {
        return Collection.class.cast(items).size();
    }
    int size = 0;
    for (T item : items) {
        ++size;
    }
    return size;
}
```

여기서 리플렉션 기법을 사용하는 건 타입에 따라 객체를 차별하는 것으로 볼 수 있고
OOP의 기본 사상을 심각하게 훼손시키는 것.
런타임에 객체의 타입을 조사(introspect)하는 것은 클래스 사이의 결합도가 높아짐.

올바른 방법으로 설계한다면 다음과 같이 해볼 수 있다.

``` java
public <T> int size(Collection<T> items) {
    return items.size();
}

public <T> int size(Iterable<T> items) {
    int size = 0;
    for (T item : items) {
        ++size;
    }
    return size;
}
```

#### In my opinion

여기서 저자의 주장에 동의하는데 Collection은 Iterable을 상속 받은 인터페이스인데도 굳이 instanceof로 물어보고 casting을 시도하는 것 자체가 잘못된 메서드 설계라는 걸 반증한다.
따라서 interface를 나눠서 메서드 오버로딩을 사용하는 게 아주 좋아 보인다.