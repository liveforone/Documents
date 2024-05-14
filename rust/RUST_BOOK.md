# Rust Book

- [Rust Book](#rust-book)
  - [update \& uninstall command](#update--uninstall-command)
  - [cargo command](#cargo-command)
  - [basic](#basic)
    - [변수 \& 가변/불변](#변수--가변불변)
    - [상수](#상수)
    - [숫자타입](#숫자타입)
    - [튜플](#튜플)
    - [배열](#배열)
    - [함수](#함수)
  - [소유권](#소유권)
    - [복사](#복사)
    - [참조](#참조)
    - [변경 가능한 참조](#변경-가능한-참조)
    - [슬라이스](#슬라이스)
  - [구조체](#구조체)
    - [구조체 정의](#구조체-정의)
    - [구조 업데이트 구문](#구조-업데이트-구문)
    - [메서드](#메서드)
    - [self를 리턴하는 메서드](#self를-리턴하는-메서드)
  - [enum과 패턴매칭](#enum과-패턴매칭)
    - [enum basic](#enum-basic)
    - [null safe - Option](#null-safe---option)
    - [패턴매칭](#패턴매칭)
    - [if-let](#if-let)
  - [모듈](#모듈)
    - [모듈 선언](#모듈-선언)
    - [pub 키워드](#pub-키워드)
    - [use 키워드](#use-키워드)
    - [super 키워드](#super-키워드)
    - [\* 연산자](#-연산자)

## update & uninstall command

- `rustup update`
- `rustup self uninstall`

## cargo command

- `cargo new projectName`
- `cargo build`
- `cargo check` : build보다 가볍게 컴파일하여 오류를 체크함
- `cargo run`
- `cargo build --release` : 최적화하여 빌드(배포시)

## basic

### 변수 & 가변/불변

- 변수는 기본적으로 불변(immutable)이다.
- 가변 변수를 원하면 `mut` 키워드를 붙여야한다.
- `let x = 10` / `let mut x = 10`

### 상수

- `const`키워드로 정의하며 항상 불변상태를 가진다.
- 상수의 생명주기는 프로그램 종료까지이다.

### 숫자타입

- `i8` / `u8` / `i16`/ `u16` / `i32` / `u32` / `i64` / `u64` / `i128` / `isize` / `usize`
- isize와 usize는 컴퓨터의 아키텍처에 따라 사이즈가 달라진다.
- `f32` / `f64`

### 튜플

- `(값, 값, 값)`
- 튜플은 서로 다른 타입을 넣을 수 있다.
- 튜플은 인덱스로 접근하며 `변수명.인덱스`와 같은 형태로 접근한다.
- ex : `tup.0`

### 배열

- `[값, 값, 값]`
- 배열은 서로 같은 타입을 넣을 수 있다.
- 배열은 벡터에 비해 유연하지 않다. 고정 길이 형태를 가지기 때문이다.
- 배열은 인덱스로 접근한다. ex : `a[0]`
- 배열의 타입 선언은 `[타입; 갯수]`와 같은 형태로 선언한다.
- ex : `let list: [i32; 3] = [1, 2, 3]`

### 함수

- `fn`키워드로 선언한다.
- 함수는 snake case가 기본적인 컨벤션이다.
- 반환 값이 있는 경우 `-> 타입`으로 표현한다.
- `fn plus_one(x: i32) -> i32 {x + 1}`
- 여타 함수형 언어처럼 반환값이 있는 함수의 경우 함수 블록의 마지막 값이 리턴값이다.

## 소유권

### 복사

- 러스트는 `let s2 = s1`과 같은 대입을 하였을때, s1이 래퍼런스타입(일례로 String)이라면 힙 주소를 복사한다.
- 간단히 말해 래퍼런스타입을 대입하면 포인터 주소를 복사한다.
- 두 값은 같은 포인터 주소를 가르킨다.
- 이러한 이유로 복사가 일어나고 s1의 라이프타임이 끝나 메모리가 해제되면 복사한 s2도 문제가 발생할 수 있다.
- 이러한 이유로 메모리 안전을 위해 복사하였다면 복사한 값을 러스트는 더이상 유효하지 않다고 판단한다.

```rust
let s1 = String::from("hello");
let s2 = s1;
println!("{}, world", s1);
//무효화된 참조를 했기에 에러가 발생한다.
```

- 얕은 복사와는 개념이 조금다르다. 러스트에서는 그냥 s1에서 s2로 옮겨지는 것(move)이기 때문이다.
- 결론적으로 러스트는 래퍼런스 타입 값을 어떤 값에 대입하면 기존 값을 무효화 해버리고 포인터 참조를 새롭게 대입된 값에 할당한다.
- 진짜 복사를 원한다면 `clone()`함수를 사용해야한다.

### 참조

- 러스트의 복사 방식 때문에 매개변수로 래퍼런스 타입을 던지는 것이 불안할 수도 있다.
- 왜냐면 소유권이 계속 이동하는 복잡한 상황에 놓이기 때문이다.
- 따라서 c++의 래퍼런스처럼 `&`를 통해서 참조자를 던질 수 있다.
- 참조자를 사용하면 소유권을 가져오지 않고 참조할 수 있다.
- 당연하게도 소유권을 가져온 것이 아니기에 수정과 같은 일은 불가능하다

```rust
let s1 = String::from("hello");
let len = calculate_length(&s1);
fn calculate_length(s: &String) -> usize {
    s.len()
}
```

### 변경 가능한 참조

- 변경이 가능한 참조도 존재한다.
- `&mut` 키워드를 사용하면된다.
- 다만 한 값에 대한 변경가능한 참조를 갖는 것(할당)은 한번이다.

```rust
let mut s = String::from("hello");
let r1 = &mut s;  //한 번의 참조할당은 가능하다.
let r2 = &mut s;  //error, 불가능하다.
```

- 다만 아래처럼 새로운 스코프를 연다면 가능하다.

```rust
let mut s = String::from("hello");
{
    let r1 = &mut s;
}
let r2 = &mut s;
```

### 슬라이스

- 문자열을 자르는 슬라이스는 러스트에서 이미 구현이 되어있다
- 슬라이스 타입은 `&str`이다.
- `&변수명[0..4]`와 같이 사용한다.
- 당연하게도 0인덱스부터 3까지를 가져온다.
- 앞/뒤를 생략할 수 있다. ex : `&s[..2]` / `&s[3..]`

## 구조체

> 관계형 데이터를 표현하기 위한 타입

### 구조체 정의

- `struct` 키워드로 정의한다.
- 이를 사용할땐 `name { }`과 같이 사용한다.
- 구조체는 특정 필드만 변경가능하게 만들 수 없다. 모든 필드가 변경가능하다.

### 구조 업데이트 구문

- 타입이 같지만 다르게 생성된 구조체를 사용해서 새로운 구조체를 만들때에는 아래와 같이한다.
- `..`을 통해서 나머지 모든 부분을 가져온다.

```rust
let user2 = User {
    email : String::from("hello"),
    ..user1
}
```

### 메서드

- 메서드는 함수와 비슷하게 생겼지만 첫번째 인수로 `&self`를 갖는다.
- 구조체의 값을 변경하려면 `&mut self`를 인수로 받아야한다.
- 또한 `impl` 키워드로 구현부에서 메서드를 선언해주어야한다.

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}
```

### self를 리턴하는 메서드

- `Self`를 리턴하는 메서드가 있다.
- 팩토리 메서드처럼 자기 자신의 타입을 만들어서 리턴하는 것인데, 이러한 메서드는 호출시 `::` 연산자를 사용한다.

```rust
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}

Rectangle::square(3);
```

## enum과 패턴매칭

### enum basic

- 열거형은 `enum` 키워드를 사용하며 `::`를 통해 접근한다.
- enum은 아무것도 값이 없는 상수만 사용할 수도 있고,
- `(타입)`을 사용해서 튜플처럼 하나부터 원하는 수만큼 값을 넣을 수도 있다.
- 또한 `{}`를 사용해서 구조체처럼 구현할 수도 있다.

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

### null safe - Option

- rust는 Option타입이 존재한다.
- 이는 enum으로 이루어져 있는데, 아래와 같다.

```rust
enum Option<T> {
    None,
    Some(T),
}

let some_number: Option<i32> = None;
```

### 패턴매칭

- 패턴매칭은 `match 매개변수 { 조건 => 리턴밸류, }`의 형식을 따른다.
- 내부 구현이 필요할 경우 리턴밸류 부분에서 `{}`를 통해서 블럭을 열어도 된다.
- Option타입을 사용할때, 혹은 default 밸류가 필요할 때에는 `_`를 사용해서 default 밸류를 표현할 수 있다.
- 특히나 Option타입의 경우 `Some`과 `None` 모두에 대해 매칭을 해주지 않으면 컴파일 에러가 발생한다.

### if-let

- 단 하나의 패턴에 대해서만 매칭시키고 싶을때 `if-let`구문을 사용한다.
- 예시는 아래와 같다.

```rust
let config_max = Some(3u8);
if let Some(max) = config_max {
    println!("The maximum is configured to be {}", max);
}
```

## 모듈

- `mod`, `pub`, `use`, `crate` 키워드를 사용한다.
- crate 루트파일 경로는 `src/lib.rs`

### 모듈 선언

- 모듈은 보통 `src/모듈이름.rs` 혹은 `src/모듈이름/mod.rs` 위치에서 모듈을 찾는다.
- 하위 모듈이라면 `src/모듈이름/하위모듈.rs` 혹은 `src/모듈이름/하위모듈/mod.rs`위치에서 찾는다.

### pub 키워드

- 모듈안에 모듈을 정의할 수 있다. 그러나 private이 default로 적용된다.
- 따라서 `pub`키워드로 open해주면 된다. 그러나 모듈안에 모듈이 있고, 그 모듈에 함수나 어떤 값들이 있다면 그 값들도 `pub` 키워드로 public 접근을 허용하지 않으면 사용할 수 없다.

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

// 절대경로 사용
crate::front_of_house::hosting::add_to_waitlist();

// 상대경로 사용
front_of_house::hosting::add_to_waitlist();
```

### use 키워드

- use 키워드는 모듈을 범위 안(선언한 파일)으로 가져오는 키워드이다.
- 따라서 아래 코드는 컴파일 에러가 발생한다.
- 오류가 발생핳지 않으려면 custom 모듈 안에서 use 키워드로 hosting을 불러와야한다.

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting;

mod customer {
    pub fn eat_at_restaurant() {
        hosting::add_to_waitlist();
    }
}
```

### super 키워드

- 파일 전체에서 어떤 모듈을 사용하길 원한다면 `super::경로`를 사용해서 가져오면 된다.

### \* 연산자

- `*`연산자를 활용하면 경로의 모든 공개 항목을 가져온다.
- ex : `std::*;`
