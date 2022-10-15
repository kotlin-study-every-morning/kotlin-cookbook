> 어떤 속성이 필요할 때까지 해당 속성의 초기화를 지연시키고 싶다.
⇒ lazy 대리자 사용
>

```kotlin
fun <T> lazy(initializer: () -> T): Lazy<T>
```

- 처음 접근이 일어날 때 값을 계산하기 위해 만들어짐

```kotlin
val ultimateAnser: Int by lazy {
    println("Computing the answer...")
    42
}

fun main() {
    println(ultimateAnser)
    println(ultimateAnser)
}
```

- 처음 접근할 떄만 “computing the answer…” 실행이 됨
- 내부적으로 코틀린은 이 값을 캐시하는 lazy 타입의 ultimateAnswer$delegate라는 특별한 속성을 생성
- LazyThreadSafetyMode 타입의 인자는 다음과 같은 값의 enum을 받음
    - SYNCHRONIZED - 오직 하나의 스레드만 lazy 인스턴스를 초기화할 수 있게 락을 사용
    - PUBLICATION - 초기화 함수가 여러 번 호출될 수 있지만 첫 번째 리턴값만 사용
    - NONE - 락이 사용되지 않음