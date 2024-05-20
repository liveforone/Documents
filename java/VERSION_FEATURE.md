# 자바 버전별 특징

## Java8

- 날짜, 시간 API 추가
- Unsinged 추가
  - `Integer.parseUnsignedInt("4294967295");`, `Long.parseUnsignedLong("18446744073709551615");`과 같이 사용한다.
- 람다식 추가
- stream api 추가
- Null safe를 위한 Optional 추가

## Java11

- G1 GC가 기본 GC로 지정되었다.
  - G1 GC는 점진적으로 메모리를 회수하는 알고리즘의 GC이다.
  - GC는 사용하는 환경과 타켓하는 모델에 따라 잘 맞는 GC는 선택하는 것이 올바르다.
- 람다 매개변수에 대해서 `var`키워드 사용가능
  - `(var first, var second) -> blabla..`

## Java17

- sealed class 추가
  - 상속을 받을 수 있는 범위를 가지는 클래스이다.
  - `sealed`로 선언 후 `permits` 키워드 뒤에 상속받는 것을 허용할 클래스를 나열한다.
  - 이 클래스들은 어떤 클래스던 상속이 가능한 `non-sealed`클래스로 선언하거나, 상속이 더이상 불가능한 `final`클래스로 선언한다.
