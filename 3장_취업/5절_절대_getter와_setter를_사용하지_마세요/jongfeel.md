### 3.5 Never use getters and setters

``` java
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

getter, setter가 있는 Cash는 클래스가 아니라 단순 자료(data structure)구조이다.
이유: 생성자 없음, 메서드 이름 짓기 잘못됨, 가변 객체

#### 3.5.1 Objects vs data structures

class (object)

  - class는 encapsulation으로 내부 데이터를 숨기고 요청에 응답하는 방법
  - 객체는 불투명함. black box
  - 능동적이고 살아 있음

struct (data structures)

  - struct는 의사소통이 없고 개성이 없는 단순한 데이터 덩어리
  - 자료구조는 투명하고, glass box
  - 수동적이고 죽어 있음

객체를 사용해야 하는 이유, 유지보수성
가시성의 범위를 축소해서 단순화시키면 특정 시점에 이해해야 하는 범위가 작아지고
소프트웨어의 유지보수성이 향상되고 이해하고 수정하기도 쉬워짐

#### 3.5.2 Good intentions, bad outcome

getter, setter는 캡슐화 원칙을 위반하기 위해 설계되었음
겉으로는 메서드 처럼 보이지만 데이터데 직접 접근하는 방식임
이런 형태는 데이터가 무방비로 노출되어 있는 형태임.

#### 3.5.3 It's all about prefixes

메서드에 get을 붙이는 것은 객체와 대화를 원하지 않는 방식
getDollars()는 "데이터 중에 dollars를 찾은 후 반환하세요"
dollars()는 "얼마나 많은 달러가 필요한가요?"

#### In my opinion

getter, setter에 대해서는 다른 책에서도 본 것이 있어서
내부 데이터를 setter, getter로만 할 거면 객체지향적인 객체가 아니라
그냥 데이터 객체라는 점은 수긍이 간다.