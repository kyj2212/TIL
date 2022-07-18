

[2022.07.18](#HashMap)

## HashMap

<aside>
❓ HashMap 이란?

</aside>

- `key, value` 를 묶어서 하나의 `entry`로 저장한다.
- hashing을 사용하기 때문에 많은 양의 데이터를 검색하는데 있어 뛰어난 성능을 보인다.

### 실제 HashMap

`Node [] table` 을 선언

`Node`는 내부에 정의된 내부클래스 → `key[], val[]` 배열 2개로 선언하는 것보다 더 객체지향적이다. (integrity)

```java
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable {
	 transient Node<K,V>[] table;
		...
		static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;
...
// Map.Entry 는 Map 인터페이스에 정의된 static inner interface 
```

<aside>
🤔 참고

</aside>

- HashMap 은 value로 null을 허용한다. `put(null,null)`  `get(null)` 가능

<aside>
❓ hashing, hash function

</aside>

`hashing` 이란, `hash function`을 이용하여 데이터를 hash table에 저장하고 검색하는 기법

`hash function`은 데이터가 저장되어 있는 곳을 알려주기 때문에 다량의 데이터 중에서도 원하는 데이터를 빠르게 찾을 수 있다.

**hashing 은 `배열`과 `linkedlist`의 조합으로 되어있다.**

- 저장할 데이터의 키를 hash funtion에 넣으면, ex) `f(key) { LinkedList [] data; }`
- 배열의 한 요소를 얻게 되고 ex) `LinkedList list = data[key]`
- 다시 그곳에 연결되어 있는 linkedlist에 저장 ex) `list.push(data);`

### hsahing으로 데이터를 검색하는 과정

ex) key = 96xxxx-xxxxxxx (주민번호)

1. `hashCode = getHash(key)`; // hash function으로 hashcode를 얻는다. // hashcode=9라고 가정
2. `LinkedList list = data[hashcode];` // 해당 9번 배열에 저장된 LinkedList 가져온다.
3. `list.get(key)`  //해당 key를 갖는 데이터 찾기

<aside>
❓ 참고

</aside>

`LinkedList`는 검색에 불리한 자료구조 이므로, 빠른 검색을 하기위해 `LinkedList`의 `size`**는 작아야 한다.**

따라서 `hashcode`는 `LinkedList`에 최소한의 데이터만 저장될 수 있도록, 저장될 데이터의 크기를 고려하여 HahMap의 크기를 적절히 지정해야 한다.  **즉, 서로 다른 key에 대해 중복된 hashcode가 나오지 않도록 hash function를 만들어야 한다.**

실제, HashMap(컬렉션 클래스)에 구현된 Hash function은 `Object클래스`의 `hashCode()`를 사용한다.

해당 메소드는 객체의 주소를 기반으로 hashcode를 만들기 때문에 **모든 객체에 대해 hashcode가 서로 유일하다.** 즉, 중복된 hashcode가 나오지 않으므로 좋은 hash function 이다.

(추가)

String 클래스는 Object 클래스의 HashCode()를 오버라이딩하여 문자열의 내용으로 해시코드를 만들기 때문에, 서로 다른 객체이지만 같은 문자열을 가진다면 같은 hashcode값을 가진다.

<aside>
📝 HashMap 만들기

</aside>

- 내 소스 코드 (전체)

  UseHashMap.java

    ```java
    package com.ll.exam.hashmap;
    import java.util.ArrayList;
    import java.util.Set;
    import java.util.HashSet;
    
    public class UseHashMap {
    
        public static void main(String[] args) {
    
            HashMap<String, Integer> ages = new HashMap<>();
            ages.put("영희", 22);
            ages.put("철수", 23);
            ages.put("민서", 25);
            ages.put("철수", 27);
            ages.remove("영희");
            ages.put("광수", 27);
            for ( String name : ages.keySet() ) {
                System.out.println("이름 : " + name + ", 나이 : " + ages.get(name));
            }
            /* 출력결과 */
            // 이름 : 철수, 나이 : 27
            // 이름 : 민서, 나이 : 25
            // 이름 : 광수, 나이 : 27
            HashMap<String, Object> everyone = new HashMap<>();
            everyone.put("사람", new Human());
            everyone.put("원숭이", new Monkey());
            ((Human)everyone.get("사람")).speak();
            // 출력 => 사람이 말합니다.
            ((Monkey)everyone.get("원숭이")).performSkill();
            // 출력 => 원숭이가 묘기를 부립니다.
        }
    
        public static class Human {
            public void speak() {
                System.out.println("사람이 말합니다.");
            }
        }
    
        private static class Monkey {
            public void performSkill() {
                System.out.println("원숭이가 묘기를 부립니다.");
            }
        }
    
        public static class HashMap<T,E>{
    
            Set<T> keySet;
    
            ArrayList<Data> data;
    
            int initcapacity=10;
            int size;
    
            HashMap(){
                this.keySet=new HashSet<>();
                this.data=new ArrayList<>();
            }
    
            public Set<T> keySet(){
                return this.keySet;
            }
    
            public void put(T key, E value) {
                if(keySet.contains(key))
                    System.out.println("key 값 중복입니다.");
                    else{
                        keySet.add(key);
                        data.add(new Data(key,value));
                        size++;
                    }
    
            }
    
            private int getIdx(T key){
                if(!keySet.contains(key))
                    return -1;
                for(int i=0;i<size;i++){
                    if(data.get(i).getKey().equals(key))
                        return i;
                }
                return -1;
            }
    
            public E get(T key) {
                if(keySet.contains(key))
                    return (E) data.get(getIdx(key)).getValue();
                else return null;
            }
    
            public E remove(T key) {
                Data del = data.get(getIdx(key));
                data.remove(del);
                keySet.remove(del.getKey());
                size--;
                return (E)del.getValue();
            }
    
            static class Data<T,E>{
                private Object key;
                private Object value;
                Data(T key, E value){
                    this.key=key;
                    this.value=value;
                }
                public E getValue(){
                    return (E)this.value;
                }
    
                public T getKey() {
                    return (T)this.key;
                }
            }
    
        }
    
    }
    ```

  TDD

    ```java
    package com.ll.exam.hashmap;
    
    import static org.testng.AssertJUnit.assertEquals;
    import static org.testng.AssertJUnit.assertTrue;
    import static org.assertj.core.api.Assertions.assertThat;
    
    import com.ll.exam.arraylist.TestUtil;
    import org.testng.annotations.Test;
    import com.ll.exam.hashmap.UseHashMap.*;
    
    import java.io.BufferedOutputStream;
    import java.io.ByteArrayOutputStream;
    import java.io.IOException;
    
    public class HashMapTest {
    
        @Test
        public void test__put_and_get_intValue(){
            HashMap<String,Integer> map = new HashMap<>();
            map.put("영희",22);
            assertEquals((Integer)22, map.get("영희"));
    
        }
    
        @Test
        public void test__put_and_get_ObjectValue() throws IOException {
            HashMap<String, Object> map = new HashMap<>();
            map.put("사람", new Human());
    
            TestUtil util = new TestUtil();
            ByteArrayOutputStream output = util.toByteArrayRedirection();
    
            ((Human)map.get("사람")).speak();
    
            assertEquals("사람이 말합니다.",util.readPrintByteArray(output) );
        }
    
        @Test
        public void test__put_same_key() throws IOException {
            HashMap<String, Integer> map = new HashMap<>();
            map.put("철수", 23);
    
            TestUtil util = new TestUtil();
            ByteArrayOutputStream output = util.toByteArrayRedirection();
            map.put("철수", 27);
            assertEquals("key 값 중복입니다.",util.readPrintByteArray(output) );
    
        }
    
        @Test
        public void test__remove() throws IOException {
            HashMap<String, Integer> map = new HashMap<>();
            map.put("영희", 22);
            map.put("철수", 23);
    
            assertEquals((Integer)22,map.remove("영희"));
            TestUtil util = new TestUtil();
            ByteArrayOutputStream output = util.toByteArrayRedirection();
    
            for ( String name : map.keySet() ) {
                System.out.println("이름 : " + name + ", 나이 : " + map.get(name));
            }
            assertEquals("이름 : 철수, 나이 : 23",util.readPrintByteArray(output) );
        }
    
        @Test
        public void test__keyset() throws IOException {
            HashMap<String, Integer> map = new HashMap<>();
            map.put("영희", 22);
            map.put("철수", 23);
    
            StringBuilder sb = new StringBuilder();
            for ( String name : map.keySet() ) {
                sb.append("이름 : " + name + ", 나이 : " + map.get(name)+"\n");
            }
            String rs= sb.toString();
    
            // keySet 은 set 이므로 두가지 중 한가지가 맞으면 된다.
            String case1="""
                            이름 : 영희, 나이 : 22
                            이름 : 철수, 나이 : 23\n""".stripIndent();
            String case2="""
                            이름 : 철수, 나이 : 23
                            이름 : 영희, 나이 : 22\n""".stripIndent();
    
            assertThat(rs.equals(case1)||rs.equals(case2)).isTrue();
            assertTrue(rs.equals(case1)||rs.equals(case2));
    
        }
    
        @Test
        public void test__real_vs_mine(){
            java.util.HashMap map = new java.util.HashMap<>();
            map.put("영희",22);
            map.get("영희");
            map.remove("영희");
        }
    }
    ```


<aside>
🟪 선언부

</aside>

중복되지 않는 key → HashSet에 저장

(key,value( 조합의 클래스 Data를 이용하여 ArrayList 에 각각 저장

```java
public static class HashMap<T,E>{

  Set<T> keySet;
  ArrayList<Data> data;

  int size;

  HashMap(){
      this.keySet=new HashSet<>();
      this.data=new ArrayList<>();
  }
...
```

<aside>
🟪 void put(T key, E value) E get(T key)

</aside>

### 내 코드

Set 클래스의 contains를 이용하여 중복된 key값 확인

```java
public void put(T key, E value) {
    if(keySet.contains(key))
        System.out.println("key 값 중복입니다.");
        else{
            keySet.add(key);
            data.add(new Data(key,value));
            size++;
        }

}
private int getIdx(T key){
    if(!keySet.contains(key)) // hashSet.contains 이용하여 key 없으면 -1 반환
        return -1;
    for(int i=0;i<size;i++){
        if(data.get(i).getKey().equals(key))
            return i;
    }
    return -1;
}

public E get(T key) {
    int idx=getIdx(key); // 해당하는 key 값이 있을 경우에는 value 반환
    if(idx>=0)
        return (E) data.get(idx).getValue();
    else return null;
}
```

### 실제 HahMap의 get

Hashing 으로 데이터를 검색하도록 구현되어있다.

// LinkedList를 TreeNode라는 내부 클래스로 구현하고 있다.

```java
final Node<K,V> getNode(Object key) {
    Node<K,V>[] tab; // hashcode를 기준으로 저장된 table
		Node<K,V> first, e; 
		int n, hash; 
		K k; // key

    if ((tab = table) != null && (n = tab.length) > 0 &&
        (first = tab[(n - 1) & (hash = hash(key))]) != null) { // hash()은 Object를 상속
        if (first.hash == hash && // always check first node
            ((k = first.key) == key || (key != null && key.equals(k))))
            return first; // key의 hash값에 해당하는 table의 Node를 찾아서
        if ((e = first.next) != null) {
            if (first instanceof TreeNode) // treeNode가 LinkedList
                return ((TreeNode<K,V>)first).getTreeNode(hash, key);
            do {
                if (e.hash == hash && // 해당 LinkedList에서 key와 일치하는 value가져오기
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    return e;
            } while ((e = e.next) != null);
        }
    }
    return null;
}

// hash()
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

<aside>
🟪 remove

</aside>

해당 key에 해당하는 값을 얻어 성공적으로 지운 경우, 해당 object의 값을 반환한다.

```java
public E remove(T key) {
    Data del = data.get(getIdx(key));
    data.remove(del);
    keySet.remove(del.getKey());
    size--;
    return (E)del.getValue();
}
```

<aside>
🟪 keySet

</aside>

`keySet()`의 구현은 간단하였으나, TDD 를 구현하였을 때 `Set`은 `순서를 저장하지 않`으므로, 출력값을 하나의 expected를 갖는 `assertEquals` 구현하기 어려웠다.

```java
public Set<T> keySet(){
    return this.keySet;
}
```

### TDD

`assertj.assertThat` 와 `assertTrue`을 이용하여 case1,2에 대한 조건문으로 test 케이스를 구성하였다. (보통 `assertThat`이 다양한 method를 제공하여 많이 사용하는 것으로 확인된다.)

```java
public void test__keyset() throws IOException {
    HashMap<String, Integer> map = new HashMap<>();
    map.put("영희", 22);
    map.put("철수", 23);

    StringBuilder sb = new StringBuilder();
    for ( String name : map.keySet() ) {
        sb.append("이름 : " + name + ", 나이 : " + map.get(name)+"\n");
    }
    String rs= sb.toString();

    // keySet 은 set 이므로 두가지 중 한가지가 맞으면 된다.
    String case1="""
                    이름 : 영희, 나이 : 22
                    이름 : 철수, 나이 : 23\n""".stripIndent();
    String case2="""
                    이름 : 철수, 나이 : 23
                    이름 : 영희, 나이 : 22\n""".stripIndent();

		assertThat(rs.equals(case1)||rs.equals(case2)).isTrue();
    assertTrue(rs.equals(case1)||rs.equals(case2));

}
```

[LionClass/HashMapTest.java at master · kyj2212/LionClass](https://github.com/kyj2212/LionClass/blob/master/src/test/java/com/ll/exam/hashmap/HashMapTest.java)

[LionClass/UseHashMap.java at master · kyj2212/LionClass](https://github.com/kyj2212/LionClass/blob/master/src/main/java/com/ll/exam/hashmap/UseHashMap.java)