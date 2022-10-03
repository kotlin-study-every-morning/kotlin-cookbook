# 시퀀스

### 지연 시퀀스 사용하기
- 3으로 나누어지는 첫 번 째 배수 찾기
```kotlin
(100 until 2_000_000).asSequence() 
    .map { println("doubling $it"); it * 2 }
    .filter { println("filtering $it"); it % == 0 }
    .first()
    
// doubling 100
// filtering 200
// doubling 101
// filtering 202
// doubling 102
// filtering 204
```
- 시퀀스의 각 원소는 다음 원소로 진행하기 전에 완전한 전체 파이프라인에서 처리되기 때문에 오직 6개의 연산만이 수행된다
- 시퀀스가 우연히 비었다면 first 함수에서 예외를 던진다
- 시퀀스가 비는 경우가 발생할 수도 있다면 first대신 firstOrNull을 사용한다
- 시퀀스에 대한 연산은 중간 연산, 최종 연산이라는 범주로 나뉜다
- map과 filter 같은 중간 연산은 새로운 시퀀스를 리턴한다
- first 또는 toList 같은 최종 연산은 시퀀스가 아닌 다른 것을 리턴한다
- 중요한 점은 최종 연산 없이는 시퀀스가 데이터를 처리하지 않는다는 것이다

> 연쇄된 함수로 구성된 파이프라인의 끝에서 최종 연산을 호출하는 경우에만 시퀀스가 데이터를 처리한다

### 시퀀스 생성하기
- 값이 있을 때 시퀀스 생성하기
- sequenceOf 함수는 arrayOf, listOf 또는 다른 연관 함수가 동작하는 방식과 똑같이 동작한다
- asSequence 함수는 기존의 Iterable 을 시퀀스로 변환한다
```kotlin
val numSequence1 = sequenceOf(3, 1, 4, 1, 5, 9)
val nunmSequence2 = listOf(3, 1, 4, 1, 5, 9).asSequence()
```

> isPrime 함수의 동작을 확인하기 위한 테스트 케이스는 이 책의 소스 코드 리포지토리에 들어있다

- 주어진 정수 다음에 나오는 소수 찾기
```kotlin
fun nextPrime(num: Int) = 
    generateSequence(num + 1) { it + 1 } // 주어진 수보다 1 큰 수에서 시작하고 1 증가 반복
        .first(Int::isPrice) // 첫 소수 값을 리턴
```

### 무한 시퀀스 다루기
- 처음 N개의 소수 찾기
```kotlin
fun firstNPrimes(count: Int) = 
    generateSequnce(2, ::nextPrime) // 2부터 시작하는 소수의 무한 시퀀스
        .take(count) // 요청한 수만큼만 원소를 가져오는 중간 연산
        .toList() // 최종 연산
```
- take 함수는 상태가 없는 중간 연산이며 처음 count 개수의 값만으로 구성된 시퀀스를 리턴한다
- 끝나 toList 함수 없이 단순히 take 함수만 실행한마녀 아무런 소수도 계산되지 않는다
- 이 경우 시퀀스는 있지만 시퀀스 안에 값은 없다
- 최종 연산인 toList는 실제로 값을 계산하는데 사용되고 계산된 모든 값을 하나의 리스트로 리턴한다

<br>

- 주어진 수보다 작은 모든 소수 
```kotlin
fun primesLessThan(max: Int): List<Int> = 
    generateSequence(2, ::nextPrime)
        .takeWhile { it < max }
        .toList()
```
- takeWhile 함수는 시퀀스에서 제공된 술어가 true를 리턴하는 동안 시퀀스에서 값을 추출한다

### 시퀀스에서 yield하기
- sequence 함수의 시그니처
```kotlin
fun <T> sequence(
    block: suspend SequenceScope<T>.() -> Unit
): Sequence<T>
```
- sequnce 함수는 주어진 블록에서 평가되는 시퀀스를 생성한다
- 이 블록은 인자 없는 람다 함수이며 void를 리턴하고 평가 후에 SequenceScope 타입을 받는다

- sequence를 사용해 피보나치 수 생성하기
```kotlin
fun fibonacciSequence() = sequence {
    var terms = Pair(0, 1)
    
    while (true) {
        yield(terms.first)
        terms = terms.second to terms.first + terms.second
    }
}
```
- yield 함수는 이터레이터에 값을 제공하고 다음 값을 요청할 때까지 값 생성을 중단한다
- 따라서 yield는 suspend 함수가 생성한 시퀀스 안에서 각각의 출력하는 데 사용된다
- yield가 중단 함수라는 사실은 yield가 코루틴과도 잘 동작한다는 의미다
- 다시 말해 코틀린 런타임은 코루틴에 값을 제공한 후에 다음 값을 요청할 때가지 해당 코루틴을 중단시킬 수 있다

- sequence 안의 yieldAll 함수
```kotlin
val sequence = sequence {
    val start = 0
    yield(start) // 단일 값(0)을 yield
    yieldAll(1..5 step 2) // 범위를 통해 순회 가능 (1, 3, 5)를 yield
    yieldAll(generateSequence(8) { it * 3 }) // 8부터 시작해 각각 값에 3을 곱한 값을 원소로 갖는 무한 시퀀스를 yield
```