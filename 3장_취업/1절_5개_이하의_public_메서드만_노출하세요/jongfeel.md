### 3.1 Expose fewer than five public methods

#### Summary

public method의 갯수 기준은 5개: 작은 객체를 유지한다면 우아하고, 유지보수가 가능하고, 응집력이 높고, 테스트 하기 용이한 객체가 됨
private 메서드의 갯수는 제외, 그리고 protected 메서드는 포함시켜서 갯수를 제한.
5개의 갯수는 저자의 개인적인 의견, public method 5개 까지가 작은 객체의 최소 단위

#### In my opinion

책에는 언급되어 있지 않지만, 생성자가 포함될 수 있을까? 라는 생각을 해 봤다.
만약 기본 생성자라면 포함 시키지 않을 것 같고, 파라미터가 있는 생성자라면 public method로 쳐서 갯수를 셀 것 같다.