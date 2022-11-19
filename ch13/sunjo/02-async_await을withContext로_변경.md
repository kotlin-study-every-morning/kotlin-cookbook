> async로 코루틴을 시작하고 바로 다음에 코루틴이 완료될 동안 기다리는 await 코드를 간소화하고 싶다.
⇒ async/ await을 withContext로 변경
>

- CoroutineScope에 withContext라는 확장 함수가 있음
    - 주어진 코루틴 컨텍스트와 함께 명시한 일시정지 블록을 호출하고 완료될 때까지 일시정지한 후에 그 결과를 리턴한다.
    - async await의 조합을 대체하기 위해 사용

```kotlin
suspend fun retrieve1(url: String) = coroutineScope {
    async(Dispatchers.IO) {
        println("retrieve1")
        delay(100L)
        "asyncResults"
    }.await
}

suspend fun retrieve2(url: String) = 
    withContext(Dispatchers.IO) {
        println("retrieve2")
        delay(100L)
        "asyncResults"
    }

fun main() = runBlocking<Unit> {
    val result1 = retrieve1("url")
    val result2 = retrieve2("url")
    println("result1: $result1")
    println("result2: $result2")
}
```

- withContext로 대체 가능(인텔리제이도 변경해준다.)