## 예외 처리

ex1) 예외처리 try-catch 예시

```java
package org.example.LionClass.ch36;

public class ExceptionEx01 {

    static class Main{

        public static void main(String[] args) {
            try{
                int rs = Calculater.divide(10, 0);
                System.out.println(rs);
            }catch (ArithmeticException e){
                System.out.println("Can't divide by 0");
            }catch (Exception e){
                System.out.println("Cant divide");
            }
        }
    }
    private class Calculater {
        static int divide(int a, int b) throws ArithmeticException {
            return a/b;
        }
    }
}
```

<aside>
🤔 예외처리는 어디에서 하는 것이 옳은가?

</aside>

상황에 맞는 예외처리 지점을 선택해야 한다.

예외가 발생하는 지점

- try-catch

예외가 발생하는 메소드를 호출하는 지점

- 예외가 발생하는 지점 - throws Exception 표시
- 해당 메소드를 호출하는 지점 : try-catch

ex2) 적절한 예외처리 지점

- 파일이름이 유효하지 않을 때, default 값으로 생성하고 싶은 경우 : 예외지점에
- 파일이름이 유요하지 않을 때, 다시 유요한 파일이름을 받고 싶은 경우 : 호출지점에

```java
package org.example.LionClass.ch36;

import java.io.File;

public class ExceptionEx02 {

    public static void main(String[] args) {
        // command line에서 입력받은 값을 이름으로 갖는 파일을 생성한다.
        File f = createFile(args[0]);
        System.out.println( f.getName() + " 파일이 성공적으로 생성되었습니다.");

        // 사용하는 곳에서 try-catch 필요
        try {
            File f2 = createFileNoTryCatch(args[0]);
            System.out.println( f.getName()+"파일이 성공적으로 생성되었습니다.");
        } catch (Exception e) {
            System.out.println(e.getMessage()+" 다시 입력해 주시기 바랍니다.");
        }
        
        
    } 

    static File createFile(String fileName) {
        try {
            if (fileName==null || fileName.equals(""))
                throw new Exception("파일이름이 유효하지 않습니다.");
        } catch (Exception e) {
            // fileName이 부적절한 경우, 파일 이름을 '제목없음.txt'로 한다.
            fileName = "제목없음.txt";
        } finally {
            File f = new File(fileName); // File클래스의 객체를 만든다.
            createNewFile(f);		     // 생성된 객체를 이용해서 파일을 생성한다.
            return f;
        }
    }	

    static void createNewFile(File f) {
        try {
            f.createNewFile();		// 파일을 생성한다.
        } catch(Exception e){ }
    }	
    

    static File createFileNoTryCatch(String fileName) throws Exception {
        if (fileName==null || fileName.equals(""))
            throw new Exception("파일이름이 유효하지 않습니다.");
        File f = new File(fileName);		//  File클래스의 객체를 만든다.
        // File객체의 createNewFile메서드를 이용해서 실제 파일을 생성한다.
        f.createNewFile();
        return f;		// 생성된 객체의 참조를 반환한다.
    }	
    
    
}
```