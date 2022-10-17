### observable과 vetoable 대리자 사용하기

- 속성의 변경을 가로체서, 필요에 따라 변경을 거부하고 싶다
- -> 변경 감지에는 observable 함수를 사용하고, 변경의 적용 여부를 결정할 때는 vetoable 함수와 람다를 사용


- observable
```kotlin
fun <T> observable(
    initialValue: T,
    onChange: (property: KProperty<*>, oldValue: T, newValue: T) -> Unit
): ReadWriteProperty<Any?, T>

// 사용법
class User {
    var name: String by Delegates.observable("초기값") {
        prop, old, new -> println("$old 값이 $new 값으로 변경됩니다.")
    }
}
fun main() {
    val user = User()
    user.name = "홍길동"
    user.name = "임꺽정"
    
    // 초기값 값이 홍길동 값으로 변경됩니다.
    // 홍길동 값이 임꺽정 값으로 변경됩니다.
}
```

- vetoable
```kotlin
fun <T> vetoable(
    initalValue: T,
    onChange: (property: KProperty<*>, oldValue: T, newValue: T) -> Boolean
) : ReadWriteProperty<Any?, T>

// 사용법
class MoreBiggerInt(initValue: Int) {
    var value: Int by Delegates.vetoable(initValue) {
        property, oldValue, newValue -> {
        val result = newValue > oldValue
        if(result) {
            println("더 큰 값이므로 값을 변경합니다.")
        } else {
            println("작은 값이므로 변경을 취소합니다.")
        }
        result
    }()
    }
}

fun main() {
    val vv = MoreBiggerInt(10)
    vv.value = 20
    println("${vv.value}")
    // 더 큰 값이므로 값을 변경합니다.
    // 20
    
    vv.value = 5
    println("${vv.value}")
    // 작은 값이므로 변경을 취소합니다.
    // 20
}
```
  - veto 라는 단어는 거부 라는 뜻

https://kennethss.medium.com/kotlin-%EA%B3%A0%EC%B0%A8%ED%95%A8%EC%88%98%EC%99%80-inline-noinline-crossinline-reified-960f1f1511c2
