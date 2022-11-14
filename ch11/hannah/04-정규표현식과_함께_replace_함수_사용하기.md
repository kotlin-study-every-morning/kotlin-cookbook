### 정규 표현식과 함께 replace 함수 사용하기

> 문자열 인자 또는 정규식을 받도록 중복된 String의 replace 함수를 사용

- 2개가 중복된 replace 함수
```kotlin
fun String replace(
    oldValue: String,
    newValue: String,
    ignoreCase: Boolean = false
): String

fun CharSequence.replace(
    regex: Regex,
    replacement: String
): String
```

- 중복된 두 가지 replace 사용하기
```kotlin
@Test
fun `demonstrate replace with a string vs regex`() {
    assertAll(
        { assertEquals("one*two*", "one.two.".replace(".", "*") },
        { assertEquals("********", "one.two.".replace(".".toRegex(), "*") }
    )
}
``` 

- 자바 개발자 주의
  - replace 함수는 첫 번째 항목이 아니라 발생하는 모든 항복을 교체한다 (replaceAll)
  - 첫 번째 인자로 문자열을 받는 replace 중복은 인자로 받은 문자열을 정규표현식으로 해석하지 않는다

- 코틀린 스타일로 작성한 회문 확인 함수
```kotlin
fun String.isPalindrome() = 
    this.toLowerCase().replace("""[\W+]""".toRegex(), "")
        .let { it == it.reversed() }
```

- 두 가지 접근 방식은 동일하게 동작하겠지만 좀 더 숙련된 코틀린 개발자라면 두 번째 접근 방식을 사용할 가능성이 높다
- 두 방식을 모두 숙지하는 것이 최선이다
