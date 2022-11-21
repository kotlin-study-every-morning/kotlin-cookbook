### async / await 을 withContext 로 변경하기

- 문제
  - async로 코루틴을 시작하고 바로 다음에 코루틴이 완료될 동안 기다리는 await코드를 간소화하고 싶다
- 해법
  - async/await 조합을 withContext로 변경한다

```koltlin
suspend fun <T> withContext(
    context: CoroutineContext,
    block: suspend CoroutineScope.() -> T
): T
```
- 주어진 코루틴 컨텍스트와 함께 명시한 일시정시 블록을 호추랗고, 완료될 때까지 일시정지한 후에 그 결과를 리턴한다

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

// Retrieving data on DefaultDispatcher-worker-2
// Retrieving data on DefaultDispatcher-worker-2
// printing result on main withContextxResults
// printing result on main asyncResults
```
