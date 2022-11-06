## 파라미터화된 테스트에 data 클래스 사용하기

```kotlin
@ParameterizedTest
@MethodSource("fibonacciTestData")
fun `check fibonacci using data class`(data: FibonacciTestData) {
    assertThat(fibonacci(data.nummber), `is`(data.expected))
}

private fun fibonacciTestData() = Stream.of(
    FibonacciTestData(number = 1, expected = 1),
    FibonacciTestData(number = 2, expected = 1),
    FibonacciTestData(number = 3, expected = 2),
    FibonacciTestData(number = 4, expected = 3),
    FibonacciTestData(number = 5, expected = 5),
    FibonacciTestData(number = 6, expected = 8),
    FibonacciTestData(number = 7, expected = 13),
)
```
