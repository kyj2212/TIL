

[2022.07.18](#구간합(RangeSum))

## 구간합 (Range Sum)

[Range sum queries without updates - GeeksforGeeks](https://www.geeksforgeeks.org/range-sum-queries-without-updates/)

합 배열을 이용하여 시간 복잡도를 더 줄이기 위해 사용하는 특수한 목적의 알고리즘

## 문제 1) input 1차원

[11659번: 구간 합 구하기 4](https://www.acmicpc.net/problem/11659)

### 핵심 이론

누적 합( prefix sum) 을 저장하는 배열을 미리 생성한다.

[Range sum queries without updates - GeeksforGeeks](https://www.geeksforgeeks.org/range-sum-queries-without-updates/)

[Prefix Sum Array - Implementation and Applications in Competitive Programming - GeeksforGeeks](https://www.geeksforgeeks.org/prefix-sum-array-implementation-applications-competitive-programming/)

```java
prefixSum[i] = arr[0] + arr[1] + arr[2] … arr[i]
```

Query = 구간 (qs~qe) 까지의 합 을 구하면,

```java
qeury(qs,qe) = prefixSum[qe] - prefixSum[qs-1]
```

### 처음 내 코드

합배열(누적합,prefix sum) 을 구할 때, input[] 에 입력을 다 받아 저장 후, 그값을 매개변수로 구하였음 → 총 2N 의 공간을 사용하였으나, 입력을 받을때 바로 sum 을 구하는 것이 효율적일 것 같다.

- 52372kb
- 596ms
- 전체 소스

    ```java
    package RangeSum;
    
    import java.io.*;
    import java.util.StringTokenizer;
    
    public class RangeSumEasy {
    
        public static void main(String[] args) throws IOException {
    
            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            StringTokenizer st = new StringTokenizer(br.readLine()," ");
            BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
    
            int N = Integer.parseInt(st.nextToken());
            int M = Integer.parseInt(st.nextToken());
    
    				// input 배열을 다 저장한 후에
            int [] input=new int[N];
            st = new StringTokenizer(br.readLine()," ");
            for (int i = 0; i < N; i++) {
                input[i]=Integer.parseInt(st.nextToken());
            }
    
    				// 해당 배열에 대한 누적합 배열을 저장
            int [] prefixSum = prefixSum(input);
    
            for(int i=0;i<M;i++){
                st = new StringTokenizer(br.readLine()," ");
                int qs = Integer.parseInt(st.nextToken())-1;
                int qe = Integer.parseInt(st.nextToken())-1;
                bw.write(String.valueOf(rangeSum(qs,qe, prefixSum)));
                bw.newLine();
            }
    
            br.close();
            bw.flush();
            bw.close();
        }
    
        private static int[] prefixSum(int[] input) {
            int [] sum= new int[input.length];
            sum[0]=input[0];
            for(int i=0;i<input.length-1;i++){
                sum[i+1]=sum[i]+input[i+1];
            }
            return sum;
        }
    
        static int rangeSum(int qs, int qe, int[] prefixSum){
            return prefixSum[qe]-(qs>0?prefixSum[qs-1]:0);
        }
    
    }
    ```


### 자바 Do it 참고하여 변경

아래 두가지 케이스를 다 시도하였으나, 시간,메모리 거의 동일

300ms 정도가 나오는 사람들의 정답은 입출력을, stream을 사용하지 않고, 자체적으로 main() 메소드의 args로 받은 string값을 버퍼로 받는 입출력 메소드를 따로 생성하여 사용…

굳이 이정도는 하지 않아도 될것 같아 생략

- input []  X → 바로 sum[] 수행
- stringbuilder 사용
- 수정한 소스

    ```java
    int [] prefixSum = new int[N];
    st = new StringTokenizer(br.readLine()," ");
    
    prefixSum[0]=Integer.parseInt(st.nextToken());
    for (int i = 0; i < N-1; i++) {
        prefixSum[i+1]=prefixSum(Integer.parseInt(st.nextToken()),prefixSum[i]);
    }
    
    private static int prefixSum(int input,int sum) {
        return sum+input;
    }
    ```

    ```java
    StringBuilder sb=new StringBuilder();
    
    	for(int i=0;i<M;i++){
    	    st = new StringTokenizer(br.readLine()," ");
    	    int qs = Integer.parseInt(st.nextToken())-1;
    	    int qe = Integer.parseInt(st.nextToken())-1;
    	    sb.append(rangeSum(qs,qe, prefixSum)).append("\n");
    	}
    	bw.write(sb.toString());
    ```


[https://github.com/kyj2212/Algorithm/commit/81ad0c64a6792e924bd3cbad3962ffe2dd71a421](https://github.com/kyj2212/Algorithm/commit/81ad0c64a6792e924bd3cbad3962ffe2dd71a421)

## 문제 2) input 2차원

[11660번: 구간 합 구하기 5](https://www.acmicpc.net/problem/11660)

### 처음 내 코드

2차원 → 1차원

(2,2) → (3,4)

(2,2)+(2,3)+(2,4)+(3,1)+(3,2)+(3,3)+(3,4)  인줄 알았으나,

(2,2)+(2,3)+(2,4)+(3,2)+(3,3)+(3,4) 였음

- 전체 소스

    ```java
    package RangeSum;
    
    import java.io.*;
    import java.util.StringTokenizer;
    
    public class RangeSum02 {
    
        public static void main(String[] args) throws IOException {
    
            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            StringTokenizer st = new StringTokenizer(br.readLine()," ");
            BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
            StringBuilder sb=new StringBuilder();
    
            int N = Integer.parseInt(st.nextToken());
            int M = Integer.parseInt(st.nextToken());
    
            //int[] input = new int[N*N];
            int[] prefixSum=new int[N*N];
    
            for(int i=0;i<N;i++){
                st = new StringTokenizer(br.readLine()," ");
                for (int j = 0; j < N; j++) {
                    //input[i*N+j]=Integer.parseInt(st.nextToken());
                    if(i*N+j<1){
                        prefixSum[0]=Integer.parseInt(st.nextToken());
                        continue;
                    }
                    prefixSum[i*N+j]=prefixSum[i*N+j-1]+Integer.parseInt(st.nextToken());
                }
            }
    
            for(int i=0;i<M;i++){
                st = new StringTokenizer(br.readLine()," ");
                int qs=N*(Integer.parseInt(st.nextToken())-1)+Integer.parseInt(st.nextToken())-1;
                int qe=N*(Integer.parseInt(st.nextToken())-1)+Integer.parseInt(st.nextToken())-1;
                if(qs<1)
                    sb.append(prefixSum[qe]).append("\n");
                else
                    sb.append(prefixSum[qe]-prefixSum[qs-1]).append("\n");
            }
    
            br.close();
            bw.write(sb.toString());
            bw.flush();
            bw.close();
    
        }
    }
    ```


### 수정 - 2차원 배열, 행마다 prefixSum

2차원배열로 수정

각 행마다 sum 을 구하여 저장

```java
prefixSum[i][j] = input[i][0] + ...+ input[i][j]
```

(qsx, qsy) → (qex,qey) 일때,

```java
각 행마다
i=qsx 부터 i=qey 까지
prefixSum[i][qey] - prefixSum[i][qsy-1]
모두 더한다.
```

- 전체 소스

    ```
    package RangeSum;
    
    import java.io.*;
    import java.util.StringTokenizer;
    
    public class RangeSum02 {
    
        public static void main(String[] args) throws IOException {
    
            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            StringTokenizer st = new StringTokenizer(br.readLine()," ");
            BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
            StringBuilder sb=new StringBuilder();
    
            int N = Integer.parseInt(st.nextToken());
            int M = Integer.parseInt(st.nextToken());
    
    //int[] input = new int[N*N];
            //int[] prefixSum=new int[N*N];
    int[][] prefixSum=new int[N][N];
    
            for(int i=0;i<N;i++){
                st = new StringTokenizer(br.readLine()," ");
    //prefixSum[i*N]=Integer.parseInt(st.nextToken());
    prefixSum[i][0]=Integer.parseInt(st.nextToken());
                for (int j = 1; j < N; j++) {
    //input[i*N+j]=Integer.parseInt(st.nextToken());
    prefixSum[i][j]=prefixSum[i][j-1]+Integer.parseInt(st.nextToken());
                }
            }
    
            for(int i=0;i<M;i++){
                st = new StringTokenizer(br.readLine()," ");
                int qsx=Integer.parseInt(st.nextToken())-1;
                int qsy=Integer.parseInt(st.nextToken())-1;
                int qex=Integer.parseInt(st.nextToken())-1;
                int qey=Integer.parseInt(st.nextToken())-1;
    // (qsx,qsy) ~ (qsx,qey) + (qex,qsy) ~ (qex,qey)
    int sum=0;
                for(int x=qsx;x<=qex;x++){
                    sum+=prefixSum[x][qey]-(qsy>0?prefixSum[x][qsy-1]:0);
                }
                sb.append(sum).append("\n");
    //if(N*qsx+qsy<1||N*qex+qsy<1)
                //  sb.append(prefixSum[N*qsx+qey]+prefixSum[N*qex+qey]);
                //sb.append(prefixSum[N*qsx+qey]-prefixSum[N*qsx+qsy-1]+prefixSum[N*qex+qey]-prefixSum[N*qex+qsy-1]).append("\n");
    
    }
    
            br.close();
            bw.write(sb.toString());
            bw.flush();
            bw.close();
    
        }
    }
    
    ```


120040kb 1456ms

수행시간이 아주 오래걸림

query의 수행시간이 O(MN) 이 걸림.

qeury의 수행시간이 O(M) 이 걸리도록 수정 필요

### 수정 - dp로 접근

prefixSum[i][j] = (0,0) ~ (i,j)의 합

```java
int[][] prefixSum=new int[N][N];

for(int i=0;i<N;i++){
    st = new StringTokenizer(br.readLine()," ");
    for (int j = 0; j < N; j++) {
        prefixSum[i][j]=(j<1?0:prefixSum[i][j-1])
												+(i<1?0:prefixSum[i-1][j])
												-((i<1||j<1)?0:prefixSum[i-1][j-1])
												+Integer.parseInt(st.nextToken());
    }
}
```

(qsx,qsy)~(qex,qey) = sum(qex,qey) - sum(qsx-1,qey) - sum(qex,qsy-1) +sum(qsx-1,qsy-1)

```java
// s(qex,qey) - s(qsx-1,qey) - s(qex,qsy-1) + s(qsx-1,qsy-1)
int sum=prefixSum[qex][qey] - (qsx<1?0:prefixSum[qsx-1][qey]) 
-(qsy<1?0:prefixSum[qex][qsy-1]) +((qsx<1||qsy<1)?0:prefixSum[qsx-1][qsy-1]);
```

117060kb 872ms

수행시간 O(1)*q = O(q)

수행시간 단축됨.

### 아이디어 - 입력할때, prev 값을 이용하자 - 불가능

```java

for(int i=1;i<N+1;i++)
int prev=0;
for(int j=1;j<N+1;j++)
int next=Integer.parseInt(st.nexttoken());
prefixSum[i][j]=prefix[i-1][j]+prev+next;

int prev=next;
```

## 문제 3) 나머지 합

[10986번: 나머지 합](https://www.acmicpc.net/problem/10986)

### 풀이1) 시간초과

단순히 O(NlogN) query 수행

- 전체소스

    ```java
    package RangeSum;
    
    import java.io.*;
    import java.util.StringTokenizer;
    
    public class RemainRangeSum {
    
        public static void main(String[] args) throws IOException {
    
            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            StringTokenizer st = new StringTokenizer(br.readLine()," ");
            BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
    
            int N = Integer.parseInt(st.nextToken());
            int M = Integer.parseInt(st.nextToken());
    
            long [] prefixSum = new long[N];
            st = new StringTokenizer(br.readLine()," ");
            prefixSum[0]=Long.parseLong(st.nextToken());
            for (int i = 0; i < N-1; i++) {
                prefixSum[i+1]=Long.parseLong(st.nextToken())+prefixSum[i];
            }
    
            int cnt=0;
            if(prefixSum[0]%M==0)
                cnt++;
            for(int qs=0;qs<N;qs++){
                for (int qe = qs; qe <N ; qe++) {
                    if((prefixSum[qe]-(qs<1?0:prefixSum[qs-1]))%M==0)
                        cnt++;
                }
    
            }
            bw.write(String.valueOf(cnt));
            br.close();
            bw.flush();
            bw.close();
        }
    }
    
    ```


### 풀이2) 추후 진행