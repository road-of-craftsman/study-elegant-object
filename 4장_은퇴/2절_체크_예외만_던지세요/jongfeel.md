### 4.2 Throw only checked exceptions

모든 예외는 checked exceptions 여야 한다.
다양한 예외 타입을 만드는 것도 좋지 않은 생각이다.

IOException은 호출하는 쪽에서 처리하는게 좋고
메서드 안에서 try catch로 안전하게 처리하는 경우 실제로는 안전하지 않은 방법이다.

안좋은 예

``` java
public int length(File file) {
    try {
        return content(file).length();
    } catch (IOExcpetion ex) {
        // 이 예외에 대해 어떤 처리를 해야 하며
        // 바로 여기에서 예외를 해결하거나 
        // 더 상위 레벨로 전달해야 한다.
    }
}
```

책임을 클라이언트로 전파하면서(escalating) 안전하지 않은 방식으로 진행
예외를 위로 올리게 문제를 더 높은 레벨로 확대시킴.

``` java
public int length(File file) throws IOException {
    return content(file).length();
}
```

checked 예외는 예외를 발생시킬 수 있다는 사실을 무시할 수 없다.
상위로 전파하기 위해 매서드 시그니처에 선언해야 한다.
checked 예외는 항상 가시적이어야 하고
length() 메서드를 이용하는 동안에는 content()라는 해롭고 안전하지 않은 메서드를 다루고 있다는 사실을 기억해야 한다.

unchecked 예외는 무시할 수 있고 예외를 잡지 않아도 됨
unchecked 예외는 상위로 전파할 수 있지만 예외 처리를 강요하지 않음
IllegalArgumentException은 unchecked 예외임

``` java
public int length(File file) throws IOExcpetion {
    if (!file.exists()) {
        throw new IllegalArgumentException(
            "File doesn't exist; I can't count its length."
        );
    }
    return content(file).length();
}
```

#### 4.2.1 Don't catch unless you have to (꼭 필요한 경우가 아니라면 예외를 잡지 마세요)

모든 예외를 잡아서 안전하게 만들지, 상위로 문제를 잔파할지 선택
빠르게 실패하기와 안전하게 실패하기의 차이점을 예외 원칙에도 동일하게 적용 가능.

``` java
public int length(File file) {
    try {
        return content(file).length();
    } catch (IOException ex) {
        return 0;
    }
}
```

위 예제에서 length() 메서드는 안전하게 처리함
하지만 문제를 은폐함으로써 length() 메서드를 사용하는 모든 곳에서는 안 좋은 품질의 서비스가 제공됨.
당장 프로그램이 종료되는 문제는 피할 수 있지만, 0이라는 값을 통해서 구체적인 오류에 대해 디버깅 하기는 어려움

#### 4.2.2 Always chain exceptions

``` java
public int length(File file) throws Exception {
    try {
        return content(file).length();
    } catch (IOException ex) {
        throw new Exception("길이를 계산할 수 없다.", ex);
    }
}
```

예외를 잡은 즉시 예외를 던지는 방법, 원래의 문제를 새로운 문제로 대체함으로써 문제가 발생했다는 사실을 무시하지 않는다.
만약 ex를 포함하지 않고 예외를 발생시킨다면 나쁜 프랙티스가 됨.

> throw new Exception("길이를 계산할 수 없다.");

메시지의 수준이 낮아서 정보를 전달하는데 충분하지 않다면 예외 체이닝으로 더 많은 정보를 담아서 전달.

#### 4.2.3 Recover only once

빠르게 실패하기 관점에서는 예외 발생시에 복구를 하면 안됨, 빠르게 실패하기에서는 복구라는 개념 자체가 존재하지 않음.
아래 예제는 NULL을 반환하는 안티 패턴과 유사

``` java
int age;
try {
    age = Integer.parseInt(text);
} catch (NumberFormatExcpetion ex) {
    // 여기에서 발생한 예외를 '복구'한다
    age = -1;
}
```

복구가 필요한 시점은 가장 최상위 시점, main 메서드에서 뿐임.

#### 4.2.4 Use aspect-oriented programming

``` java
public String content() throws IOException {
    int attempt = 0;
    while (true) {
        try {
            return http();
        } catch (IOException ex) {
            if (attempt >= 2) {
                throw ex;
            }
        }
    }
}
```

예외가 던져지면 3번의 재시도를 하는 코드, 하지만 복구 코드가 있으므로 안티 패턴이기는 함.
한가지 해결 방법은 aspect-oriented programming으로 해결

``` java
@RetryOnFailure(attempts = 3)
public String content() throws IOException {
    return http();
}
```

실패 재시도 코드 블록을 관점이라고 부름, 제어를 위임 받아서 content()를 언제, 어떻게 호출할지 결정하는 객체를 의미.

#### 4.2.5 Just one exception type is enough

예외 체이닝을 한다면 무엇을 해야 할지 결정하기 위한 목적의 예외가 아니라 다시 던지기 위해서만 잡으므로 예외 실제 타입에 대해서는 신경 쓸 필요가 없음.
또 예외를 사용할 일이 없기 때문에 타입 정보도 필요하지 않음.
어떤 예외라도 담을 수 있는 예외 객체면 되므로 많은 타입이 필요로 하지 않음.

### In my opinion

확실히 예전의 내가 작성한 코드를 떠올려보면 예외를 처리하는 과정에서 메시지만 출력하고 처리만 했지 이걸 상위로 올려서 예외가 발생한 사실을 크게 알리지 않았던 것 같다.
오히려 책에서 설명한 안전하게 처리하기 위해 try catch를 많이 사용했기에
앞으로는 예외를 적극적으로 알리는 방법으로 해서 테스트 코드를 더 빡세게(?) 작성하는 방향으로 하는 게 좋겠다는 생각도 든다.