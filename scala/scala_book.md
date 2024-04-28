# 스칼라북

- [스칼라북](#스칼라북)
  - [1. basic](#1-basic)
    - [블록](#블록)
    - [함수](#함수)
    - [메서드](#메서드)
    - [클래스](#클래스)
    - [case class](#case-class)
    - [object](#object)
    - [trait](#trait)
    - [메인 메서드](#메인-메서드)
  - [2. 타입](#2-타입)
    - [계층구조](#계층구조)
    - [캐스팅](#캐스팅)
  - [3. 클래스](#3-클래스)
    - [생성자](#생성자)
    - [private 멤버, getter/setter](#private-멤버-gettersetter)
  - [4. trait](#4-trait)
    - [trait 사용](#trait-사용)
  - [5. 튜플](#5-튜플)
    - [각 요소에 접근](#각-요소에-접근)
    - [패턴 매칭](#패턴-매칭)
    - [튜플과 case class](#튜플과-case-class)
  - [6. 믹스인 클래스 컴포지션](#6-믹스인-클래스-컴포지션)
  - [7. 고차함수](#7-고차함수)
  - [8. 중첩함수](#8-중첩함수)
  - [9. 커링](#9-커링)
  - [10. 케이스 클래스 -\> simply class](#10-케이스-클래스---simply-class)
  - [11. 패턴매칭](#11-패턴매칭)
  - [12. 싱글턴 객체](#12-싱글턴-객체)
    - [동반자(Companions)](#동반자companions)
    - [자바 개발자가 주의할 것](#자바-개발자가-주의할-것)
  - [13. 제네릭 클래스](#13-제네릭-클래스)
    - [가변성 - 순가변/역가변](#가변성---순가변역가변)
    - [상위 타입 경계](#상위-타입-경계)
    - [하위 타입 경계](#하위-타입-경계)
    - [제네릭 총 예제](#제네릭-총-예제)
  - [14. 내부클래스](#14-내부클래스)
  - [15. 추상타입](#15-추상타입)
  - [16. 합성타입](#16-합성타입)
  - [17. 명시적으로 타입이 지정된 자가참조](#17-명시적으로-타입이-지정된-자가참조)

## 1. basic

### 블록

- {} 블록 안 마지막 표현식의 결과는 블록 전체의 결과이다.
- 이는 여타 다른 함수형 언어의 표현법과 동일하다.

### 함수

- 함수와 메서드는 다르다. 함수형 언어에서는 함수와 메서드의 경계가 명확하다.
- 함수는 이름이 있는 함수와 익명함수를 만들 수 있다.

```scala
val name = (파라미터 존재가능) => 함수 구현 및 리턴 선언부
```

### 메서드

- 메서드는 `def`를 사용한다.
- 매개변수가 없을경우 `()`를 생략할 수 있다.
- 또한 여러 매개변수 목록을 가질 수 있다.

```scala
def 이름(매개변수 존재가능): 리턴타입 = 함수 구현 및 리턴 선언부
def 이름: 리턴타입 = 함수 구현 및 리턴 선언부
def 이름(매개변수들)(매개변수들): 리턴타입 = 함수 구현 및 리턴 선언부
def 이름(): 리턴타입 = {
  여러줄 표현식
}
```

### 클래스

- class 키워드로 정의하고 이름과 생성자 매개변수로 이루어진다.
- new 키워드로 생성한다.

```scala
class 이름(매개변수) {}
```

### case class

- case class는 변하지 않으며 값으로 비교한다
- 또한 new 클래스 없이 생성한다.
- 값으로 비교한다는 의미는 값처럼 사용가능하다는 것이다.
- `if (Point(2, 3) != Point(1, 2)` 는 true이다

### object

- 객체는 클래스의 싱글턴이다.
- 메서드와 멤버 변수 모두 선언 가능하다.

```scala
object 이름 {
  멤버 변수
  메서드
}
```

### trait

- 트레이트는 특정 필드와 메소드를 가지는 타입이고 다양한 트레이트와 결합할 수 있다.
- 인터페이스처럼 함수를 선언하기도 하며, 기본 구현도 할 수 있다.
- extends 키워드로 트레이트를 상속할 수 있고 override 키워드로 구현을 오버라이드할 수 있다.
- 클래스는 트레이트의 다중상속도 가능하다

```scala
trait 이름 { 구현 }
```

### 메인 메서드

- 메인 메서드는 싱글턴인 object로 선언하며 main메서드를 이용해 정의한다.
- jvm 언어 답게 jvm언어들이 프로그램을 실행하는 방법과 유사하다.

```scala
object Main {
  def main(args: Array[String]): Unit =
    println("Hello, Scala developer!")
}
```

## 2. 타입

### 계층구조

- `Any`: 모든 타입의 상위타입이다.
  - `AnyVal`: 모든 값타입의 상위타입이다.
    - `Double`, `Float`, `Long`, `Int`, `Short`, `Byte`, `Char`, `Unit`, `Boolean`: null을 가질 수 없다.
  - `AnyRef`: 모든 참조타입의 상위타입이다.
    - 값타입 이외의 모든 타입, `Null(null)`을 가진다.
  - `Nothing`: 모든 타입의 하위타입이다. 예외는 기본적으로 이 타입을 포함한다.
  - `None` : 아무것도 없는 값을 표현한다.
  - `Unit`: 빈값, 자바의 void와 유사하다.
  - `Nil` : 아무것도 없는 list를 표현한다.

### 캐스팅

- `byte` -> `short` -> `int` -> `long` -> `float` -> `double`
- `char` -> `int`
- 위와 같은 형태로 단방향 캐스팅이 가능하다.

## 3. 클래스

### 생성자

- 생성자는 기본값을 가질 수 있다.
- 생성시 `new 클래스명`으로 선언하게되면 정의한 기본값 대로 클래스가 생성된다.
- 또한 명시적 지정을 혀용하기에, 생성시 명시적으로 멤버 변수명을 지정하여 생성할 수 있다.
- `new 클래스명(멤버변수명=값)`

### private 멤버, getter/setter

- 멤버는 기본적으로 `public`이다.
- var는 mutable, val은 immutable이다.
- 기본생성자에서 var/val이 지정되었다면 `public`멤버이다.
- 지정되지 않았다면 `private`이기에 클래스 내에서만 참조 가능하다.
- private으로 숨기고, 커스텀 getter/setter를 사용하길 원한다면 아래와 같이한다.

```scala
class Point {
  private var _x = 0
  private var _y = 0
  private val bound = 100

  //getter
  def x = _x
  //setter
  def x_= (newValue: Int): Unit = {
    if (newValue < bound) _x = newValue else printWarning
  }

  def y = _y
  def y_= (newValue: Int): Unit = {
    if (newValue < bound) _y = newValue else printWarning
  }

  private def printWarning = println("WARNING: Out of bounds")
}

val point1 = new Point
point1.x = 99
point1.y = 101 // 경고가 출력됩니다
```

## 4. trait

> interface와 유사하다. 제네릭 타입과 추상메서드로 유용하다.

### trait 사용

- trait을 사용하려면 `extends` 키워드로 확장하고, `override`로 추상 메서드를 구현하라.

```scala
//제네릭 타입 선언, 두개의 추상 메서드
trait Iterator[A] {
  def hasNext: Boolean
  def next(): A
}

//current가 to가 될때까지 iterating하는 클래스
class IntIterator(to: Int) extends Iterator[Int] {
  private var current = 0
  override def hasNext: Boolean = current < to
  override def next(): Int = {
    if (hasNext) {
      val t = current
      current += 1
      t
    } else 0
  }
}

val iterator = new IntIterator(10)  //10까지 iterating함
iterator.next()  // returns 0
iterator.next()  // returns 1
```

## 5. 튜플

- 각 요소가 고유한 타입을 가지는 값
- 여러값을 편리하게 리턴한다.
- 튜플은 불변이다.
- 튜플은 `Tuple요소갯수[타입들]`로 추론된다.

```scala
val ingredient = ("Sugar" , 25)
// Tuple2[String, Int], 약칭 (String, Int)
```

### 각 요소에 접근

- 튜플에서 각요소는 `_순서`형태를 갖는다. 순서는 인덱스와 달리 1부터 시작한다.

```scala
val ingredient = ("Sugar" , 25)
ingredient._1 //Suger
ingredient._2 //25
```

### 패턴 매칭

- 패턴매칭으로 분리하여 사용할 수 있다.

```scala
val (name, quantity) = ingredient
//name과 quantity는 각각 String과 Int 타입으로 추론된다.

val numPairs = List((2, 5), (3, -7), (20, 56))
for ((a, b) <- numPairs) {
  println(a * b)
}
```

### 튜플과 case class

- 둘중 무엇을 써야할지 고민할 수 있다.
- 이름있는 요소를 원한다면 case class를, 아니라면 튜플이다.

## 6. 믹스인 클래스 컴포지션

- 스칼라는 새로운 클래스를 정의할 때 클래스의 새로운 멤버 정의 (즉, 슈퍼클래스와 비교할 때 변경된 부분)를 재사용할 수 있다.

```scala
//iterator를 추상화함
abstract class AbsIterator {
  type T
  def hasNext: Boolean
  def next(): T
}
```

- 이터레이터가 반환하는 모든 항목에 주어진 함수를 적용해주는 foreach 메소드로 AbsIterator를 확장한 믹스인 클래스는 아래와 같다.

```scala
trait RichIterator extends AbsIterator {
  def foreach(f: T => Unit): Unit = { while (hasNext) f(next()) }
}
```

- 아래는 문자열의 캐릭터를 차례로 반환하는 클래스이다.

```scala
class StringIterator(s: String) extends AbsIterator {
  type T = Char
  private var i = 0
  def hasNext = i < s.length()
  def next() = { val ch = s charAt i; i += 1; ch }
}
```

- 이제 StringIterator와 RichIterator를 합치고 싶다면 어떻게 해야할까?
- 스칼라는 이것을 지원한다.

```scala
object StringIteratorTest {
  def main(args: Array[String]): Unit = {
    class Iter extends StringIterator("Scala") with RichIterator
    val iter = new Iter
    iter foreach println
  }
}
```

## 7. 고차함수

- 다른 함수를 파라미터로 받거나, 함수를 리턴하는 함수이다.

```scala
def apply(f: Int => String, v: Int) = f(v)

class Decorator(left: String, right: String) {
  def layout[A](x: A) = left + x.toString() + right
}

object FunTest extends App {
  def apply(f: Int => String, v: Int) = f(v)
  val decorator = new Decorator("[", "]")
  println(apply(decorator.layout, 7))
}
//실행결과는 [7]
```

## 8. 중첩함수

- 중첩함수는 매개변수를 받기도 하며, 부모 함수의 매개변수를 사용할 수도 있다.
- 아래 함수는 `threshold`보다 작은 값을 xs 매개변수에서 찾아 리턴하는 함수이다.

```scala
object FilterTest extends App {
  def filter(xs: List[Int], threshold: Int) = {
    def process(ys: List[Int]): List[Int] =
      if (ys.isEmpty) ys
      else if (ys.head < threshold) ys.head :: process(ys.tail)
      else process(ys.tail)
    process(xs)  //process 함수 호출
  }
  println(filter(List(1, 9, 2, 8, 3, 7, 4), 5))
}
```

## 9. 커링

- 커링은 간단히 말해 매개변수가 2개이상인 함수에서 매개변수를 하나만 사용하여 호출하면,
- 그 한개가 기억이 되었다가, 나머지 인자를 넣어서 마저 호출을 하면
- 기존에 기억되었던 값을 이용해서 결과를 만들어내는 함수이다.

```scala
def add(x: Int)(y: Int): Int = x + y
add(2)
println(add(3)) //5
```

## 10. 케이스 클래스 -> simply class

- 기본적으로 불변이다. 필드에 `var`을 넣으면 가능하지만, 권장하지 않는다.
- 패턴매칭으로 분해가능하다. 래퍼런스가 아닌 동등성으로 비교한다.
- `new`로 초기화 하지 않고 간편하게 초기화한다.
- 생성자 파라미터는 `public`이다.
- 또한 case class는 `copy`메서드를 사용해서 원하는 값을 넣어 복사할 수 있다.

```scala
case class Email(sourceEmail: String, title: String, body: String)

val emailFromJohn = Email("john.doe@mail.com", "Greetings From John!", "Hello World!")
val editedEmail = emailFromJohn.copy(title = "I am learning Scala!", body = "It's so cool!")
```

- 패턴 매칭은 추상 클래스를 `extends`하고 이를 match-case 를 이용해서 분리할 수 있다.

```scala
abstract class Notification
case class Email(sourceEmail: String, title: String, body: String) extends Notification
case class SMS(sourceNumber: String, message: String) extends Notification
case class VoiceRecording(contactName: String, link: String) extends Notification

def showNotification(notification: Notification): String = {
  notification match {
    case Email(email, title, _) =>
      "You got an email from " + email + " with title: " + title
    case SMS(number, message) =>
      "You got an SMS from " + number + "! Message: " + message
    case VoiceRecording(name, link) =>
      "you received a Voice Recording from " + name + "! Click the link to hear it: " + link
  }
}
```

## 11. 패턴매칭

- 특정 타입, 모든 타입 등에 대해 패턴매칭이 가능하다.

```scala
x match {
  case 조건 => 리턴값
}

def matchTest(x: Any): Any = x match {
  case 1 => "one"
  case "two" => 2
  case y: Int => "scala.Int"
}
```

## 12. 싱글턴 객체

### 동반자(Companions)

- 대부분의 싱글턴 객체는 같은 이름의 클래스와 연관이 있다.
- 만약 동반자 객체를 정의하길 원한다면 같은 소스파일에 정의되어야한다.
- 타입클래스 패턴을 따를때, 일반적으로 타입클래스 인스턴스들을 동반자 안에 정의된 암시적 값들로 생각한다.

```scala
class IntPair(val x: Int, val y: Int)

object IntPair {
  import math.Ordering

  implicit def ipord: Ordering[IntPair] =
    Ordering.by(ip => (ip.x, ip.y))
}
```

### 자바 개발자가 주의할 것

- 코틀린의 companion object를 생각하면 편하다.
- static으로 정의하길 원한다면 아래 처럼 정의하라
- 또한 객체는 동반 클래스의 private 멤버에 접근이 가능하다.
- 또한 apply를 사용하면 new 키워드 없이 클래스를 생성할 수 있다.

```scala
class X(name: String) {
  import X._

  def blah = foo
}

object X {
  private def foo = 42
  def apply(name: String) = new X(name)
}

val x = X("first one")
```

## 13. 제네릭 클래스

- 다른 언어의 제네릭과 동일하다.

```scala
class Stack[T] {
  var elems: List[T] = Nil
  def push(x: T): Unit =
    elems = x :: elems
  def top: T = elems.head
  def pop(): Unit = { elems = elems.tail }
}

object GenericsTest extends App {
  val stack = new Stack[Int]
  stack.push(1)
  stack.push('a')
  println(stack.top)
  stack.pop()
  println(stack.top)
}
```

### 가변성 - 순가변/역가변

- 제네릭 클래스에서 살펴볼 수 있듯, 제네릭에서 타입 `T`를 정의하면 매개변수, 리턴타입에 `T`가 강제된다.
- 그러나 가변성을 이용하면 매개변수, 리턴타입 제약이 없도록 만들 수 있다. 이를 통해 재사용성을 극대화할 수 있다.
- 이때 `+`는 순가변 상태를 의미하며, `-`가 붙을 경우 역가변상태를 의미한다.
- 순가변과 역가변은 아래와 같다.
- 순가변 : b가 a의 하위 타입 => class[A] = class[B]
- 역가변 : b가 a의 하위타입 => class[B] = class[A]
- 결론적으로 순가변은 `상위타입이 하위타입을 참조하는것이 가능하다`
- 역가변은 `하위타입이 상위타입을 참조하는것이 가능하다`

```scala
// 순가변성
trait A
class B extends A

trait Container[+T] {
  def get: T
}

val catContainer: Container[B] = new Container[B] {
  def get: B = new B
}

val animalContainer: Container[A] = catContainer // 가능

// 역가변
trait Printer[-T] {
  def print(value: T): Unit
}

val animalPrinter: Printer[A] = new Printer[A] {
  def print(value: A): Unit = println(value)
}

val catPrinter: Printer[B] = animalPrinter // 가능
```

### 상위 타입 경계

- 제네릭을 정의할때 함수던 클래스던 정의하는 타입 `T`가 `S`보다 하위타입들어야한다.

```scala
class Container[T <: SomeClass] {
  // T는 SomeClass의 하위타입이어야 함
  //T < S
}
```

### 하위 타입 경계

- 제네릭을 정의할때 정의하는 타입 `T`가 `S`보다 상위타입들어어야한다.

```scala
class Container[T >: SomeClass] {
  // T는 SomeClass의 상위 타입이어야함
  //T > S
}
```

### 제네릭 총 예제

```scala
//순가변성을 가짐 -> 상위타입이 하위타입을 참조할 수 있음
class Stack[+T] {
  //하위타입의 경계 S > T -> T <: S 로도 표현가능
  def push[S >: T](elem: S): Stack[S] = new Stack[S] {
    override def top: S = elem
    override def pop: Stack[S] = Stack.this
    override def toString: String =
      elem.toString + " " + Stack.this.toString
  }
  def top: T = sys.error("no element on stack")
  def pop: Stack[T] = sys.error("no element on stack")
  override def toString: String = ""
}

object VariancesTest extends App {
  //Any는 모든 타입들의 상위 타입임
  var s: Stack[Any] = new Stack().push("hello")
  s = s.push(new Object())
  s = s.push(7)
  println(s)
}
```

## 14. 내부클래스

- 그래프 예제로 내부 클래스 동작을 살펴본다.
- 자바의 경우 내부클래스를 선언하고 같은 타입의 서로 다른 두 객체를 만든 후, 두 객체간 상호작용을 할때 별다른 문제가 없다.
- 그러나 스칼라는 문제를 일으킨다. 이 문제를 해결하기 위해서는 `#`연산자를 사용한다.

```scala
class Graph {
  class Node {
    var connectedNodes: List[Node] = Nil
    def connectTo(node: Node): Unit = {
      if (!connectedNodes.exists(node.equals)) {
        connectedNodes = node :: connectedNodes
      }
    }
  }
  var nodes: List[Node] = Nil
  def newNode: Node = {
    val res = new Node
    nodes = res :: nodes
    res
  }
}
```

- 위의 코드는 그래프 예제이다. 자바와 동일한 형태로 내부클래스를 선언하였다.
- 그리고 아래와 같이 점진적으로 그래프에 노드들을 추가해나갈 수 있다.

```scala
def graphTest: Unit = {
  val g = new Graph
  val n1 = g.newNode
  /*
  이 코드는 타입을 구체적으로 다음과 같이 명시할 수 있다.
  val n1: g.Node = g.newNode
  그냥 Node 타입이 아니라 어떤 그래프의 노드인지 표현이 가능하다.
  이것이 스칼라의 내부클래스 표현법이다.
  */
  val n2 = g.newNode
  val n3 = g.newNode
  n1.connectTo(n2)
  n3.connectTo(n1)
}
```

- 여기까지는 별다른 문제가 없다. 그러나 새로운 그래프를 만들고 서로 다른 그래프를 갖고 있는 노드끼리 연결시키면 문제가 발생한다.

```scala
object IllegalGraphTest extends App {
  val g: Graph = new Graph
  val n1: g.Node = g.newNode
  val n2: g.Node = g.newNode
  n1.connectTo(n2)      // legal
  val h: Graph = new Graph
  val n3: h.Node = h.newNode
  n1.connectTo(n3)      // illegal!
}
```

- 자바에서는 `n1.connectTo(n3)`이 문제 없이 동작한다. 그러나 스칼라는 아니다.
- 스칼라에서 내부클래스는 동일한 타입의 외부 클래스이더라도 다른 객체라면 같은 타입으로 보지 않는다.
- 따라서 아래와 같이 구현을 하게 되면 동일한 타입의 외부클래스이면서 다른 객체일때에도 소통이 가능해진다.
- 바로 타입선언시 `#`을 외부클래스와 내부클래스 사이에 두는 것이다.

```scala
class Graph {
  class Node {
    var connectedNodes: List[Graph#Node] = Nil
    def connectTo(node: Graph#Node): Unit = {
      if (!connectedNodes.exists(node.equals)) {
        connectedNodes = node :: connectedNodes
      }
    }
  }
  var nodes: List[Graph#Node] = Nil
  def newNode: Graph#Node = {
    val res = new Node
    nodes = res :: nodes
    res
  }
}
```

## 15. 추상타입

- 추상타입은 제네릭과 유사하며, 하위 객체에서 재정의해서 사용할 수 있다.
- 추상 타입 멤버를 포함한 트레잇이나 클래스는 종종 익명 클래스 인스턴스화와 함께 사용된다

```scala
trait Buffer {
  type T  //추상타입
  val element: T
}

abstract class SeqBuffer extends Buffer {
  type U  //추상타입
  type T <: Seq[U]  //T 재정의
  def length = element.length
}

abstract class IntSeqBuffer extends SeqBuffer {
  type U = Int  //U 재정의
}

//익명클래스 인스턴스화
def newIntSeqBuf(elem1: Int, elem2: Int): IntSeqBuffer =
  new IntSeqBuffer {
        type T = List[U]  //T 재정의
        val element = List(elem1, elem2)
      }
val buf = newIntSeqBuf(7, 8)
println("length = " + buf.length)
println("content = " + buf.element)
```

- 추상 타입 멤버를 클래스의 타입 파라미터로 하거나 클래스의 타입 파라미터로 추상 타입 멤버를사용할 수 있음에 주목하자.
- 아래는 타입 파라미터만을 사용한 예제이다.

```scala
abstract class Buffer[+T] {  //type: T를 타입 파라미터로 사용
  val element: T
}
abstract class SeqBuffer[U, +T <: Seq[U]] extends Buffer[T] {
  def length = element.length
}
def newIntSeqBuf(e1: Int, e2: Int): SeqBuffer[Int, Seq[Int]] =
  new SeqBuffer[Int, List[Int]] {
    val element = List(e1, e2)
  }
val buf = newIntSeqBuf(7, 8)
println("length = " + buf.length)
println("content = " + buf.element)
```

- `type: T`를 타입파라미터로 바꾸고, 가변성 어노테이션을 사용했다.
- 가변성 어노테이션을 사용하지 않으면 반환되는 객체의 특정 시퀀스 구현 타입을 감출 수 없게 된다.
- 또한 가변성 어노테이션을 사용할 수 없는 경우도 있으니, 두 구현 모두 숙지하자.

## 16. 합성타입

- 객체 타입을 여러 다른 타입의 하위타입으로 표현하려할 때 합성타입을 사용할 수 있다.
- 일례로 복제가 가능한 `Cloneable`타입이 있고, 리셋이 가능한 `Restable`타입이 있다고 가정할때, 타입을 복제하며 리셋하는 `cloneAndReset`이라는 함수를 만드려고 한다면?
- 만약 매개변수로 객체를 받을때 `Cloneable`을 사용하면 리셋이 불가능해지고, `Restable`를 사용한다면 복제가 불가능해진다.
- 따라서 스칼라는 `Cloneable with Resetable`과 같은 형태로 합성타입을 지원한다.

```scala
trait Cloneable extends java.lang.Cloneable {
  override def clone(): Cloneable = {
    super.clone().asInstanceOf[Cloneable]
  }
}
trait Resetable {
  def reset: Unit
}
def cloneAndReset(obj: Cloneable with Resetable): Cloneable = {
  val cloned = obj.clone()
  obj.reset
  cloned
}
```

## 17. 명시적으로 타입이 지정된 자가참조

- 확장가능한 개발을 할때, 일례로 추상클래스를 선언하고 구현할땐 this값의 타입을 명시적으로 선언하는 것이 편하다.
- 간단히 말해 `this`에 나의 타입이 아닌 다른 타입을 부여하는 것이다.
- 그래프 데이터구조를 예시로 하여 더욱 구체적으로 설명하겠다.
- 아래는 그래프 정의한 추상 클래스이다.
- 그리고 그 추상 클래스를 이용해서 부분적으로 구현한 클래스이다.

```scala
abstract class Graph {
  type Edge
  type Node <: NodeIntf
  abstract class NodeIntf {
    def connectWith(node: Node): Edge
  }
  def nodes: List[Node]
  def edges: List[Edge]
  def addNode: Node
}

abstract class DirectedGraph extends Graph {
  type Edge <: EdgeImpl
  class EdgeImpl(origin: Node, dest: Node) {
    def from = origin
    def to = dest
  }
  class NodeImpl extends NodeIntf {
    def connectWith(node: Node): Edge = {
      val edge = newEdge(this, node)
      edges = edge :: edges
      edge
    }
  }
  protected def newNode: Node
  protected def newEdge(from: Node, to: Node): Edge
  var nodes: List[Node] = Nil
  var edges: List[Edge] = Nil
  def addNode: Node = {
    val node = newNode
    nodes = node :: nodes
    node
  }
}
```

- 다른 부분보다 메소드 `connectWith`의 구현을 좀 더 자세히 살펴보면, 엣지를 생성하기 위해선 반드시 자기 참조 `this`를 팩토리 메소드 `newEdge`로 전달해야 함을 알 수 있다.
- 하지만 `this`에는 타입 `NodeImpl`이 할당되고, 이는 팩토리 메소드에서 요구하는 타입 `Node`와 호환되지 않는다.
- 왜냐하면 `newEdge`메서드는 `Node`타입의 매개변수가 필요하기 때문이다.
- 따라서 위의 `connectWith`메서드는 에러가 발생한다.
- 스칼라에선 자기 참조 `this`에 다른 타입을 명시적으로 부여함으로써 클래스를 다른 타입(향후에 구현될)과 묶을 수 있다.
- 아래는 `connectWith`메서드를 수정한 코드이다.

```scala
class NodeImpl extends NodeIntf {
    self: Node =>
    def connectWith(node: Node): Edge = {
      val edge = newEdge(this, node)  // now legal
      edges = edge :: edges
      edge
    }
  }
```

- `self: Node =>`라는 명시적 어노테이션이 포함되었다.
- 아래는 이 추상 그래프를 콘크리트화 한 후 사용하는 예제이다.

```scala
class ConcreteDirectedGraph extends DirectedGraph {
  type Edge = EdgeImpl
  type Node = NodeImpl
  protected def newNode: Node = new NodeImpl
  protected def newEdge(f: Node, t: Node): Edge =
    new EdgeImpl(f, t)
}

def graphTest: Unit = {
  val g: Graph = new ConcreteDirectedGraph
  val n1 = g.addNode
  val n2 = g.addNode
  val n3 = g.addNode
  n1.connectWith(n2)
  n2.connectWith(n3)
  n1.connectWith(n3)
}
```
