# 3.5 절대 getter와 setter을 사용하지 마세요

```java
class Cash {
    private int dollars;
    
    public int getDollars() {
        return this.dollars;
    }

    public void setDollars(int value) {
        this.dollars = value;
    } 
}
```
- 책에서 거듭 강조했듯이 모든 클래스는 불변해야 한다. 이 클래스는 setter로 데이터에 직접 접근하여 상태를 변경할 수 있으므로 가변 클래스이다. Cash는 클래스가 아닌 자료구조에 가깝고, 이는 용서받기 어렵다.
- 클래스는 어떤 식으로는 멤버에게 접근하는 것을 허용하지도 않고, 노출하지도 않는다.

## 자료구조와 객체의 차이

| 자료구조   | 객체    |
|--------|-------|
| 투명     | 불투명   |
| 글래스 박스 | 블랙 박스 |
| 수동적    | 능동적   |

## OOP로 얻는 유지보수성
- 모든 프로그래밍 스타일의 핵심 목표는 **가시성의 범위를 축소**해서 **사물을 단순화**하는 것이다.
- 특정 시점에 알아야 하는 범위가 작을수록 유지보수성이 향상되며, 코드를 이해하고 수정하기 쉬워진다.

## 객체
- 데이터는 객체 안에 캡슐화되어 있고, 객체는 살아있는 유기체다.
- 객체끼리 서로 연결되어있고, 어떤 일을 수행하기 위해서는 메시지(메서드)를 전송하여 작업을 실행한다.
- OOP에서는 코드가 데이터를 지배하지 않고, 필요한 시점에 객체가 자율적으로 자신의 코드를 실행한다.
- 객체 본인이 무엇을 캡슐화하고 있고, 자료구조가 얼마나 복잡한 지는 오직 객체 스스로만 알고 있어야 한다. (캡슐화)

## getter, setter
- getter와 setter는 OOP 캡슐화 원칙 위반을 부추기는 네이밍이다.

```java
class Cash {
    private int dollars;

    public int getDollars() {
        return this.dollars;
    }

    public int dollars() {
        return this.dollars;
    }
}
```
- `getDollars()`와 `dollars()`는 동일한 역할을 가져 같은 값을 반환한다.
    - `getDollars()`: 데이터 중에 dollars를 찾은 후 반환
    - `dollars()`: 달러 주세요
- get 접두사에 비해 dollars는 객체를 자료구조로 보지 않고, 객체를 존중하여 메시지를 건네는 행동이다.
- 결론은 getter, setter은 OOP 관점에서 클래스를 자료구조로 취급하게 되는 지표에 가깝다.

# 생각
getter, setter 대신 객체에게 메시지를 건네고, 메시지를 받은 객체가 자율적으로 요청을 수행하는 것이 OOP의 핵심 중 하나라고 생각한다.
getter, setter은 데이터에 직접 접근할 수 있어 불변 객체와 거리도 멀어지며, 한 발자국 뒤에서 살펴보면 정말 자료구조로 취급하는 행위인 것 같다.


이를 인지해 getter, setter을 최대한 사용하려 하지 않고 프로그래밍 하고 있지만, 아직까지 getter에서 완전히 벗어나지는 못한 것 같다.