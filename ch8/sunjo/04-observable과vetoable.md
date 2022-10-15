> 속성의 변경을 가로채서, 필요에 따라 변경을 거부하고 싶다.
⇒ 변경 감지에는 observable 함수 사용
⇒ 변경 적용 여부를 결정할 때는 vetoable 함수와 람다를 사용
>

```kotlin
fun main() {
    var watched: Int by Delegates.observable(1) { property, oldValue, newValue ->
        println("${property.name} changed from $oldValue to $newValue")
    }

    var checked: Int by Delegates.vetoable(0) { property, oldValue, newValue ->
        println("${property.name} changed from $oldValue to $newValue")
        newValue >= 0
    }

    watched = 2
    watched = 3
    checked = 1
    checked = 2
    checked = -1
    print(checked)
}

// watched changed from 1 to 2
// watched changed from 2 to 3
// checked changed from 0 to 1
// checked changed from 1 to 2
// checked changed from 2 to -1
// 2
```

- watched 변수의 타입은 Int 이고 1로 초기화됨
- 변수가 변경될때마다 메세지 출력
- checked는 오직 양수 값으로만 변경 가능
- observable, vetoable 모두 ObservableProperty 타입 객체를 리턴

```
inline과 crossline

inline은 해당 호출 위치를 실제 소스 코드로 대체
crossline ??

가끔 inline 함수는 지역 객체 또는 중첩 함수 같은 다른 컨텍스트에서 실행되어야 하는 파라미터로서 전달되는 람다..
```