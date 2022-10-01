# 5장 컬렉션

### 배열 다루기
- 배열 생성 : arrayOf
  - `val strings = arrayOf("this", "is", "an", "array", "of", "strings")`
- 널만 있는 빈 배열 생성 : arrayOfNulls
  - `val nullStringArray = arrayOfNulls<String>(5)`
- Array class 의 생성자
  - Int 타입의 size
  - init, 즉 (Int) -> T 타입의 람다
  - `val squares = Array(5) i -> (i * i).toString() } // {"0", "1", "4", "9", "16"}`
- autoboxing, unboxing 의 비용을 방지할 수 있는 기본 타입을 나타내는 클래스가 있다
  - booleanArrayOf, byteArrayOf, shortArrayOf ... 등 등
> kotlin에서는 명시적인 기본 타입은 없지만 값이 널 허용 값인 경우 생성된 바이트코드는 Integer와 Dobule 같은 자바 래퍼 클래스를 사용하고,
> 널 비허용 값인 경우 생성된 바이트코드는 int와 double 같은 기본 타입을 사용한다
> - null 허용 - wrapper class
> - null 비허용 - 기본 타입
- array에 withIndex를 사용하면 index와 value에 접근할 수 있다
  ```kotlin
  for ((index, value) instrings.withIndex()) {
    println("Index $index maps to $value")
  }}
  ```

### 컬랙션 생성하기
- 변경이 불가능한 컬렉션
  - listOf
  - setOf
  - mapOf
- 변경이 가능한 컬렉션
  - mutableListOf
  - mutableSetOf
  - mutableMapOf
> asList의 구현은 읽기 전용 리스트를 리턴하는 자바의 Arrays.asList에 위임한다

### 컬렉션에서 읽기 전용 뷰 생성하기
- 변경 가능한 리스트의 읽기 전용 버전을 생성하는 방법
  - toList() 함수
    - `mutableNums.toList()`
  - List타입에 가변 리스트를 할당
    - `val readOnlySameList: List<Int> = mutableNums`

### 컬렉션에서 맵 만들기
- associate, associateWith
  ```kotlin
  val keys = 'a'..'f'
  val map = keys.associate { it to it.toString().repeat(5).capitalize() }
  val map = keys.associate { it.toString().repoeat(5).capitalize() }
  println(map)
  // {a=Aaaaa, b=Bbbbb, c=Ccccc, d=Ddddd, e=Eeeee}
  ```
  
### 컬렉션이 빈 경우 기본값 리턴하기
- 컬렉션과 문자열에서 isEmpty 사용하기
```kotlin
fun onSaleProducts_ifEmptyCollection(products: List<Product>) = 
    products.filter { it.onSale }
        .map { it.name }
        .ifEmpty { listOf("none") } // 빈 컬렉션에 기본 리스트를 제공
        .joinToString(separator = ", ")
      
fun onSaleProducts_ifEmptyString(products: List<Product>) = 
    products.filter { it.onSale }
        .map { it.name }
        .joinToString(separator = ", ")
        .ifEmpty { "none" } // 빈 문자열에 기본 문자열을 제공
```

> 코틀린도 `Optional<T>`를 지원하지만 ifEmpty 함수를 사용해 특정한 값을 리턴하는 방법이 더 사용하기 쉽다

- 참고 사이트
  - [https://medium.com/@limgyumin/코틀린-nullable-타입-vs-자바-optional-e698adc6d617](https://medium.com/@limgyumin/%EC%BD%94%ED%8B%80%EB%A6%B0-nullable-%ED%83%80%EC%9E%85-vs-%EC%9E%90%EB%B0%94-optional-e698adc6d617)
  - [https://hwanchang.tistory.com/8](https://hwanchang.tistory.com/8)

### 주어진 범위로 값 제한하기
- 범위 지정
```kotlin
val range = 3..8

assertThat(5, `is` (5.coerceIn(range)))
assertThat(range.start, `is` (1.coerceIn(range))) // range.start = 3
assertThat(range.endInclusive, `is` (9.coerceIn(range))) // range.endInclusive = 9
```

- 최솟값, 최대값 지정
```kotlin
val min = 2
val max = 6

assertThat(5, `is` (5.coerceIn(min, max)))
assertThat(range.start, `is` (1.coerceIn(min, max)))
assertThat(range.endInclusive, `is` (9.coerceIn(min, max)))
```

### 컬렉션을 윈도우로 처리하기
- chunked : collection 을 주어진 크기로 나누기
  - 사실상 windowed 를 위임 (`return windoew(size, size, partialWindows = true)`)
```kotlin
val range = 0..10

val chunked = range.chunked(3)
// [[0, 1, 2], [3, 4, 5], [6, 7, 8], [9, 10]]
```

- windowed : collection 의 element 를 주어진 사이즈로 구성 가능한 모든 리스트를 얻기
  - 인자
    - size: 각 윈도우에 포함될 원소의 개수
    - step: 각 단계마다 전진할 원소의 개수 (기본 1개)
    - partialWindows: 윈도우 크기보다 작아 남게 되는 element 들을 결과에 포함 (기본 false)
```kotlin
val range = 0..10

println(range.windowed(3, step = 2))
println(range.windowed(3, step = 2, parialWindows = true))
// [[1, 2, 3], [3, 4, 5], [5, 6, 7], [7, 8, 9]]
// [[1, 2, 3], [3, 4, 5], [5, 6, 7], [7, 8, 9], [9, 10]]
```

### 리스트 구조 분해하기
```kotlin
val list = listOf("a", "b", "c", "d", "e", "f", "g")
val (a, b, c, d, e) = list
// a = "a", b = "b", c = "c", d = "d", e = "e"
```
- componentN이라는 이름의 확장 함수가 정의되어 있다 (해당 컬렉션의 N 번째 원소 리턴)
- 데이터 클래스는 정의된 모든 속성 관련 component 메소드를 자동으로 추가한다

### 다수의 속성으로 정렬하기
- sortedWith, comparedBy
```kotlin
val sorted = golfers.sortedWith(
    compareBy({ it.score }, { it.last }, { it.first })
) // ASC

val sorted = golfers.sortedWith(
    compareByDescending({ it.score }, { it.last }, { it.first })
) // DESC
```
> !! sortBy와 sortWith 함수는 자신의 원소를 제자리에서 정렬하므로 변경 가능 컬렉션을 요구한다

### 사용자 정의 이터레이터 정의하기
```kotlin
val team = Team("Warriors")
team.addPlayers(Player("Curry"), Player("Green"), Player("Durant"))

for (player in team.players) {
    println(player)
}

// custom iterator
operator fun Team.iterator(): Iterator<Player> = players.iterator()

for (player in team) {
    println(player)
}
```

### 타입으로 컬렉션을 필터링하기
```kotlin
// string 값만 저장
val strings = list.filter { it is String }

// 반환값 List<Any> 타입 캐스팅 X
for (s in strings) {
    // s.length // 컴파일 되지 않음 (타입 삭제)
}

// 구체적인 타입
val ints = list.filterIsInstane<Int>()

// 구체적인 타입을 사용해 제공된 리스트 채우기
val dates = list.filterIsInstanceTo(mutableListOf<LocalDate>())
```

### 범위를 수열로 만들기
https://torbjorn.tistory.com/748

```kotlin
// LocalDate를 위한 수열

class LocalDateProgression(
    override val start: LocalDate,
    override val endInclusive: LocalDate,
    val step: Long = 1
) : Iterable<LocalDate>, ClosedRange<LocalDate> {
    override fun iterator(): Iterator<LocalDate> = LocalDateProgressionIterator(start, endInclusive, step)
    infix fun step(days: Long) = LocalDateProgression(start, endInclusize, days)
} 

// LocalDateProgression 클래스를위한 이터레이터
internal class LocalDateProgressionIterator(
    start: LocalDate,
    val endInclusive: LocalDate,
    val step: Long
) : Iterator<LocalDate> {
    
    private var current = start
    
    override fun hasNext() = current <= endInclusive
    
    override fun next(): LocalDate {
        val next = curent
        current = current.plusDays(step)
        return next
    }
}
```

