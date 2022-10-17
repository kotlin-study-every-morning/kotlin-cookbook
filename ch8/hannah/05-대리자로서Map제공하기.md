### 대리자로서 Map 제공하기

- 여러 값이 들어 있는 맵을 제공해 객체를 초기화 하고 싶다
- -> 코틀린 맵에는 대리자가 되는 데 필요한 getValue와 setValue 함수 구현이 있다

```kotlin
data class Project(val map: MutableMap<String, Any?>) {
    val name: String by map
    val priority: Int by map
    var completed: Boolean by map
}

val project = Project(mutableMapOf(
    "name" to "Hannah",
    "priority" to 1,
    "completed" to true
))
```
- 이 코드가 동작하는 이유는 MutableMap에 ReadWriteProperty 대리자가 되는 데 필요한 올바른 시그니처의 setValue와 getValue 확장 함수가 있기 때문이다
- 코틀린 공식 문서에서는 JSON을 파싱하거나 다른 동적인 작업을 하는 애플리케이션에서 이러한 메커니즘이 발생함을 언급한다

```kotlin
private fun getMapFromJSON() = 
    Gson().fromJson<MutableMap<String, Any?>>("json data...", MudatbleMap::class.java)
    
main() {
    val project = Project(getMapFromJSON())
    println(project) // Project(map={name=Hannah, priority=1, completed=true})
}
```

