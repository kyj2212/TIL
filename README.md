# TIL

[2022.06.06](#20220606)  
[2022.07.08](#20220708) 
[2022.07.10](#20220710)


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