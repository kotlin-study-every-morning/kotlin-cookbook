> 여러 값이 들어있는 맵을 제공해 객체를 초기화 하고 싶다.
⇒ 코틀린 map 에는 대리자가 되는데 필요한 getValue, setValue 함수 구현이 있다.
>

```kotlin
data class Project(val map: MutableMap<String, Any?>) {
    val name: String by map
    val priority: Int by map
    val startDate: LocalDate by map
}

fun main() {
    val project = Project(mutableMapOf(
        "name" to "Kotlin",
        "priority" to 1,
        "startDate" to LocalDate.now()
    ))
    println(project)
}

// Project(map={name=Kotlin, priority=1, startDate=2022-10-15})
```

- map 인자에 위임
- mutableMap에 ReadWriteProperty 대리자가 되는 데 필요한 올바른 시그니처의 setValue, getValue 확장 함수가 있기 때문에 가능

### 이것이 필요한 이유?

- 맵을 사용하는 대신 해당 속성을 생성자의 일부러 만들지 않는 이유가 무엇인가?

- 결론은 JSON을 받고 맵을 사용하여 Project 인스턴스를 생성하기 위해서이다. (API 통신 또는 JSON 문자열 파싱)
- 개인적인 생각으로 요즘 httpClient에서 이러한 부분을 다 해주는듯?