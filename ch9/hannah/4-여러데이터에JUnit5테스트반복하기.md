## 여러 데이터에 JUnit 5 테스트 반복하기

### @CsvSource()
- 테스트 함수를 위한 입력 데이터로서 문저열 리스트를 인자로 받는다
```kotlin
@ParameterizedTest
@CsvSource("1, 1", "2, 1", "3, 2", "4, 3", "5, 5", "6, 8", "7, 13")
fun `first 7 Fibonacci number (csv)`(n: Int, fib: Int)
    = assertThat(fibonacci(n), `is`(fib))
```

### @ParameterizedTest()
- 파라미터 소스 함수를 담기 위해 동반 객체 사용
```kotlin
companion object {
    // Lifecycle.PER_METHOD 수명주기로 파라미터화된 테스트를 하려면 필요
    @JvmStatic // JUnit 라이브러리(자바)는 함수를 static으로 간주하므로 필요
    fun fibs() = listOf(
            Arguments.of(1, 1), Arguments.of(2, 1),
            Arguments.of(3, 2), Arguments.of(4, 3),
            Arguments.of(5, 5), Arguments.of(6, 8)
        )
}

@ParameterizedTest(name = "fibonacci({0}) == {1}")
@MethodSource("fibs")
fun `first 6 Fibonacci numbers (companion method)`(n: Int, fib: Int)
    = assertThat(fibonacci(n), `is`(fib)
```
