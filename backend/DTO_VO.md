# Dto와 Vo

## DTO

- 클라이언트로 받는 데이터 객체와 클라이언트에게 리턴할 객체로 dto라는 이름을 자주 사용한다.
- 그런데 이러한 데이터 전달 객체는 DTO 뿐만 아니라 VO 라는 것도 있다.
- 일반적인 상황에서는 DTO가 맞다.
- 계층간 데이터를 전달하며, 내부 값이 가변성을 띄고, getter/setter외에 다른 로직이 필요하지 않다면 DTO가 적합하다.

## VO

- 그런데 서버에서 클라이언트로 데이터를 전달할 객체를 정의할 때, 이 객체는 다음과 같은 특성을 주로 띈다.
- 내부 값들은 불변성을 띄며, getter/setter외에 다른 로직이 있기도 하고, 강하게 read-only 성향을 띄며, 내부 필드가 같은 두 객체를 비교할 때 동등하다고 표현할 수 있다면 이때는 VO가 적합하다.
- 이러한 이유로 VO에는 `equals()`와 `hashCode()`를 정의하곤 한다.

## 이게 그렇게 중요하나?

- 딱히 그렇게 중요한 것 같진 않다.
- 그러나 객체의 종류에 따라 위치와 쓰임새가 다르다.
- 이 때문에 가능하면 구분을 해주는 것이 좋다.
- 특히나 위치의 경우 필자는 DTO는 도메인 디렉토리 내부에 상위 디렉토리로 선언하고
- VO는 엔티티와 함께 있는 디렉토리 안에 VO 디렉토리를 선언하는 편이다.
