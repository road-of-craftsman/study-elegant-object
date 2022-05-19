# 2.6 불변 객체로 만드세요

## 정리

### 1. 불변 객체와 가변 객체

- 클래스를 **불변 클래스(Immutable class)** 로 구현하면 유지보수성을 크게 향상시킬 수 있다.
- **가변 클래스(mutable class)** 보다 불변 클래스를 이해하는 것이 훨씬 수월하다.
- 불변 객체는 필요한 모든 것을 내부에 캡슐화하고 변경할 수 없도록 통제한다.
- 불변 객체를 수정해야 한다면 프로퍼티를 수정하는 대신 새로운 객체를 반환해야 한다.
- 그러나 불변 객체를 사용하면 지연 로딩(Lazy Loading)을 구현하는데에 있어 제약이 생긴다.

<br>

```java
// 가변객체
class Cash {
	private int dollars;

	public void setDollars(int val) {
		this.dollars = val;
	}
	
	 public int mul(int factor) {
		this.dollars *= factor;
	}

}

//불변 객체
class Cash {
	private final int dollars;

	Cash(int val) {
		this.dollars = val
	}

	public Cash mul(int factor) {
		return new Cash(this.dollars * factor);
	}
}
```

### 2. 식별자 가변성(Identity Mutability)

- 불변객체는 식별자 가변성(identity mutability) 문제가 발생하지 않는다.
- 가변 객체는 식별자가 변경될 수 있기 때문에 안전하지 않다.

<br>

```java
Map<Cash, String> map = new HashMap<>();
Cash five = new Cash("$5");
Cash ten = new Cash("$10");
map.put(five, "five");
map.put(ten, "ten");
five.mul(2);
System.out.println(map); // {$10=>"five", $10=>"ten"}
```

분명 동일하지 않은 두 객체를 map에 추가하였다. 그런데 이후 five 객체의 값을 변경하여 ten과 동일한 값을 가지게 됐으므로 map에는 중복된 식별자의 엔트리가 존재하게 된다. 


### 3. 실패 원자성(Failure Atomicity)

- 불변 객체는 **실패 원자성(Failure Atomicity)** 의 특성을 가진다.
- 실패 원자성이란 완전하거나 아니면 실패하거나 둘 중 하나만 가능한 특성이다.
- 가변 객체는 상태값을 변경하는 메서드를 실행할 때 발견하기 어려운 에러가 발생할 수 있다.
    - 예를 들면 메서드 실행 도중 절반만 값이 변경되어 원치않은 결과를 낳게되는 현상
- 불변 객체는 내부의 어떤것도 수정할 수 없기 때문에 가변 객체의 결함이 발생하지 않는다.
- 불변 객체의 정의에 따르면 모든 불변 객체는 ‘원자적'이다.


### 4. 시간적 결합(Temporal Coupling)

- 불변 객체는 **시간적 결합(Temporal Coupling)** 을 제거할 수 있다.
- 상태를 변경할 수 있는 가변객체는 실행 순서에 따라 서로 결합되어 있다.
- 불변 객체는 생성 당시 완전한 상태로 초기화하기 때문에 시간적 결합이 발생하지 않는다.

<br>


```java
//가변 객체의 예시 - 1
Cash price = new Cash();
price.setDollars(29);
price.setCents(95);
System.out.println(price); // "$29.95"

//가변 객체의 예시 - 2
Cash price = new Cash();
price.setDollars(29);
System.out.println(price); // "$29.00"
price.setCents(95);

//불변 객체의 예시
Cash price = new Cash(29, 95)
System.out.println(price); // "$29.95"
```


### 5. 부수효과 제거(Side effect-free)

- 가변 객체는 어디서든 수정할 수 있다. 때문에 어느 곳에서 어떤 값을 수정하여 부수효과가 발생하였는지 이해하는데에 많은 시간이 소요된다.
- 불변 객체는 객체를 수정할 수 없기 때문에 코드가 제대로 동작하지 않는 경우에도 부수효과가 발생한 위치를 찾을 필요가 없다.


### 6. NULL 참조 없애기

- 불변 객체는 생성과 동시에 초기화 하기 때문에 Null 참조 문제를 피할 수 있다.
- 생성과 동시에 초기화 해야하는 특성 때문에 불변 객체는 **작고 견고하고 응집도 높은** 클래스로 생성할 수 있다.
- 가변 객체는 객체를 생성할 당시 모든 값을 초기화하는 경우가 드물다. 매번 Null 체크를 하는 것이 아니라면 NullPointerException이 발생할 확률이 높아진다.
- 실제값이 아닌 Null 값을 참조하면 유지보수성이 저하된다.


### 7. 스레드 안전성

- 스레드 안전성이란 여러 객체가 동시에 객체를 사용하여도 그 값을 항상 예측할 수 있는 것을 말한다.
- 여러개의 병렬 스레드 안에서 수정이 가능한 객체에 동시에 접근하게 되면 값을 예측할 수 없다.
- 불변 객체는 값을 수정할 수 없기 때문에 스레드 Safe 하다.
- 가변 객체도 동기화를 적용하여 스레드에 안전하게 만들 수 있지만 성능이 상당히 저하된다.


### 8. 더 작고 더 단순한 객체

- 불변 객체는 단순성을 가진다. 단순성은 유지보수성으로 해석할 수 있다.
- 불변 객체는 생성자에서만 값을 초기화 할 수 있기 때문에 가변 객체보다 작다.
    - 생성자가 커지면 더 작은 클래스로 분리해야 한다.
- 불변 객체는 클래스를 깔끔하고 더 짧게 만든다.

<br>

## 느낀점

읽으면서 웬지 모를 반항심에 가변 객체를 사용해도 되는 이유를 하나라도 찾아보려 했는데 없었다. 저자에게 완전히 설득당한 챕터이다. 이번 챕터는 불변 객체의 장점과 가변 객체의 단점을 설명한다. 그런데 계속 강조하는 단순한 클래스, 작은 클래스도 중요한 핵심인 것 같다. 

불변 객체는 많은 장점을 가지고 있다. 그런데 결국 클래스의 역할이 제대로 분리되지 않아, 클래스가 커지면 모든 장점이 무색해진다는 생각이 들었다.
