
### 유용할듯! 
> 데이터 값 세트를 제공해서 Junit5 테스트 실행
⇒ Junit5 파라미터화된 테스트와 동적 테스트를 사용
>

- 데이터 세트를 사용해 함수를 테스트하고 싶다.
  - 쉼표로 구분된 값(CSV)
  - 팩토리 메서드가 포함된 옵션과 함께 데이터 소스를 명시할 수 있는 파라미터화된 테스트

```kotlin
@ParameterizedTest
@CsvSource("1, 1", "2, 1", "3, 2", "4, 3", "5, 5", "6, 8", "7, 13", "8, 21", "9, 34", "10, 55")
fun `first 10 Fibonacci numbers (csv)`(n: Int, fib: Int) = 
    assertTaht(fibonacci(n), `is`(fib))
```

```kotlin
private fun fibnumbers() = listOf(
    Arguments.of(1, 1),
    Arguments.of(2, 1),
    Arguments.of(3, 2),
    Arguments.of(4, 3),
    Arguments.of(5, 5),
    Arguments.of(6, 8))

@ParameterizedTest(name = "fib({0}) = {1}")
@MethodSource("fibnumbers")
fun `피보나치 수열`(n: Int, expected: Int) {
    assertEquals(expected, fib(n))
}
```

- Lifecycle.PER_CLASS 를 사용했다면 위처럼 사용 가능
- Lifecycle.PER_METHOD를 사용했다면 companion object 안에 있어야함