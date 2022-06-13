### 4.3 Be either final or abstract

상속을 사용하면 virtual method를 사용하게 되고
메서드 오버라이딩을 통해 원하지 않는 동작의 부모 메서드가 자식 메서드를 호출하면서
상속의 기본 속성에 어긋나는 행위가 일어나고 코드를 해석하는 복잡성이 증가함

final class: black box, 상속을 통해 수정할 수 없음, 메서드는 영원히 final
abstract class: glass box, 불완전함. abstract class의 특정 메서드를 오버라이딩 할 수 있지만 다른 메서드는 모두 final

가끔 상속을 사용해야 하고 적절한 경우는 언제?
클래스의 행동을 확장(expand)하지 않고 정제(refine)할 때임.
확장을 통해 새로운 행동을 추가해서 기존의 행동을 부분적으로 보완
정제를 통해 부분적으로 불완전한 행동을 완전하게 만드는 일을 함

``` java
abstract class Document {
    public abstract byte[] content();
    public final int length() {
        return this.content().length;
    }
}
```

Document를 상속 받은 클래스의 경우
length() 메서드가 자신들이 메서드를 사용하는 방법을 명확하게 알고 있다는 가정하에 메서드를 개선하고 있음.

### In my opinion

상속의 해로움에 대해 더 추가적인 설명을 함으로써 일반적인 클래스들로 상속 구조를 만들지 말라는 메시지는 이해가 됨.
보다 잘 설계된 abstract class에 대해서 생각해 보고 설계를 한다면 무분별한 상속 구조는 안 만들 수 있을 것 같긴 하다.