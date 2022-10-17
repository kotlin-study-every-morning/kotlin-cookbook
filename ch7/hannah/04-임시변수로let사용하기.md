### 임시 변수로 let 사용하기

```kotlin
val numbers = mutableListOf("one", "tow", "three", "four", "five")
numbers.map { it.length }.filter { it > 3 }.let(::println)
```
- let 함수는 also 함수로 바꿔 사용할 수 있다
- let 함수는 블록의 결과를 리턴하는 반면에 also는 컨텍스트 객체를 리턴한다
