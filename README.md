# TIL

[2022.06.06](##2022.06.06)  
[2022.07.08](##2022.07.08) 


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