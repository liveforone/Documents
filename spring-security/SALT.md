# spring security salt 동작방식

## 이전의 해시 함수의 동작방식

- 값을 암호화 하는 해시함수는 기본적으로 단방향 알고리즘을 갖고 있다.
- 따라서 입력받은 값을 가지고 같은 해쉬 함수에 넣어서 이와 동일한 값이 DB에 있는지 확인하여 검증한다.
- 그러나 이러한 방식은 입력가능한 모든 문자열을 조합한 `Rainbow table`을 사용해 값을 하나하나 대조해가며 암호화한 값을 알아내는 것이 가능한 치명적 단점이 존재한다.
- 즉 동일한 입력이 동일한 해시 알고리즘을 통해 입력될때 항상 동일한 출력을 한다는 해시함수의 동작방식을 이용하면 해킹이 가능해진다.

## 요즘의 해시 함수 동작방식

- 위와 같은 문제가 발생함에 따라서 요즘의 시 함수들은 salt를 사용한다.
- salt는 랜덤으로 만들어낸 값으로 이를 암호화할 값에 함께 붙여서 저장한다.
- 이렇게 되면 동일한 값을 암호화 했더라도 해시된 결과가 다르게되어 암호화 이전 값을 예측할 수 없게 된다.
- salt를 사용하지 않으면 `1234`와 `1234`는 암호화 했을때 `2Fdsdf`와 같이 동일한 값이 된다.
- 그러나 salt를 사용하면 `2Fd4fesdfe`, `2Fd5dks93` 처럼 동일한 텍스트임에도 완전히 다른 값이 되어 저장된다.

## 스프링 시큐리티 salt 저장 위치

- 스프링 시큐리티에서 salt는 암호화할 값과 함께 저장된다.
- 마치 다음과 같다. `$2a$10$aI4wQMnP1lCF.OW1/zo1G.q1lRps.8sDJcQRuKDMXe2yDG4HIOURx`
- 여기서 `aI4wQMnP1lCF.OW1/zo1G.q1lRps.8sDJcQRuKDMXe2yDG4HIOURx` 이 부분이 바로 암호화된 값과 salt값이다.
- 결론적으로 salt는 암호화된 값과 함께 DB에 저장되어, 검증할 때 사용된다.
