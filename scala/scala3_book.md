# Scala3 book

> 대체로 표현이 생략되었다. 중괄호, 소괄호 등 이런 구문을 생략하고 사용가능하게 변경되었다.

- [Scala3 book](#scala3-book)
  - [문자열 보간](#문자열-보간)
  - [if문](#if문)
  - [for문](#for문)
    - [guards](#guards)
    - [yield](#yield)
    - [표현법](#표현법)
  - [패턴 매칭](#패턴-매칭)
  - [while](#while)
  - [extends](#extends)
  - [클래스](#클래스)
  - [enum](#enum)
  - [extension](#extension)
  - [싱글톤 객체로 모듈만들기](#싱글톤-객체로-모듈만들기)
  - [컬렉션](#컬렉션)
    - [컬렉션 다양하게 다루기](#컬렉션-다양하게-다루기)
  - [동시성](#동시성)
    - [for문에서 동시성](#for문에서-동시성)
  - [예외](#예외)
  - [Eta Expansion](#eta-expansion)

## 문자열 보간

- `s`키워드와 `$` 키워드를 사용하면 문자열을 쉽게 표현할 수 있다.
- 표현식을 포함하려면 `${}`를 사용하면된다.

```scala
val firstName = "John"
val mi = 'C'
val lastName = "Doe"
println(s"Name: $firstName $mi $lastName")  // "Name: John C Doe"
println(s"${2 + 2}")
```

## if문

- scala2에서의 if-else는 여타 다른언어와 유사했다.
- 3에서는 파이썬과 같이 들여쓰기 + 오래된 함수형 언어 스타일로 전환되었다.
- 들여쓰기와 `then`으로 중괄호와 소괄호를 대체했다.
- 이는 표현식으로 쓸때에도 동일하다.

```scala
if x < 0 then
  println("negative")
else if x == 0 then
  println("zero")
else
  println("positive")

val x = if a < b then a else b
```

## for문

- 괄호를 생략하고 `do`키워드를 사용하도록 바뀌었다.
- `do`뒤에는 본문이 온다.

```scala
val ints = List(1, 2, 3, 4, 5)

for i <- ints do println(i)
```

### guards

- for문에 if식을 포함하는 guards는 아래와 같이 사용하도록 변경되었다.

```scala
val ints = List(1, 2, 3, 4, 5)

for
  i <- 1 to 3
  j <- 'a' to 'c'
  if i == 2
  if j == 'b'
do
  println(s"i = $i, j = $j")
```

### yield

- `yield`를 사용하면 새로운 리스트를 만든다.

```scala
val doubles = for i <- ints yield i * 2
//2, 4, 6, 8, 10

val names = List("chris", "ed", "maurice")
val capNames = for name <- names yield name.capitalize
//CHRIS, ED, MAURICE
```

### 표현법

- 물론 스코프를 나누는 표현법을 사용할 수 있다.

```scala
val doubles = for i <- ints yield i * 2
val doubles = for (i <- ints) yield i * 2
val doubles = for (i <- ints) yield (i * 2)
val doubles = for { i <- ints } yield (i * 2)
```

## 패턴 매칭

- 패턴매칭시에 `Any` 타입을 사용하지 않고 대신 `Matchable`를 사용하여 모든 타입에 대해 일치시킬 수 있다.

```scala
def getClassAsString(x: Matchable): String = x match
  case s: String => s"'$s' is a String"
  case i: Int => "Int"
  case d: Double => "Double"
  case l: List[?] => "List"
  case _ => "Unknown"
```

## while

- 중괄호가 생략되고 `do`키워드가 추가되었다.

```scala
while x >= 0 do x = f(x)

var x = 1

while
  x < 3
do
  println(x)
  x += 1
```

## extends

- 기존에서 다중 상속을 받을 경우 첫번째 상속에서는 `extends`를 쓰고, 그 뒤에는 `with`키워드를 사용했다.
- 그러나 scala3에서는 `extends`키워드 하나로 모든것을 처리한다.
- 이는 코틀린과 굉장히 유사하다.

```scala
class Cat(name: String) extends Speaker, TailWagger, Runner:
  def speak(): String = "Meow"
  override def startRunning(): Unit = println("Yeah ... I don’t run")
  override def stopRunning(): Unit = println("No need to stop")
```

## 클래스

- 클래스는 중괄호를 생략하고 `:`과 들여쓰기를 사용할 수 있다.
- 또한 생성시 `new`키워드를 사용하지 않고 생성할 수 있게 변경되었다.

```scala
class Person(var firstName: String, var lastName: String):
  def printFullName() = println(s"$firstName $lastName")
val p = Person("John", "Stephens")
```

## enum

- enum이 추가되었다.
- 기존의 `sealed abstract class + object`방식의 문법에서 `enum`하나로 변경되었다.
- 그리고 이러한 enum은 if를 내장하고 있다. 따라서 비교시 if를 사용하지 않는다.

```scala
enum CrustSize:
  case Small, Medium, Large

import CrustSize.*
val currentCrustSize = Small

// enums in a `match` expression
currentCrustSize match
  case Small => println("Small crust size")
  case Medium => println("Medium crust size")
  case Large => println("Large crust size")

// enums in an `if` statement
if currentCrustSize == Small then println("Small crust size")
```

## extension

- scala3에서 새롭게 추가된 키워드이다.
- 확장함수를 편리하게 사용할 수 있게 한것이다.

```scala
extension (s: String)
  def makeInt(radix: Int): Int = Integer.parseInt(s, radix)

"1".makeInt(2)
"10".makeInt(2)
"100".makeInt(2)

case class Circle(x: Double, y: Double, radius: Double)
extension (c: Circle)
  def circumference: Double = c.radius * math.Pi * 2
  def diameter: Double = c.radius * 2
  def area: Double = math.Pi * c.radius * c.radius

val aCircle = Circle(2, 3, 5)
aCircle.circumference
aCircle.diameter
```

## 싱글톤 객체로 모듈만들기

- 콘크리트 모듈을 만들어보는 예제는 아래와 같다.
- 싱글톤으로 만들어서 재사용성이 뛰어나다.

```scala
trait AddService:
  def add(a: Int, b: Int) = a + b

trait MultiplyService:
  def multiply(a: Int, b: Int) = a * b

object MathService extends AddService, MultiplyService

import MathService.*
println(add(1,1))
println(multiply(2,2))
```

## 컬렉션

### 컬렉션 다양하게 다루기

```scala
val b = (1 to 5).toList  //1, 2, 3, 4, 5
val c = (1 to 10 by 2).toList  //1, 3, 5, 7, 9
val e = (1 until 5).toList //1, 2, 3, 4
val f = List.range(1, 5) //1, 2, 3, 4
val g = List.range(1, 10, 3) //1, 4, 7

val a = List(10, 20, 30, 40, 10)
a.drop(2)  // List(30, 40, 10), 앞에 2개를 버림
a.filter(_ < 25) // List(10, 20, 10)
a.slice(2,4)  // List(30, 40), 2번 4-1번 인덱스만 가져옴
a.tail  // List(20, 30, 40, 10)
a.take(3)   // List(10, 20, 30)

// flatten
val a = List(List(1,2), List(3,4))
a.flatten // List(1, 2, 3, 4)

// map, flatMap
val nums = List("one", "two")
nums.map(_.toUpperCase)   // List("ONE", "TWO")
nums.flatMap(_.toUpperCase) //List('O', 'N', 'E', 'T', 'W', 'O')
```

## 동시성

- `Future`을 사용하여 동시성을 제어할 수 있다.
- `Success`와 `Failure` 두 가지 상태를 가진다. 이는 여타 동시성을 지원하는 다른 언어들과 동일하다.

```scala
def longRunningAlgorithm() =
  Thread.sleep(10_000)
  42
```

- 위와 같이 10초간 sleep하는 함수를 실행하고 성공/실패에 대해서 아래와 같이 컨트롤 가능하다.
- `onComplete`를 사용해서 성공 실패에 대한 스코프를 열수가 있다.

```scala
Future(longRunningAlgorithm()).onComplete {
  case Success(value) => println(s"Got the callback, value = $value")
  case Failure(e) => e.printStackTrace
}
```

- 이외에 콜백메서드는 `andThen`, `foreach`가 있다.

### for문에서 동시성

- for문에서 동시성을 사용하는 경우는 두가지가 있다.
- 첫째는 for문 밖에서 동시성 표현식을 만들고 사용하는 것과
- 두번째는 for문 안에서 동시성 표현식을 만들고 사용하는 것이다.
- 첫째의 경우는 병렬로 실행이 잘 된다.
- 그러나 둘째의 경우에는 순차적으로 실행되므로 동시성을 사용하는 의미가 무색해진다.

```scala
val f1 = Future { sleep(800); 1 }
val f2 = Future { sleep(200); 2 }
val f3 = Future { sleep(400); 3 }

//병렬로 실행된다.
val result =
    for
      r1 <- f1
      r2 <- f2
      r3 <- f3
    yield
      println(s"in the 'yield': ${delta()}")
      (r1 + r2 + r3)


result.onComplete {
    case Success(x) =>
      println(s"in the Success case: ${delta()}")
      println(s"result = $x")
    case Failure(e) =>
      e.printStackTrace
}

//이 경우는 순차적으로 실행된다.
for
  r1 <- Future { sleep(800); 1 }
  r2 <- Future { sleep(200); 2 }
  r3 <- Future { sleep(400); 3 }
yield
  r1 + r2 + r3
```

## 예외

- 스칼라의 standard libray와 third party library에서는 exception을 throw하기보다 `Option`을 사용해서 성공시에는 `Some`을 실패할때에는 `None`을 리턴할 것이다.
- 함수를 작성할때 이러한 방식으로 예외를 핸들링하는 것이 권장된다.

```scala
def makeInt(s: String): Option[Int] =
  try
    Some(Integer.parseInt(s.trim))
  catch
    case e: Exception => None
```

- 이 경우 이러한 메서드를 사용할 때에는 어떻게 처리해야할까?
- 이럴때에는 패턴 매칭을 사용하는 것이다.

```scala
makeInt(x) match
  case Some(i) => println(i)
  case None => println("That didn’t work.")
```

## Eta Expansion

- 고차함수같이 함수를 파라미터로 받는 함수에 메서드를 넣으면 어떻게 될까?
- scala3이전에는 에러가 발생했다. 그러나 scala3에서는 에러가 발생하지 않는다.
- 이는 eta expansion이라는 기술 때문이다. 이 기술은 메서드 타입을 함수 타입으로 변환시켜준다. 조용히 보이지 않게 바꾸어주는 이 기술덕분에 고차함수에 메서드 또한 삽입이 가능하다.

```scala
def isEvenMethod(i: Int) = i % 2 == 0  // a method
val isEvenFunction = (i: Int) => i % 2 == 0 // a function
val functions = List(isEvenFunction) // works
val methods = List(isEvenMethod) // works
```
