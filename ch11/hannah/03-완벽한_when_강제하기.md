### 완벽한 when 강제하기

- 모든 객체에 exhaustive 속성 추가하기
  - 모든 제네릭 타입 T에 exhaustive 속성과 함께 객체 자신을 리턴하는 사용자 정의 획득자 메소드
```kotlin
val <T> T.exhaustive: T
    get() = this
```

- 숫자를 3으로 나눈 나머지
```kotlin
fun printMod3Exhaustive(n: Int) {
    when (n % 3) {
        0 -> println("$n % 3 == 0")
        1 -> println("$n % 3 == 1")
        2 -> println("$n % 3 == 2")
        else -> println("Houstom, we have a problem...")
    }.exhaustive
}
```
