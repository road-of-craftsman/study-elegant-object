# 4.4 RAII를 사용하세요

## 정리

- RAII(Resorce Acquisition Is Initialization)이란 리소스 획득을 초기화하는 개념이다.
- 객체가 더 이상 필요하지 않으면 리소스를 해제하고 객체를 파괴해야 한다.
- 그렇지만 Java에서는 파괴자가 존재하지 않기 때문에 RAII 기법을 적용하기 힘들다.
- java7 이상에선 try-with-resources 기법을 사용하여 RAII과 유사한 처리를 할 수 있다.

**예시**

```java
int main () {
	try(Text t = new Text("/tmp/test.txt")) {
		t.content();
	}
}
```

try 블록이 끝날 때 객체 t를 파괴하는 대신에 t의 close() 메서드를 호출한다.  
이 방법을 사용하기 위해서는 Text 클래스가 Closable 인터페이스를 구현하게 하면 된다.

### 📌 중요

파일, 스트림, 데이터베이스 커넥션 등 실제 리소스를 사용하는 곳에서 RAII를 사용할 것을 추천한다.  
C++에서는 파괴자를 사용하고 Java에서는 AutoCloseable을 사용하여라

## 느낀점

중요한 내용인 것 같다 메모리관리를 조금 더 적극적으로 해봐야겠다.

그런데 문득 드는 생각은 GC가 있어서 너무 편하지만, 이런 내용을 읽을 때마다 편한 만큼 잃은 것도 많다는 생각이..? 든다
