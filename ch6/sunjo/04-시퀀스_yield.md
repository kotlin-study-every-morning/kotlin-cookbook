> 구간을 지정해 시퀀스에서 값을 생성하고 싶다.
⇒ yield suspend(중단 함수)와 함께 sequence 함수를 사용하자
>

- sequence 함수는 주어진 블록에서 평가되는 시퀀스를 생성한다. 이 블록은 인자 없는 람다 함수이며 void를 리턴하고 평가 후에 SequenceScope 타입을 받는다.
- sequence 함수의 경우 필요할 때 yield해 값을 생성하는 람다를 제공

sequence를 사용하여 피보나치 수 생성

```kotlin
fun fibonacciSequence() = sequence { 
    var terms = Pair(0, 1)
    yield(terms.first)
    while (true) {
        yield(terms.second)
        terms = Pair(terms.second, terms.first + terms.second)
    }
}

// 0, 1 담고있는 Pair로부터 시작
// 람다 함수는 무한 루프를 이용해 다음 피보나치 수를 생성
// 원소가 생성될 때마다 yield 함수는 pair의 첫번째 원소를 리턴
```

```kotlin
public abstract suspend fun yield(value: T)
public abstract suspend fun yieldAll(iterator: Iterator<T>)
```

1. yield 함수는 이터레이터 값을 제공하고 다음 값을 요청할 때까지 값을 생성을 중단함
2. 시퀀스 안에서 값을 출력 (값을 반환)
3. 멈춘 시점에서 다시 실행을 재개

⇒ 중단함수로서 코루틴과 잘 맞음 (코틀린 런타임은 코루틴에 값을 제공한 후 다음 값을 요청할 때까지 해당 코루틴을 중단시킬 수 있다.)

⇒ 무한 루프가 존재하고 take연산에 의해서 해당 값을 제공 받음

- yieldAll은 다수의 값을 이터레이터에 넘겨준다.