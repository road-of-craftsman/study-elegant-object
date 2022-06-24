# 4.3 final이나 abstract이거나

## 정리

### 1. 상속의 문제점

- 상속의 계층구조가 높아질수록 객체들의 관계를 이해하기 어려워진다.
- 상속의 문제점은 객체의 복잡한 관계도 문제이지만 근본적인 문제는 `가상메서드(virtual method)`이다.

```java
class Document {
	public int length() {
		return this.content().length();
	}

	public byte[] content() {
			// 문서의 내용을
			// 바이트 배열로 로드한다
	}
}

class EncrypteDocument extends Document {
	@Override
	public byte[] content() {
		// 문서를 로드해서,
		// 즉시 복호화 하고,
		// 복호화한 내용을 반환한다. 
	}
}
```

설계만 보았을 때는 큰 문제점은 없어보인다. 하지만 Document 클래스의 content() 메서드를 오버라이딩했기 때문에 length() 메서드의 행동이 변경되었다. 변경된 메서드는 사용자가 기대했던 값을 반환하지 않을 수 있다.  

코드를 살펴보면 일반적인 상속과 달리, 메서드 오버라이딩은 부모가 자식의 코드에 접근하는 것을 가능하게 한다. 이런 정반대의 사고 방식은 상식에 어긋난다. 

### 2. final이거나 abstract

- 클래스와 메서드를 `final`이나 `abstract` 둘 중 하나로만 제한하면 상속의 `가상 메서드`로 인한 문제가 발생할 가능성을 없앨 수 있다.
- final 클래스는 상속을 통해 수정이 불가능하다. 불투명하고 독립적이며 자신이 어떻게 행동해야 하는지 알고있다. (== 블랙박스)
- abstract 클래스는 불완전하다. 스스로 행동할 수 없기 때문에 누군가의 도움이 필요하며 일부 요소가 누락되어 있다. (== 글래스 박스)
- final도 abstract도 아닌 클래스는 블랙박스나 글래스 박스 둘 중 어느 쪽에도 해당되지 않는다.
- 상속이 적절한 경우는 클래스의 행동을 `확장하지 않고 정제할 때` 이다.
    - 확장 : 새로운 행동을 추가해서 기존의 행동을 부분적으로 보완하는 일
    - 정제 : 부분적으로 불완전한 행동을 완전하게 만드는 일

**final**

```java
//예시 1
final class Document {
	public int length() { /* 코드는 동일 */}
	public byte[] content() { /* 코드는 동일 */ }
}
```

`예시 1`은 클래스 앞에 final 수정자를 붙여서 컴파일러에게 이 클래스의 어떤 메서드도 오버라이딩이 불가능하다는 사실을 알린다.  
그런데 EncryptedDocument는 Document지만 final 클래스인 Document를 상속받을 수 없다. 이러한 문제를 해결하기 위해서는 인터페이스를 추가해야 한다. 

```java
//예시 2
interface Document {
	int length();
	byte[] content();
}

final class DefaultDocument implements Document {
	@Override
	public int length() { /* 코드는 동일 */ }
	@Override
	public byte[] content() { /* 코드는 동일 */ }
}

final class EncryptedDocument implements Document {
	private final Document plain;
	EncryptedDocument(Document doc) {
		this.plain = doc;
	}

	@Override
	public int length() {
		return this.plain.length();
	}
	@Override
	public byte[] content() {
		byte[] raw = this.plain.content();
		return /* 원래 내용을 복호화 한다. */
	}
}
```

`예시 2` 는 Document 인터페이스를 따로 두어 DefaultDocument, EncryptedDocument가 이를 구현하게 변경하였다. 그리고 상속이 불가능한 DefaultDocument를 재사용하기 위해 캡슐화를 선택하였다.

DefaultDocument, EncryptedDocument 모두 final 클래스로 구현되었기 때문에 확장이 불가능하다. 

**abstract**

```java
abstract class Docuemnt {
	public abstract byte[] content();
	public final int length() {
		return this.content().length();
	}
}

final class DefaultDocument extends Document {
	@Oveeride
	public byte[] content() {
		// 디스크에서 내용을 로드한다. 
	}
}
```

길이를 계산하는 방법만 알고있는 미완성의 Document 클래스(== 글래스 박스)를 DefaultDocument 클래스를 추가하여 정제하였다. 이 설계에서도 length() 메서드의 행동이 매번 변경되지만 첫 예시와 다르게 사용자가 이를 의식하고 있다는 점이 다르다. 

### 3. 결론

- 의도를 명확하게 표현해야 한다.
- 올바른 방식으로 설계하거나, 설계하지 말아야 한다. 그 사이에는 어떤 것도 있으면 안된다.
- 상속의 가상메서드는 사용자를 혼란하게 만들 수 있다.
- final과 abstract를 이용하면 가상 메서드가 주는 혼란을 피할 수 있다.

## 느낀점

저자가 무슨 말을 하고싶은 건지는 이해했고,, 맞는 말인 것 같다. 

상속을 아예 피하지는 않을 것 같고, 적절한 상황에 맞게 선택해서 사용할 것 같다.
