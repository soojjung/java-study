## 2. String클래스

- 자바에서는 문자열을 위한 클래스를 제공하는데, 바로 String클래스가 문자열을 저장하고 이를 다루는데 필요한 메서드를 함께 제공한다.

### 변경 불가능한(immutable) 클래스

- 문자열을 저장하기 위해서 문자형 배열 참조변수(`char[]`) value를 인스턴스 변수로 선언해놓고, 인스턴스 생성 시 생성자의 매개변수로 입력받는 문자열은 이 인스턴스변수(value)에 **문자형 배열(char[])로 저장**된다.
- 사실 JDK9부터 메모리 절약을 위해 `char[]`이 `byte[]`로 바뀜 (**컴팩트 문자열**)
- 한번 생성된 String인스턴스의 문자열은 **읽어 올 수만 있고, 변경할 수는 없다**.
- `‘+’`연산자를 이용해서 문자열을 결합하는 경우, 인스턴스 내의 문자열이 바뀌는 것이 아니라 새로운 문자열이 담긴 String인스턴스가 생성된다.

### 문자열의 비교

문자열을 만드는 두가지 방법: **문자열 리터럴**을 지정하는 방법과 **String클래스의 생성자**로 만드는 방법이 있다. 

```java
String str1 = "abc";
String str2 = "abc";
String str3 = new String("abc");
String str4 = new String("abc");
```

- 생성자로 생성하면 `new연산자`에 의해서 메모리 할당이 이루어지기 때문에 항상 새로운 String인스턴스가 생긴다. 반면에 문자열 리터럴은 이미 존재하는 것을 재사용하는 것이기 때문에 불변(immutable)이라 여러 곳에서 공유해도 아무런 문제가 없다.
- 문자열 리터럴은 클래스가 메모리에 로드될 때 자동적으로 미리 생성된다.
- 위 코드가 실행되었을 때 상황은 다음과 같다.
    
    ```java
    str1 == str2 // true
    str3 == str4 // false
    
    str1.equals(str2) // true
    str3.equals(str4) // true
    ```
    
 - `equals()`를 사용했을 때는 두 문자열의 내용을 비교하고, `==`는 주소값을 비교한다!

### 문자열 리터럴

- 자바의 모든 **문자열 리터럴은 컴파일 시에 클래스 파일에 저장**되는데, 이 때 같은 내용의 문자열 리터럴은 한번만 저장된다.
- 문자열 리터럴도 String인스턴스이고, 한번 생성하면 내용을 변경할 수 없으니 하나의 인스턴스를 공유해도 되기 때문이다.

### 빈 문자열(empty string)

- 자바에서는 길이가 0인 배열 존재
- 이 배열을 내부적으로 가지고 있는 문자열이 “빈 문자열”
- `String s = “”;` 에서 s가 참조하고 있는 String인스턴스는 내부에 `new char[0]`과 같이 길이가 0인 char배열을 저장하고 있다.
- But, char형 변수에는 반드시 하나의 문자를 지정해야 한다. `char c = '';` 불가능

    ```java
    String s = "";  // 빈 문자열로 초기화
    char c = ' ';   // 공백으로 초기화
    ```

### trim()과 strip()

- `trim()`은 유니코드 이전에 주로 사용하던 아스키(ASCII)에서 정의한 공백인 공백 문자(’ ‘)와 탭 문자(’/t’), 개행 문자(’/n’) 등만 제거
- `strip()`은 유니코드에서 추가적으로 정의된 공백도 제거
- `stripLeading()`: 왼쪽 공백만 제거
- `stripTrailling()`: 오른쪽 공백만 제거

### 문자 인코딩 변환

- 자바는 처음부터 기본 인코딩으로 UTF-16을 사용해왔는데, JDK18부터 **UTF-8**로 바뀌었다.

    ```java
    byte[] cp949_str = "가".getBytes("CP949"); // 인코딩을 CP949로 변환
    String str = new String(cp949, "CP949");  // byte배열을 문자열로 변환
    ```

### 기본형을 String으로 변환

- 숫자를 문자열로 변경하는 가장 간단한 방법은 숫자에 빈 문자열`””`을 더하는 것이다.
- 이 외에도 `valueOf()` 를 사용하는 방법도 있다.
- 성능은 `valueOf()` 가 더 좋지만, 빈 문자열을 더하는 방법이 간단하고 편하기 때문에 성능향상이 필요한 경우에만 `valueOf()` 를 쓴다.

### String을 기본형으로 변환

- `valueOf()`를 쓰거나 `parseInt()`를 쓰면 된다.

    ```java
    int i = Integer.parseInt("100"); // "100"을 100으로 변환하는 방법1
    int i2 = Integer.valueOf("100"); // "100"을 100으로 변환하는 방법2
    ```

- 원래 `valueOf()` 의 반환 타입은 int가 아니라 Integer인데, 오토박싱에 의해 Integer가 int로 자동 변환된다.

## 3. StringBuffer와 StringBuilder

- StringBuffer: 동기화o
- StringBuilder: 동기화x

- String: 불변(immutable)
- StringBuffer: 가변(mutable)

## 4. Math클래스

## 5. 래퍼(wrapper) 클래스

객체지향 개념에서 모든 것은 객체이어야 한다. 그러나 자바에서는 8개의 기본형을 객체로 다루지 않는데, 이것때문에 자바가 완전한 객체지향 언어가 아니라는 얘기를 듣기도 한다. 하지만 대신 보다 높은 성능을 얻을 수 있다. 

때로는 기본형(primitive type) 값도 어쩔 수 없이 객체로 다뤄야 하는 경우가 있는데, 이 때 사용되는 것이 바로 **래퍼(wrapper) 클래스**이며, 8개의 기본형을 대신할 수 있는 8개의 래퍼 클래스가 있다.

**✅ 자바의 기본형과 래퍼 클래스 정리표**

| **래퍼 클래스** | **주요 팩토리 메서드** | **활용 예시** |
| --- | --- | --- |
| Boolean | Boolean.valueOf(boolean value) | boolean b = Boolean.valueOf("true"); |
| **Character** | Character.valueOf(char c) | Character c = Character.valueOf('A'); |
| Byte | Byte.valueOf(byte value) | Byte b = Byte.valueOf("10"); |
| Short | Short.valueOf(short value) | Short s = Short.valueOf("20"); |
| **Integer** | Integer.valueOf(int value) | int n = Integer.valueOf("123"); |
| Long | Long.valueOf(long value) | Long l = Long.valueOf("100000"); |
| Float | Float.valueOf(float value) | float f = Float.valueOf("3.14"); |
| Double | Double.valueOf(double value) | Double d = Double.valueOf("2.718"); |

**📌 팁: 오토박싱 & 언박싱도 지원됨**

자바는 JDK 1.5 이후부터 **오토박싱/언박싱** 기능을 지원해서 `기본형 ↔ 래퍼 클래스`를 자동으로 변환해준다.

```java
Integer i = 10;         // 오토박싱 (int → Integer)
int j = i + 5;          // 언박싱 (Integer → int)
```

### 팩토리 메서드

객체를 생성해서 반환하는 메서드를 “팩토리 메서드”라고 하며, 객체를 생성하지 않고 호출할 수 있어야 하므로 static메서드이다. 그래서 "정적 팩토리 메서드 (static factory method)"라고도 한다. 

```java
public static Integer valueOf(int i) {
		return new Integer(i);    // 객체를 생성해서 반환
}
```
