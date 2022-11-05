### 지금까지 해온 방식

- close() 를 통해서 직접 닫아줘야 하는 자원이 많이 있다. (`InputStream`, `OutputStream`, `java.sql.Connection` 등)
- 자원을 제대로 닫기 위해서 아래와 같이 try-finally 방식으로 구현해왔다.

try-finally 방식

```java
//try-finally방식
static void copy(String src, String dst) throws IOException {
    InputStream in = new FileInputStream(src);
    try {
        OutputStream out = new FileOutputStream(dst);
        try {
            byte[] buf = new byte[BUFFER_SIZE];
            int n;
            while ((n = in.read(buf)) >= 0)
                out.write(buf, 0, n);
        } finally {
            out.close();
        }
    } finally {
        in.close();
    }
}
```

try-with-resoucres 방식

```java
//try-with-resources방식
static void copy(String src, String dst) throws IOException {
    try (InputStream in = new FileInputStream(src);
         OutputStream out = new FileOutputStream(dst)) {
        byte[] buf = new byte[BUFFER_SIZE];
        int n;
        while ((n = in.read(buf)) >= 0)
            out.write(buf, 0, n);
    }
}
```

⇒ 이 구조를 사용하려면 해당 자원이 AutoClosable인터페이스를 구현해야 한다.

### try-with-resources 방식의 장점

1. 가독성이 좋다
    - try-finally방식은 자원이 2개 이상인 경우 try문을 중첩으로 사용하지만, try-with-resources방식은 그렇지 않다
2. 자원을 닫지 않는 실수를 막을 수 있다.
3. try-finally방식보다 문제를 진단하기에 좋다
    - 예를 들어 아래의 코드의 try-finally방식에서, 물리적인 문제 때문에 br.readLine(), br.close() 두 곳에서 예외가 발생한다고 가정했을 때

    ```java
    //try-finally방식
    static String firstLineOfFile(String path) throws IOException {
        BufferedReader br = new BufferedReader(new FileReader(path));
        try {
            return br.readLine();//실패
        } finally {
            br.close();//실패
        }
    }
    
    //try-with-resources방식
    static String firstLineOfFile(String path) throws IOException {
        try (BufferedReader br = new BufferedReader(new FileReader(path))) {
            return br.readLine();
        }
    }
    ```

    - try-finally방식은 br.readLine()에 대한 스택 추적 내역은 남지 않고 br.close()의 스택 추적 내역만 남는다
    - try-with-resources방식은 br.readLine()은 기록되고, br.close()는 '숨겨졌다'(Suppressed)는 꼬리표를 달고 출력된다.