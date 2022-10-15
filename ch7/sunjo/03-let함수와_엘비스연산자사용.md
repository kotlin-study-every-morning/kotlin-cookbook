> 오직 널이 아닌 레퍼런스의 코드 블록을 실행하고 싶지만 래퍼런스가 널이라면 기본값을 리턴하고 싶다.
>

- let은 객체가 아닌 블록의 결과를 리턴한다.

```kotlin
fun main() {
    val person = Person("홍길동", 30)
    val let = person.let { it: Person ->
        it.name
    }
    println(let)
}

// 홍길동
```

```kotlin
fun processString(str: String?): String {
    val s: String = str.let {
        when {
            it.isEmpty() -> "Empty"
            it.isBlank() -> "Blank"
            else -> it.capitalize()
        }
    } ?: "null"
    return s
}
```

- map처럼 컨텍스트 객체의 변형처럼 동작
- ?. 와 let 함수, ?: 조합으로 null값을 쉽게 처리가 가능하다.