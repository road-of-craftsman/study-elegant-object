### 4.4 Use RAII

리소스 획득이 초기화 (Resource Acquisition Is Initialization, RAII)
즉 객체가 살아 있는 동안에만 리소스를 확보하는 방법, 객체가 더 이상 필요하지 않으면 리소스를 해제하고 객체가 소멸되는 방식.
C++에서는 구현 가능하지만 Java와 같은 고수준 언어는 구현이 안됨.

하지만 Java7 부터 try-with-resources 기법을 통해 RAII와 유사한 처리가 가능

``` java
int main() {
    try (Text t = new Text("/tmp/test.txt")) {
        t.content();
    }
}
```

try 블록을 벗어나면 Text 객체 t의 close() 메서드를 호출함. Text 클래스는 Closable 인터페이스를 구현해야 함.

### In my opinion

나는 C# 언어도 주력으로 쓰고 있으므로 C#의 using문과 매우 흡사하다고 느꼈고 인터페이스 역시 IDisposable 인터페이스의 Dispose() 메서드를 호출해 준다는 점에서 동일하다는 점을 발견했다.
어쨌든 필요할 때만 호출해서 사용하고 바로 해제하자는 식의 개념은 C#에서도 있는 예제이므로 자주 사용하면 좋을 것 같다.