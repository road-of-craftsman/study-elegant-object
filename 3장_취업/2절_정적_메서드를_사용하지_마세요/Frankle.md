# 3.2 정적 메서드를 사용하지 마세요
- OOP 관점에서 정적 메서드를 포함해 static 키워드는 악이다.
- 정적 메서드는 절차지향의 산물이며, 유지보수를 어렵게 만든다.
- 유명 라이브러리에서 조차 정적 메서드로 만들어진 수많은 코드들을 볼 수 있는데, 직접 객체로 감싸서 고립시킬 수 있다.
```java
class FileLines implements Iterable<String> {
    private final File file;
    public Iterator<String> iterator() {
        return Arrays.asList(FileUtils.readLines(this.file).iterator());
    } 
}
Iterable<String> lines = new FileLines(f);
```

## 유틸리티 클래스
- 절차적인 프로그래머들이 OOP 영토에서 거둔 승리의 상징..?!
- 끔찍한 안티패턴, 절대 사용하지 말 것..!

---
- 정적 메서드는 앞에서 설명했던 것들을 불가능하게 만들고, 객체들을 조합하기 어렵다.
- 결론적으로 static을 사용하지 말 것.

## 생각
OOP 관점에서 해롭다는 것은 어느정도 알겠지만.. 단시간에 이것들을 다 지키면서 개발을 할 수 있을지는 모르겠다. 일단.. 머릿속에 각인이 되었으니, 나중에 필요한 상황이 온다면 적절한 판단일지 고민을 해볼 것 올 것 같다.