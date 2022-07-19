
# 정리

---

## 동일성(identity)와 동등성(equality)에 대해 설명해주세요. (equals(), ==)

동일성은 객체의 식별성을 말합니다. 즉, 객체자체의 주소값이 같은지, 객체가 동일한 객체인지를 판단하는 것입니다. 동등성은 논리적인 동치성입니다. 즉, 객체의 값이 같음을 비교하는 것입니다.

기본적으로 Object 클래스에 정의된 equals() 메소드는 동일성 비교를 하며, String 클래스와 같은 값 클래스에 정의된 equals()는 동등성 비교를 합니다.

따라서 개발자는 인스턴스의 논리적 동치성을 판단하여야 한다면, equals() 메소드를 오버라이딩 하여 동치성의 판단기준을 정의해 주어야 합니다.

추가로, equals() 메소드를 재정의하여 논리적 동치성을 구현 할때는 hashcode도 재정의 해야 합니다. HashMap이나 HashSet 클래스의 원소로 사용시, 논리적으로 같은 객체는 같은 hashcode를 반환하여야 하기 때문입니다.

<details>
<summary>참고 답변 https://github.com/ksundong/backend-interview-question</summary>
<div markdown="1">

동일성은 객체의 주소를 비교하는 것이고, 동등성은 객체의 값이 같음을 비교하는 것입니다.
기본적으로 자바에서는 Object 클래스에 정의된 equals() 메소드가 동일성 비교를 합니다. 
따라서, 개발자는 원한다면 equals() 메소드를 오버라이딩해서 동등성의 판단 기준을 정의해주면 됩니다.
</div>
</details>


```text

🤔 목차
1. 동일성 (identity)
2. 동등성 (equality)
3. 차이점,응용
4. 정리

```


# 동일성 (identity)

---

## 객체 식별성 (Object Identity)

두 **객체**가 물리적으로 같은가

### ex1) Object 클래스의 equals()

참조 변수 p1, p2는 아래의 Person 클래스의 객체를 가리킨다.

```java
class Person {
	long id;
	Person(long id) {
		this.id = id;
	}
}
```

```java
Person p1 = new Person(8011081111222L);
Person p2 = new Person(8011081111222L);

System.out.println(p1==p2);      
System.out.println(p1.equals(p2));      
System.out.println(p1.id==p2.id);
p2 = p1;
System.out.println(p1==p2);
System.out.println(p1.equals(p2));
```

결과

```java
false
false
true
true
true
```

Person 클래스는 모든 클래스의 조상인 Object 클래스를 상속 받는다.

(즉, 참조변수 p1,p2의 .equals() 는 Object 클래스의 equals() 메소드 이다.)

```java
// 실제 Object 클래스의 equals()

public boolean equals(Object obj) {
      return (this == obj);
}
```

Object 클래스의 equals() 메소드는 물리적으로 객체가 같은지 판단한다.

```java
Object obj1 = new Object();
Object obj2 = new Object();

System.out.println(obj1==obj2); // false
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/77c08761-fe8e-4f86-a8c8-29c094bcf91d/Untitled.png)

# 동등성 (equality)

---

## 논리적 동치성 (Logical Equality)

두 객체의 **값**이 같은가?

### **ex2) 값 클래스(`Integer`, `String`과 같은 값을 표현하는 클래스)의 `equals()` 오버라이딩**

클래스 Person은 id 라는 값을 표현하는 클래스 이다.

개발자는 두 값 객체를 `equals()` 로 비교할 때, 두 객체가 같은지 가 아니라, **두 객체가 가리키는 값**이 같은지를 알고 싶어한다.

따라서 `Object` 클래스의 `equals()` 메소드를 **오버라이딩** 하여 **논리적 동치성**을 확인하도록 재정의 한다.

```java
class Person {
	long id;

	@Override
	public boolean equals(Object obj) {
		if(obj!=null && obj instanceof Person) {
			return id ==((Person)obj).id;
		} else {
			return false;
		}
	}

	Person(long id) {
		this.id = id;
	}
}
```

오버라이딩한 equals() 메소드는 논리적 동치성확인 하므로, `p1.equals(p2)=true` 이다.

```java
System.out.println(p1==p2);        // false
System.out.println(p1.equals(p2)); // true
```

## 차이점, 주의점

### ex3) String 클래스의 equals()

String 클래스는 equals() 메소드를 오버라이딩하여 논리적 동치성을 확인하도록 제공하고 있다.

```java
public boolean equals(Object anObject) {
    if (this == anObject) { // 객체 자체가 동일하다면 논리적 동치성 또한 만족한다.
        return true;
    }
    return (anObject instanceof String aString)
            && (!COMPACT_STRINGS || this.coder == aString.coder)
            && StringLatin1.equals(value, aString.value);
} // value 값이 같은지, 논리적인 동치성을 확인한다.
```

위의 `StringLatin1.equals()` 메소드는 아래와 같이 String의 `value` 값이 모두 같은지 확인한다.

```java
@IntrinsicCandidate
public static boolean equals(byte[] value, byte[] other) {
    if (value.length == other.length) {
        for (int i = 0; i < value.length; i++) {
            if (value[i] != other[i]) {
                return false;
            }
        }
        return true;
    }
    return false;
}
```

`String` 클래스는 추가로 `“상수”` 로서의 기능을 제공한다.

String 은 불변(immutable) 이다. 한번 생성되면 수정할 수 없다.

아래와 같이 문자열 자체를 힙메모리의 `String Constant Pool` 에 저장한다.

```java
String a = "yejin";         // constant pool 에 저장
String b = "yejin";

String c = new String("yejin");     // String 객체 생성
String d = new String("yejin");
```

```java
System.out.println("a : "+a+" b : "+b);
System.out.println(a==b);
System.out.println(a.equals(b));
System.out.println("c : "+c+" d : "+d);
System.out.println(c==d);
System.out.println(c.equals(d));
c=c+a;
d=d+a;
System.out.println("c : "+c+" d : "+d);
System.out.println(c==d);
System.out.println(c.equals(d));
```

```java
a : yejin b : yejin
true
true
c : yejin d : yejin
false
true
c : yejin d : yejin
false
true
```

참고 : [https://www.geeksforgeeks.org/string-constant-pool-in-java/](https://www.geeksforgeeks.org/string-constant-pool-in-java/)

## ex4) equal() 를 재정의하려거든 hashCode도 재정의 하라

이전의 Person 클래스 를 `HahMap` 클래스의 원소로 사용하는 경우이다.

`id=1L`인 Person은 논리적으로 같은 Person 이므로 `“yejin”` 이라는 값을 반환해야 할 것 같지만,

아래와 같이 null 을 반환한다. (key 해당 하는 값이 없을 때)

```java
Map<Person,String> m = new HashMap<>();
m.put(new Person(1L),"yejin");
System.out.println(m.get(new Person(1L)));
```

```java

//hashCode() 구현 X
null
```

`HashMap`은 **빠른 탐색**을 위하여 **해싱함수를 이용한 해시코드**를 이용한다. 즉, get 함수는 입력한 **key 값에 대한 해시코드**를 비교한다. HashMap의 `hashCode()` 메소드는 list의 정렬을 줄이기 위하여 최대한 **중복을 줄일** 수 있도록 `Object의 hashCode()`로 구현되어 있다.

따라서, 반드시 `equals()` 를 재정의 할때는 아래와 같이 `hashCode()`도 재정의 해야 한다.

(참고로, 적절한 hashCode 메서드는 아니며, 예시로 간단한 Objects 클래스에서 제공하는 정적 hash 이다.)

```java
@Override
public int hashCode() {
    return Objects.hash(id);
}
```

```java

//hashCode() 구현 O
yejin
```

Person 클래스에 hashCode()를 추가하면, 위와 같이 “yejin” 이라는 원하는 결과를 얻을 수 있다.

( Effective Java를 참고하였습니다.)