### Random의 무작위 동작 이해하기

- 추상 Random 클래스 내의 선언
```kotlin
open fun nextInt(): Int
open fun nextInt(until: Int): Int
open fun nextInt(from:Int, unitl: Int): Int
open fun Random.nextInt(range: IntRange): Int
```

- nextInt 함수의 중복
```kotlin
Random.nextInt()
Random.nextInt(10)
Random.nextInt(5, 10)
Random.nextInt(7..12)
```

- 시드 값과 함께 난수 생성기 사용하기
```kotlin
@Test
fun `Random function produces a seeded generator`() {
    val r1 = Random(12345)
    val nums1 = (1..10).map { r1.nextInt() }

    val r2 = Random(12345)
    val nums2 = (1..10).map { r2.nextInt() }

    // 시드값이 같은 경우 nextInt 호출은 동일한 순서의 난수 값을 제공
    assertEquals(nums1, nums2)
}
```
