### TODO로 완성 강제하기

- 문제
    - 개발자가 특정 함수나 테스트를 구현을 끝마칠 수 있게 하고 싶다
- 해법
    - 함수 구현을 완선하지 않으면 예외를 던지는 TODO 함수를 (선택적 인자 reason과 함께) 사용한다

- TODO 함수의 구현
```kotlin
public inline fun TODO(reason: String): Nothing = 
    throw NotImplementedError("An operation is not implenented: $reason")
```

- 보통의 코드에서 TODO 함수 사용하기
```kotlin
fun main() {
    TODO(reason = "none, really")
}

fun completeThis() {
    TODO()
}
```

- 테스트에서 TODO 함수 사용하기
```kotlin
fun `todo test`() {
    val exception = assertThrows<NotImplementedError> {
        TODO("seriously, finish this")
    }

    assertEquals("An operation is not implemented: seriously, finish this", exception.message)
}
```


개인적으로 엄청 많이 쓸것 같다!
