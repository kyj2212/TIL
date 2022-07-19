# TIL

[2022.06.06](#20220606)  
[2022.07.08](#20220708)  
[2022.07.10](#20220710)  
[2022.07.15](#20220715)  
2022.07.18 [JAVA/hashmap](java/CollectionFramework.md) ,  [Al/RangeSum](Algorithm/RangeSum.md)  
2022.07.20 [JAVA/identity,equality](java/IdentityAndEquality.md) , [JAVA/Exception](java/ExceptionHandling.md)
---
## 주제별 목차

### Algorithm
### JAVA


----
# Daily 회고

## 2022.06.06

#### 에라토스테네스의 체
[에라토스테네스의 체](Algorithm/수학/PrimeNumber.md)


## 2022.07.08


### JAVA

#### InputStream
String input 값을 InputStream으로 바꾸는 법
ByteArrayInputStrem(input.getBytes()) 이용하기  


#### TDD
Test 코드 메서드 - 입력값이 제대로 읽혀지는지도 테스트


```java
    
    @Test
    public void test_reader() throws IOException {
        String input = """
                regist
                내 죽음을 적에게 알리지 말라
                이순신
                """.stripIndent();
        InputStream in = new ByteArrayInputStream(input.getBytes());
        BufferedReader br = new BufferedReader(new InputStreamReader(in));
        String cmd = br.readLine().trim();
        String saying = br.readLine().trim();
        String author = br.readLine().trim();

        assertEquals("regist", cmd);
        assertEquals("내 죽음을 적에게 알리지 말라", saying);
        assertEquals("이순신", author);

    }
```


  
#### static class  
inner class 가 되어야 static class 가 가능함

```java
    class Time {
    
    static class Timer{
        
    } 
}
```

언제 static class를 쓰는가? 
- 정리 추가



## 2022.07.10

### NFT Marketplace 클론코딩 하기
[YOUTUBE Code an NFT Marketplace like OpenSea Step-by-Step [ERC-721, Solidity]](https://youtu.be/2bjVWclBD_s)  
by Dapp University

<aside>
❓ what is DAPP?

Decentralized Application

코드가 탈중앙화된 peer-to-peer network 위에서 작동하고, 데이터 호출 및 등록을
블록체인 데이터베이스로 사용하는 애플리케이션.

</aside>

<aside>
❓ what is ipfs?

InterPlanetary File System

분산형 파일 시스템에 데이터를 저장하고 인터넷으로 공유하기 위한 프로토콜

(http 보다 빠른 속도로 데이터 저장하고 가져올 수 있음)

데이터의 내용을 변환한 해시값을 이용하여 전 세계 여러 컴퓨터에 분산 저장되어 있는 콘텐츠를 찾아서 데이터를 조각조각으로 잘게 나눠서 빠른 속도로 가져온 후 하나로 합쳐서 보여주는 방식으로 작동한다.

</aside>

[http://wiki.hash.kr/index.php/IPFS](http://wiki.hash.kr/index.php/IPFS)

<aside>
❓ what is smart contract?

**스마트 컨트랙트란**

*서면으로 이루어지던 계약을 코드로 구현하고 특정 조건이 충족되었을 때 해당 계약이 이행되게 하는 script*

</aside>

예제와, smart contract가 어떻게 동작하는 지 동작 방식도 설명해 놓아서 참고하기 좋음.

[https://medium.com/haechi-audit-kr/smart-contract-a-to-z-79ebc04d6c86](https://medium.com/haechi-audit-kr/smart-contract-a-to-z-79ebc04d6c86)  

  
#### solidity 실습

```solidity
    // SPDX-License-Identifier: MIT
    SPDX 하고 ctrl-space 하면 자동 license-identifier 생김
```

```solidity
    contract NFT is ERC721URIStorage { // ERC721 쓸수 있어짐
         uint public tokenCount; // unsinged int 를 uint로 쓸수 있음
        // data를 표시할거라는데 tokenCount로 왜 그러지는지는 이해 못했음
        // public으로 두는 이유는 해당 contract를 밖에서 접근하는 key 가 될거라는 거 같음
        
        // java 처럼 contructor 를 선언하는데, contructor 선언 방식이 조금 다르네.
        // constructor() 라고 키워드를 선언하는 형식임
        constructor() ERC721("DApp NFT", "DAPP"){} 
        // name of NFT : DApp NFT
        // symbol of NFT : DAPP
        // ERC721() 이게 ERC721URIStorage 여기에 포함된 생성자인가봐.
        // intellj 처럼 parameter type 볼수 있게 설정 해봐야겟다.
            /*
            alt+f12 참고
            contract ERC721 is Context, ERC165, IERC721, IERC721Metadata {
                using Address for address;
                using Strings for uint256;
            ...
            */
    
        function mint(string memory _tokenURI) external returns(uint){
        // memory 라는 키워드가 있네.
        // _tokenURI : metadata of nft
        // returns (uint) 리턴을 이렇게 표현하네
```

```solidity
    const { ethers } = require("ethers")
```
ethers 라는 키워드를 사용하면, 자동으로 해당 파일의 최상단에 생성된다.  
ethers 를 따로 const로 선언하지 않아도 해당 프로젝트에서는 ethers를 찾아가는데,
추가한 프레임워크에서 (node_modules) 선언하고 있을 것으로 추정된다.   
해당 const를 다시 설정할경우, compile 에러 발생한다.


## 2022.07.15

# JAVA - ArrayList 구현하기

---

## 클래스 선언부 (클래스 변수와 생성자)

- 내부 배열 arr 의 초기 size 를 initCapcity로 초기화 해주었다.
- 실제 ArrayList 에는 `private static final int *DEFAULT_CAPACITY* = 10;` 로 잡혀있다.
- ArrayList<E> 리턴타입의 형변환을 위해 제네릭으로 해당 타입을 지정하였다.

```java
public static class ArrayList<E> {

  private final int initCapacity=1; // 실제 ArrayList = 10으로 잡혀있다.
  private Object[] arr;
  private int size; // 내부배열 arr의 값이 들어있는 개수를 나타내고,
										// 새로운 값을 넣을 때 그 값이 들어가는 index를 나타낸다.
	// arr[0] ~ arr[last], size=last+1 ,또는 last=size-1

  ArrayList() {
      this.arr=new Object[initCapacity];
      this.size=0;
  }
...
```

## add(object, index) , add(object)

```java
// index에 해당 data를 추가하는 add 메소드
public void add (E data, int idx) {
	if(isOverCapacity())
    grow(); // 먼저 추가하였을 때, 내부배열 arr의 capacity가 넘치면 사이즈를 늘려주기

    Object [] tmp= new Object[size-idx]; // idx ~ last 까지의 값을 저장할 tmp
    for(int i=0; i<size-idx;i++) 
        tmp[i] = arr[i+idx];
    
    arr[idx]=data; // 해당 idx 에 추가후
    for (int i=0;i<size-idx;i++) // idx+1 부터 다시 tmp 넣기
        arr[i+idx+1]=tmp[i];
    
    size++; 
}

// index를 지정하지 않는 경우
// overloading , 매개변수가 다른 add 메소드
public void add(E data){ 
    add(data,size); // 새로 넣을 위치가 last+1=size 이므로 size를 인자로 add를 호출
}
```

## grow()

- size가 기존의 capacity(=arr.length)를 넘어섰을 때 배열을 새로 생성 후, 기존 배열 copy

```java

private void grow(){
    System.out.printf("배열의 크기가 "+arr.length);
    Object tmp [] = arr.clone(); // 기존의 값을 copy할 tmp
    int newCapacity = size*2; // 새로운 capacity는 size의 두배로 지정한다. 
    arr = new Object[newCapacity];
    System.out.println("에서 "+arr.length+"으로 증가하였습니다.");
    for(int i =0;i<size;i++)
        arr[i]=tmp[i];
}

private boolean isOverCapacity(){
  //  int capacity = arr.length;
    if(size>= arr.length)
        return true;
    else return false;
}
```

## get(index)

- 현재 size보다 큰 index를 인자로 넣을 경우 실제 ArrayList는 IndexOutBoundsException를 throw 한다.
- 비슷하게 System.err 로 프로그램이 exception되지는 않도록 표기하였다.

```java
public E get (int i){
    if(checkIdxBounds(i)) // 해당 index가 boundary 안인지 체크
        return (E)arr[i]; // 해당 Object 값을 요구한 제네릭 타입으로 반환
    else return null;
}

private boolean checkIdxBounds(int idx) {
    if(idx<size) return true; // size=last+1 이므로 idx<=last(size-1) 까지 가능
    System.err.println("[IndexOutBoundsException] "+ idx+"은 boundary를 넘었습니다.");
    return false;
}
```

## removeAt(index)

- grow와 비슷하게, 해당 지우고 싶은 inx 위치를 기준으로
    - idx 전까지는 그대로
    - idx+1 부터 tmp 에 담은 후
    - idx 부터 copy

```java
public void removeAt(int del) {
    Object tmp;
    for(int i=del; i<size;i++) {
        tmp = arr[i+1];
        arr[i] = tmp;
    }
    size--;
    System.out.printf("remove : ");
    for(Object i : arr)
        System.out.printf(i+",");
    System.out.println();
}
```

## 고찰

---

1. Integer.intValue()
- 처음에는 요구사항 중 intValue() 라는 메소드를 받기 위해, Object 클래스에 해당 메소드가 없어서 Object 클래스 대신 Data 클래스를 새로 지정하였다.
- 새로 제네릭을 이용한다면 자동형변환이 가능한데, 왜 메소드를 지정하는 지 의문이 들어 찾아보니, Integer 클래스에 있는 메소드였다.

```java
// 처음에 만든 Data 클래스
// Integer 클래스를 대신하는 형태
public static class Data{
        private Object data;
        public Data(Object data){ this.data = data; }
        public Integer intValue() { return (Integer) data; }
        @Override
        public String toString() { return String.valueOf(data); }
    }
```

1. 인덱스 변수의 바운더리에 대한 표현

size라는 변수를 내부 배열의 마지막 값을 가리키는 last 변수와 혼용해서 사용했는데

```java
  private int size; // 내부배열 arr의 값이 들어있는 개수를 나타내고,
										// 새로운 값을 넣을 때 그 값이 들어가는 index를 나타낸다.
	// arr[0] ~ arr[last], size=last+1 ,또는 last=size-1
```

실제 ArrayList 는 private 으로 내부 add 함수를 통해 s 라는 last 인덱스를 가리키는 값을 이용하여 이 혼용을 줄인것으로 보인다.

```java
// 실제 ArrayList
private void add(E e, Object[] elementData, int s) {
  if (s == elementData.length)
      elementData = grow();
  elementData[s] = e;
  size = s + 1;
}
```

1. 중복 제거
- 처음에는 add() 내부에 grow를 항상 호출하고, grow에서 isOverCapacity를 확인했는데, add에서 isOverCapacity를 확인해야 불필요한 grow의 호출을 줄일수 있었다.

```java
// 초기 grow() 메소드
private void grow() {
  if(isOverCapacity()){
...
  }
}
```

1. 실제 ArrayList의 newCapacity
- 최소한으로 grow를 호출하기 위해 array의 newLenght를 구하는 메소드를 이용한다.

```java
// 실제 ArrayList의 grow() 함수내의 newCapacity
// minCapacity = 최소필요한 capacity , add()경우 old+1 
  // ArrayList.addAll 의 경우 필요한 capacity가 old+1 이 아닐수 있음 

// preferred growth = 미리 더 크게 잡아놓기 위한 growth
int newCapacity = ArraysSupport.newLength(oldCapacity,
                    minCapacity - oldCapacity, /* minimum growth */
                    oldCapacity >> 1           /* preferred growth */);
```

newLength의 실제 소스

- prefGrowh가 oldCapa>>1이라서 실제 minGrowth 보다 작은 케이스가 존재할 수 있어서

  overflow를 방지하기 위해 max 값을 가져간다고 한다.


```java
// ArraysSupoort.newLength
public static int newLength(int oldLength, int minGrowth, int prefGrowth) {
    // preconditions not checked because of inlining
    // assert oldLength >= 0
    // assert minGrowth > 0

    int prefLength = oldLength + Math.max(minGrowth, prefGrowth); // might overflow
    if (0 < prefLength && prefLength <= SOFT_MAX_ARRAY_LENGTH) {
        return prefLength;
    } else {
        // put code cold in a separate method
        return hugeLength(oldLength, minGrowth);
    }
}

public static final int SOFT_MAX_ARRAY_LENGTH = Integer.MAX_VALUE - 8;
```

1. boolean remove(Object) E remove(index)

실제 소스는 remove 가 두가지 케이스로 존재하는데,

제네릭이 나중에 추가되어 Integer타입의 Object를 지우고 싶을 때, 컴파일 바인딩이 이뤄지는 오버로딩의 한계점을 볼 수 있다.

```java
ArrayList<Integer> a = new ArrayList<>();
a.add(1); // idx=0, obj=1
a.add(2); // idx=1, obj=2
System.out.println("remove(1) : "+a.remove(1)); // obj=1 인 idx=0을 지우고 싶은데 index=1을 지운다.
//a.remove(1); // idx=1 이 없어서 outboutexception 발생
System.out.println("remove(new Integer(1)) : "+a.remove(new Integer(1)));
System.out.println("isEmpty() : "+a.isEmpty());
```

실제 ArrayList

```java
public E remove(int index) {
    Objects.checkIndex(index, size);
    final Object[] es = elementData;

    @SuppressWarnings("unchecked") E oldValue = (E) es[index];
    fastRemove(es, index);

    return oldValue;
}

public boolean remove(Object o) {
    final Object[] es = elementData;
    final int size = this.size;
    int i = 0;
    found: {
        if (o == null) {
            for (; i < size; i++)
                if (es[i] == null)
                    break found;
        } else {
            for (; i < size; i++)
                if (o.equals(es[i]))
                    break found;
        }
        return false;
    }
    fastRemove(es, i);
    return true;
}

```


# SSG - 리펙토링

---

어제 강사님의 소스와 비교를 하며 보았던 것들 중 몇가지를 리펙토링 하였다.

## Controller, Service, Ropo 의 선언부(초기화) 의 private화

default 참조변수로 선언 → private 참조변수 && 생성자에서 초기화

- 외부에서 컨트롤러 내부에 선언되어있는 service를 건드릴 수 없어진다.

```java
// 기존 방식
public class WiseSayingController {
    WiseSayingService service = new WiseSayingService();
...

// private 참조변수 생성자에서 new 초기화
public class WiseSayingController {

    private WiseSayingService service;

    public WiseSayingController() throws IOException {
        this.service=new WiseSayingService();
    }
...
```

## urlBits, paramBit

urlBit, paramBit : url 과 param을 ?, =, & 등의 기호를 기준으로 split 하여 나온 Stirng[] 을 저장

```java
// 기존에는 getPah() 메소드에서 split를 구현하여, 
//? 이후의 파라미터 값을 구하기 위해 split를 중복으로 선언함

public Rq(String url) {
    this.url=url;
    String [] urlBits=getBits(url,"\\?");
    this.path=urlBits[0];
    if(urlBits.length >1) { // 없을 때는 초기값 null
        this.queryParams=urlBits[1];
        this.queryParmMap = getQueryParm();
    }
}
```

<aside>
❓ url, param, querystring, queryparam ?

</aside>

1. url 파라미터 = 쿼리 파라미터

일반적으로 url 의 ? 뒤의 부분들을 해당 url의 파라미터 또는 쿼리문으로 이루어졌기 때문에 쿼리 파라미터 라고 칭하는 것 같다.

(아래 블로그를 참고하였으나 정확한 표현인지 더 확인해봐야할 것 같음)

[웹 데이터 분석의 기본, 파라미터를 아시나요? | 뷰저블](https://www.beusable.net/blog/?p=3798)

1. 쿼리스트링 has 쿼리 파라미터

쿼리 스트링이란 쿼리로 이루어진 문자열이기때문에, 쿼리 파라미터도 하나의 쿼리 스트링이라고 칭하는 것 같다.

[[Javascript] URL 파라미터 값 가져오기 (쿼리스트링 값)](https://hianna.tistory.com/465)