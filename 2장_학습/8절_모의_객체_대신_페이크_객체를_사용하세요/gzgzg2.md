# 2.8 모의 객체(Mock)대신 페이크 객체(Fake)를 사용하세요

## 정리

- 모킹 대신 **‘페이크 객체(fake Object)’** 를 사용해라
- ‘페이크’ 클래스를 사용하면 테스트를 더 짧게 만들 수 있어서 유지보수성이 향상된다.
- 페이스 클래스를 만족하도록 테스트를 작성하게 된다면 모킹과 별 다를게 없다.
- 페이크 클래스를 만들다보면 필연적으로 인터페이스의 설계를 고민하게 된다.
- 모킹은 가정(assumption)을 사실(facts)로 전환시키기 때문에 단위 테스트를 유지보수하기 어렵다.
- 모킹은 클래스의 구현과 관련된 세부사항을 테스트와 결합시킨다.
- 객체 내부의 구현 세부사항을 알면 테스트가 취약해지고 유지보수가 어려워진다.

**Fake Object** 

```java
interface Exchange {
	float rate(String origin, String target);

	// Fake 객체
	final class Fake implements Exchange {
		@Override
		float rate(String origin, String target) {
			return 1.2345;
		}
	}
}

// Fake 객체를 사용한 테스트 코드 예시
Exchange exchange = new Exchange.Fake();
Cash dollar = new Cash(exchange, 500);
Cash euro = dollar.in("EUR");
assert "6.17".equals(euro.toString())
```

```java
💡 Fake 객체는 인터페이스와 함께 제공한다.
💡 테스트는 클래스의 공개된(public) 메서드를 변경하지 않는 이상 실패해서는 안된다.
```

## 느낀점

Mock을 사용하여 테스트 코드를 작성해본 경험이 적기 때문에 Fake 객체와의 장단점을 정확하게 알지는 못하지만, 책의 내용만 보았을 때는 Fake 객체가 조금 더 매력적이게 다가왔다. 

그런데 마지막 장에서 큰 규모의 프로젝트에서 사용했던 Fake 객체에 대해 간략하게 얘기해주는데.. 큰 Fake 객체 대신 작고 깔끔한 단위테스트를 얻었다는 얘기 같은데,, Fake 객체가 너무 커져도 유지보수가 힘들지 않을까? 🤔 

너무 커지면 그건 리팩토링의 신호인가..? 흠 .. 실제로 경험해보기 전까지는 완벽한 비교가 어려울 것 같다. 함 해봐야지 ..
