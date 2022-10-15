> 코드 흐름을 방해하지 않고 메세지 출력하거나 다른 부수 효과 생성
>

```kotlin
public inline fun <T> T.also(block: (T) -> Unit): T
```

- 실행시킨 후 자신을 리턴 ⇒ 추가 호출이 용이하다.(빌더패턴)

```kotlin
fun testCode() = service.getLatLng("boston", "ma")
	.also { logger.info(it.toString() }
	.run {
		assertThat...
		assertThat...
	}
```

- run은 람다의 값을 호출하기 때문에 also와 run 순서가 바뀌면 안된다.
- Junit 테스트는 Unit리턴을 요구하지만 우연히 run 호출을 apply로 바꿔 쓰더라도 잘 동작함