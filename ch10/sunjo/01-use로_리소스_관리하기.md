> 파일 같은 리소스를 처리하고 사용을 끝마쳤을 때 확실하게 리소스를 닫고 싶지만 코틀린은 자바의 try-with-resources 지원하지 않음
⇒ useLines 확장 함수를 사용
>

- kotlin에서는 try-with-resources 지원하지 않음
- Closeable에는 확장함수 use, Reader와 File에는 useLine을 추가했다.

```kotlin
fun get10LongestWordsInDictionary() = 
    File("/usr/share/dict/words")
        .useLines { line ->
            line.filter { it.length > 20 }
                .sortedByDescending(String::length)
                .take(10)
                .toList()
        }
```

useLines

- 첫번째 인자는 문자 집합
- 두번째 인자는 Sequence를 제네릭 인자 T로 매핑하는 람다
- useLines 함수 구현은 처리를 완료한 다음 리더를 자동으로 닫는다.

```kotlin
public inline fun <T> File.useLines(charset: Charset = Charsets.UTF_8, block: (Sequence<String>) -> T): T =
    bufferedReader(charset).use { block(it.lineSequence()) }
```

- BufferedReader를 생성하고 use함수에 처리를 위임
- use의 확장함수의 구현은 try-catch-finally로 되어 있음