### 부수 효과를 위해 also 사용하기

- also()
  - also는 apply와 마찬가지로 수신 객체 자신을 반환한다
  - apply가 프로퍼티를 세팅 후 객체 자체를 반환 하는데만 사용된다면, also는 프로퍼티 세팅 뿐만아니라 객체에 대한 추가적인 작업(로깅, 유효성 검사 등)을 한 후 객체를 반환할 때 사용된다
  - `public inline fun <T> T.also(block: (T) -> Unit): T`

```kotlin
val bok = createBook()
  .also { println(it) }
  .also { Logger.getAnonymousLogger().info(it.toString()) }
```
- 추가 호출을 함께 연쇄시키기에 용의하다

```kotlin
class Site(
  val name: String,
  val latitude: Double,
  val longitude: Double
)

// ..test class
@Test
fun `lat,lng of Boston, MA`() = service.getLatLng("Boston", "MA")
  .also { logger.info(it.toString()) }
  .run {
    assertThat...
    assertTaht...
  }
```
- also를 사용해 테스트를 실행하며, 위치도 출력한다
- 영역 함수를 사용함으로써 전체 테스트를 더 짧은 문법의 하나의 식으로 전환된다

> run 함수는 컨텍스트 객체 대신 람다의 값을 리턴하기 때문에 이 코드의 also 호출은 테스트의 run 호출 전에 위치해야한다
- JUnit 테스트는 Unit 리턴을 요구하지만 우연히 run 호출을 apply로 바꿔쓰더라도 잘 동작한다
- apply가 컨텍스트 객체를 리턴함에도 불구하고 예제의 run 호출은 아무것도 리턴하지 않으므로 apply로 바꿔쓸 수 있다
