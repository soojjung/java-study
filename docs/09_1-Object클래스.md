**java.lang 패키지란?**

java.lang 은 **자바 프로그램에서 가장 기본적인 클래스들을 모아 둔 표준 패키지**이다. 

JVM이 구동될 때 항상 로드되며, **모든 소스 파일에 자동으로 import** 된다. 

따라서 `String`, `System`, `Math` 같은 클래스를 따로 `import` 문 없이 바로 쓸 수 있다.

## 1. Object 클래스

### equals(Object obj)

매개변수로 객체의 참조 변수를 받아서 비교하여 그 결과를 `boolean`값으로 알려 주는 역할을 한다.

```java
public boolean equals(Object obj) {
  return (this == obj);
}
```

⇒ 두 객체의 같고 다름을 “**참조변수의 값(메모리 주소)**”으로 판단한다!

이렇게 Object클래스로부터 상속받은 equals메서드는 두 개의 참조변수가 같은지를 판단하는 기능밖에 할 수 없으니 **오버라이딩**을 통하여 주소가 아닌 객체에 저장된 내용을 비교할 수 있다.

대표적으로 String클래스는 오버라이딩을 통해서 String인스턴스가 갖는 문자열의 내용을 비교하도록 되어있다. 

그렇기 때문에 같은 내용이 문자열을 갖는 두 String인스턴스에 equals()를 사용하면 항상 `true`를 받는다. 

참고) `String`뿐만 아니라, `Date`, `File`, `wrapper클래스(Integer, Double 등)`의 `equals()`도 주소값이 아닌 내용을 비교하도록 오버라이딩되어 있다. 하지만 `StringBuffer`는 오버라이딩되어 있지 않다. 

**String의 equals() 재정의**

String 클래스는 내부적으로 이렇게 오버라이드 되어 있다:

```java
@Override
public boolean equals(Object anObject) {
    if (this == anObject) return true;
    if (anObject instanceof String) {
        String another = (String) anObject;
        if (this.length() != another.length()) return false;
        // 내부 char 배열을 한 글자씩 비교
        return Arrays.equals(this.value, another.value);
    }
    return false;
}
```

1. **참조 동일성** 먼저 체크
2. **instanceof String** 으로 타입 확인
3. **길이** 비교
4. **문자 내용**(char 배열) 비교

### hashCode()

해시 함수는 찾고자하는 값을 입력하면, 그 값이 저장된 위치를 알려주는 해시 코드(hash code)를 반환한다. 

`hashCode()` 또한 String클래스에서 오버라이딩되어 있기 때문에 문자열의 내용이 같으면 항상 동일한 해시 코드를 얻는다. 

반면에 `System.identityHashCode(Object x)`는 Object클래스의 `hashCode()`처럼 객체의 주소값으로 해시 코드를 생성하기 때문에 모든 객체에 대해 항상 다른 해시 코드를 반환할 것을 보장한다.

### toString()

`toString()`은 인스턴스의 “정보”를 문자열(String)로 제공한다. 한마디로, 인스턴스 변수에 저장된 값들을 문자열로 표현한다는 뜻이다. 

```java
public String toString() {
  return getClass().getName()+"@"+Integer.toHexString(hashCode());
}
```

: 클래스 이름에 16진수의 해시 코드를 얻는다!

String클래스와 Date클래스의 경우, 다른 결과가 출력되도록 오버라이딩 되었다.

- String클래스 - String인스턴스가 갖고 있는 문자열 반환
- Date클래스 - Date인스턴스가 갖고 있는 날짜와 시간을 문자열로 변환하여 반환

```java
String str = new String("KOREA");
java.util.Date today = new java.util.Date();

System.out.println(str.toString());   // KOREA
System.out.println(today.toString()); // Sun May 04 17:14:28 KST 2025

```

주의할 점이, 자바에서 메서드를 **오버라이드**할 때는 원칙적으로 “접근 제어자가 같거나 더 넓은 범위여야” 한다.

Object클래스에 정의된 `toString()`의 접근 제어자가 public이므로 자식 클래스의 `toString()`의 접근제어자도 public으로 해야한다. 

**리스코프 치환 원칙(Liskov Substitution Principle)**

예를 들어, 다음과 같은 메서드가 있다고 가정해 보자

```java
void printObject(Object o) {
    System.out.println(o.toString());
}
```

- 이 코드는 Object 타입의 `toString()`에 **언제나 접근**할 수 있다고 전제한다.
- 만약 서브클래스에서 `toString()`의 접근 범위를 protected나 private로 줄이면,
    - `printObject(new Card2())` 호출 시 컴파일러는 **`Object.toString()` 이 공개(public)이어서** 접근 가능하지만,
    - 실제 실행 시엔 Card2 의 `toString()` 을 호출하려다 **접근 불가 오류**가 발생할 수 있다.
- 이처럼 슈퍼클래스의 계약을 깨지 않으려면, 최소한 원본 메서드의 접근 범위(public)를 유지해야 한다.

### clone()

`clone()`은 자신을 복제하여 새로운 인스턴스를 생성하는 일을 한다. 

여기서 이뤄지는 복제는 단순히 인스턴스 변수의 값만 복사하기 때문에 참조 타입의 인스턴스 변수가 있는 클래스는 완전한 복제가 이루어지지 않는다. ⇒ **얕은 복사(shallow copy)** 발생!

`clone()` 사용 방법

1. 복제할 클래스가 `Cloneable`인터페이스를 구현해야 한다.
2. `clone()`을 오버라이딩하면서 접근 제어자를 protected에서 public으로 변경
3. `try-catch`내에서 조상클래스의 `clone()`을 호출

```java
class Point implements Cloneable { // 1. Cloneable인터페이스를 구현
    int x, y;

    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public String toString() {
        return "x=" + x + ", y=" + y;
    }

    public Object clone() {      // 2. 접근 제어자를 protected에서 public으로 변경
        Object obj = null;
        try {
            obj = super.clone(); // 3. try-catch내에서 조상클래스의 clone()을 호출
        } catch (CloneNotSupportedException e) {
        }
        return obj;
    }
}

class CloneEx {
    public static void main(String[] args) {
        Point original = new Point(3, 5);
        Point copy = (Point) original.clone();
        System.out.println(original);
        System.out.println(copy);
    }
}

```

### 공변 반환타입

‘공변 반환타입’을 사용하면, 조상의 타입이 아닌, 실제로 반환되는 자손 객체의 타입으로 반환할 수 있어서 번거로운 형변환이 줄어든다.

```java
public Point clone() { // 반환타입을 Object에서 Point로 변경
  Object obj = null;
  try {
    obj = super.clone();
  } catch(CloneNotSupportedException e) {}
  return (Point)obj;   // Point타입으로 형변환한다. 
}
```

### 얕은 복사와 깊은 복사

- **얕은 복사(shallow copy)**: 객체배열열을 복제할 때, 원본과 복제본이 같은 객체를 공유 ⇒ 원본을 변경하면 복사본도 영향을 받는다.
- **깊은 복사(deep copy)**: 원본이 참조하고 있는 객체까지 복제하는 것 ⇒ 서로 다른 객체를 참조하기 때문에 원본의 변경이 복사본에 영향을 미치지 않는다.

### getClass()

: 설계도 객체를 반환

자신이 속한 클래스의 Class객체를 반환하는 메서드인데, 클래스의 모든 정보를 담고 있고 클래스 당 1개만 존재한다. 그리고 클래스 파일이 ‘**클래스 로더**’에 의해서 메모리에 올라갈 때, 자동으로 생성된다. 

파일하고 객체의 차이?

: 객체는 메모리에 있는 변수. 클래스 파일을 읽어서 사용하기 편한 형태로 저장해 놓은 것인 Class 객체(설계도 객체)이다. 

참고) 클래스 파일`(*.class)`을 메모리에 로드하고 변환하는 일은 클래스 로더가 한다. 

`getClass()` ― `java.lang.Object`가 제공하는 런타임-타입 조회 메서드

```java
Animal a = new Dog();            // 정적 타입 Animal, 실제는 Dog
System.out.println(a.getClass());
// 출력: class com.example.Dog
```

- `a instanceof Animal` → true
- `a.getClass() == Animal.class` → false
- `a.getClass() == Dog.class` → true

**instanceof 와의 차이**

| **비교 항목** | obj instanceof Foo | obj.getClass() == Foo.class |
| --- | --- | --- |
| **상속 허용** | obj 가 *Foo* 이거나 **하위 클래스**이면 true | 정확히 *Foo* **그 클래스만** 일치해야 true |
| **NPE 가능성** | obj 가 null이면 항상 false | null 에서 호출하면 **NPE 발생** |
| **가독성** | “Foo 타입인가?” | “바로 Foo 그 자체인가?” |

```java
if (obj instanceof Number) …        // Integer·Double 모두 통과
if (obj.getClass() == Number.class) // 하위 타입이면 false
```

### Class객체를 얻는 방법

```java
Class cObj = new Card().getClass();  // 생성된 객체로 부터 얻는 방법
Class cObj = Card.class;             // 클래스 리터럴(*.class)로 부터 얻는 방법
Class cObj = Class.forName("Card");  // 클래스 이름으로 부터 얻는 방법
```

특히 `forName()`은 특정 클래스 파일, 예를 들어 데이터베이스 드라이버를 메모리에 올릴 때 주로 사용한다. 문자열(클래스의 이름)만 바꾸면 코드 변경없이 다른 클래스의 객체를 생성할 수 있어 꽤 유용하다.
