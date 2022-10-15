> 연산 결과를 임시 변수에 할당하지 않고 처리하고 싶다.
⇒ let 호출을 연쇄하고 let 에 제공한 람다 또는 함수 레퍼런스 안에서 그 결과를 처리
>

```kotlin
numbers.map { it.length }.filter { it > 3 }.let { println(it) }
numbers.map { it.length }.filter { it > 3 }.let(::println)
```

- 변수를 만들어서 println(변수) 하지 않고 let을 통해 출력

- 컨텍스트 객체를 리턴해서 뭔가를 하고 싶으면 also 사용