# 3.6 부 ctor 밖에서는 new를 사용하지 마세요

### 1. 강한 결합도

- 객체가 필요한 의존성을 직접 생성하면 그 객체는 생성한 객체와 강하게 결합된다.
- 클래스가 클 경우 하드코딩된 의존성이 소프트웨어를 테스트하고 유지보수하기 어렵게 만든다.
- 부 ctor을 제외한 어느 곳에서도 new를 사용할 수 없도록 금지시킨다면 객체들은 상호간에 충분히 분리되고 테스트 용이성과 유지보수성을 크게 향상 시킬 수 있다.

<br>

**하드코딩된 의존성 예시**

```java
class Cash {
	private final int dollars;

	public int euro() {
		return new Exchange().rate("USD", "EUR") * this.dollars; 
	} 
}
```

위 클래스는 euro() 메서드 안에서 new 연산자를 이용하여 Exchange 인스턴스를 생성하고 있다. 이는 하드코딩된 의존성으로 Exchange 클래스에 직접적으로 연결되어 있기 때문에, 의존성을 끊기 위해서는 Cash 클래스의 내부 코드를 변경할 수밖에 없다. 

<br>

**의존성 제거 예시**

```java
class Clash {
	private final int dollars;
	private final Exchange exchange;
	
	Cash(int value) { // 부 ctor
		this(value, new NYSE()); 
	}

	Cash(int value, Exchange exch) {
			this.dollars = value;
			this.exchange = exch;
	}

	public int euro() {
		return this.exchange.rate("USD", "EUR") * this.dollars;
	}
	
}
```

객체가 직접 필요한 의존성을 생성하는 대신에 ctor을 통해 의존성을 `주입(inject)`하는 방식으로 변경하였다. 

그리고 편의를 위해 부 ctor을 추가하여 기본값으로 주입할 인스턴스를 지정하였다.

## 느낀점

스프링을 사용하는 개발자이기 때문에 아주 익숙한 내용 .. 전부 공감된다. 

생성자가 아니어도 의존주입이 가능하지만, 생성자 주입이 아닐 경우 발생하는 문제점들이 꽤 많다. 

부 ctor은 기본 값으로 주입할 객체가 있을 때 사용하면 좋을 것 같다.
