### 2.9 Keep interfaces short; use smarts

#### Summary

하나의 class에서 여러 interface를 구현할 수 있으므로 public method가 많을 수록 우아한 class와 거리가 멀어진다.
만약 interface가 많은 걸 요구하는, 즉 SRP를 위반하는 class를 만들도록 한다면 응집도가 낮아진다.
interface를 유지하면서 interface 안에 smart 클래스를 추가해서 다른 interface를 다시 정의하는 걸 막을 수 있다.

interface를 구현하는 서로 다른 클래스 안에 동일한 기능을 반복해서 구현하고 싶지 않을 때 smart class를 써야 한다.

``` Java
float rate = new Exchange.Smart(new NYSE()).toUsd("EUR");
```

Exchange interface의 정의는 다음과 같다

``` Java
interface Exchange {
...
    final class Smart {
        ...
        private final Exchange origin;
        public float toUsd(String source) {
            ...
        }
    }
}
```

#### In my opinion

이전에는 알지 못했던 새로운 접근 방법이라서 신선했는데, 오히려 interface의 정의와 구현이 함께 있는 smart class는 오히려 가독성을 해치지 않을까? 라는 생각이 더 강하게 들었다.
interface라면 당연히 입력, 출력, 해야 하는 일에 대한 명확한 method의 정의가 중요한데, 그 안에 class를 넣어서 정의와 구현부가 있는게 그렇게 좋아 보이지는 않는다.

만약 나라면 책의 예제 정도의 크기로 interface를 정의하고 구현하는 거라면 별도의 interface로 다시 정의하는게 나을 거라는 생각이다.