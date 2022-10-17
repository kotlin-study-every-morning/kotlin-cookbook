### let 함수와 엘비스 연산자 사용하기

- let()
  - let은 수신 객체를 이용해 작업을 한 후 마지막 줄을 return 할 때 사용
  - run이나 with과는 수신객체를 접근할 때 it을 사용해야한다는 점만 다른다
  - `public inline fun <T, R> T.let(block: (T) -> R): R`

```kotlin
fun processNullableString(str: String?) = 
  str?.let { // 안전 호출 연산자와 let을 같이 사용
    when {
      it.isEmpty() -> "Empty"
      it.isBlank() -> "Blank"
      else -> it.capitalize()
    }
  } ?: "Null" // 널인 경우를 처리하는 엘비스 연산자
```
- 스프링의 RestTemplate, Webclient 같은 많은 자바 API가 결과가 없는 경우에 null을 돌려준다
- 이러한 경우에 안전 호출 연산자와 let 블록, 엘비스 연산자의 조합은 널 리턴을 처리하는 효과적인 수단이다
