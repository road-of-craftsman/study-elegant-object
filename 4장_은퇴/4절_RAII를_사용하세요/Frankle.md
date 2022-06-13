# 4.4 RAII를 사용하세요

- RAII(Resource Acquisition Is Initialization) - 리소스 획득이 초기화
- RAII는 C++에 존재하는 매우 강력한 기법이지만, GC로 객체를 제거하는 Java에서는 사라진 개념
- RAII의 요령은 객체가 살아있는 동안에만 리소스를 확보하고, 객체가 더 이상 필요하지 않으면 리소스를 해제하고 객체를 파괴함
- Java는 객체가 더 이상 사용되지 않을 때 객체를 제거하는 작업을 백그라운드(GC)를 통해 진행한다
- 더 이상 객체가 필요하지 않는 상황이여도 RAII처럼 객체를 즉시 파괴하지는 않는다.

```java
int main() {
    try (Text t = new Text("/tmp/test.txt")) {
        t.content();
    }    
}
```
Java에서도 위와 같이 RAII과 유사한 방식으로 처리를 할 수 있다. 이 방법을 사용하기 위해서는 Text 클래스가 AutoCloseable 인터페이스를 구현해야 한다.  
파일, 스트림, 데이터베이스 커넥션 등 실제 리소스를 사용하는 모든 곳에서는 RAII을 사용하길 권장한다.


# 느낀점
바로 어제 OOME와 직면했는데..  
우리의 자원은 소중하다. GC가 되기 전에 리소스를 해제해야 한다면 try-with-resource 기법을 사용해야겠다.