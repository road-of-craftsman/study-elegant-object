# 4.1 절대 NULL을 반환하지 마세요

## 정리

### 1. NULL을 반환하는 객체는 신뢰를 상실한다.

- 객체를 장애를 가진 존재로 취급해서는 안된다.
- NULL을 반환하는 객체는 신뢰하기 어렵다. 반환객체를 사용할 때 NullPointException에 자유롭지 못하기 때문이다.


```java
public String title() {
	if (this.titile == null) {
		return null;
	}
		return "Elegent Objects";
}

String title = x.title();
print(title.length()); // title()이 null을 반환할 경우 NPE 발생
```


### 2. 객체는 자신의 행동을 전적으로 책임져야한다.

- 객체는 자신만의 상태를 가지는 살아있는 유기체이다.
- 신뢰란 객체가 자신의 행동을 전적으로 **책임지고(responsible)** 우리가 어떤 식으로든 간섭하지 않는다는 의미가 담겨있다. 즉 객체는 자신의 맡은 일을 수행하는 방법을 스스로 결정해야 한다.
- 객체에게 아무런 말도 하지 않은 채 사용자 마음대로 예외를 던지는 것은 객체를 존중하지 않는 행동이다.

```java
String title = x.title();
if(title == null) {
	print("Can't print; It's not a title.")
}
```

이처럼 반환값을 검사하는 방식은 애플리케이션에 대한 신뢰가 부족하다는 명백한 신호이다.  반환된 객체를 실제로 사용하거나 메세지를 전송하기 전에 항상 위 코드처럼 NULL이 아닌지를 체크해야할 것이다. 


### 3. 빠르게 실패하기 vs 안전하게 실패하기

- 안전하게 실패하는 방식은 버그가 발생하더라도 소프트웨어가 계속 실행될 수 있게 안전장치를 마련한다.
- 빠르게 실패하는 방식은 문제가 발생하면 곧바로 실행을 중단하고 예외를 던진다.
- 안전하게 실패하는 방식보다 빠르게 실패하는 방식이 버그를 명확하게 찾아낼 수 있다.  대신 실패를 분명하게 하여라

### 4. NULL의 대안

- 메서드를 존재여부를 확인하는 메서드와 반환하는 메서드로 분리하기
- 객체 컬렉션을 반환하기
- 널 객체(null Object) 디자인 패턴 사용하기

```java
1. 메서드를 두 가지로 분리하기
public boolean exists(String name) {
	if (/*데이터베이스에서 발견하지 못했다면*/) {
		return false;
	}
		return true;
}

public User user(String name) {
	return /*데이터베이스로부터*/;
}

💡 1번 방법은 데이터 베이스에 요청을 두 번 전송하기 때문에 비효율적

2. 객체 컬렉션을 반환하기
public Collection<User> users(String name) {
	if(/*데이터베이스로부터 발견하지 못했다면*/) {
		return new ArrayList<>(0);
	}
		return Collections.singleton(/*데이터베이스로부터*/);
}

3. 널 객체(null object) 반환하기
class NullUser implements User{
	private final String label;
	NullUser(String name) {
		this.label = name;
	}
	@Override
	public String name() {
		return this.label;
	}
	@Override
	public void raise(Cash salary) {
		throw new IllegalStateException("제 봉급을 인상할 수 없습니다. 저는 스텁(stub)입니다.");
	}
}

💡 Null 객체는 원래 반환하기로 했던 객체와 동일한 타입이여야 한다. 
```

## 느낀점

본인도 NULL을 다루는 걸 매우 싫어해서 전부 공감이 되었다. 그런데 Optional을 지양하라는 건 받아드리기 힘들었다. 개인적으로 Optional의 **orElseThrow()** 을 매우 사랑한다. 필자가 선호하는 빠르게 실패하기를 지향하는 편인데 **orElseThrow()**는 빠르게 실패하는 코드를 간결하게 만들어주는 것을 도와준다고 생각한다. 그런데 사용하면 안될 이유가 따로 더 있다면 고민해봐야할 것 같다. 

실무에선 NULL을 반환하는 대신 값이 없는 객체를 반환하거나 예외를 던지는 방법을 주로 쓰는 것 같다. 그런데 책을 읽어보면 값이 없는 객체는 객체지향 세계에선 피해야할 듯하다.
