### apply로 객체 생성 후에 초기화하기

- apply()
  - apply는 수신객체 내부 프로퍼티를 변경한 다음 수신 객체 자체를 반환하기 위해 사용되는 함수
  - `public inline fun <T> T.apply(block: T.() -> Unit): T`

```kotlin
class JdbcOfficerDAO(private val jdbcTemplate: JdbcTemplate) {
	private val insertOfficer = SimpleJdbcInsert(jdbcTemplate)
		.withTableName("OFFICERS")
		.usingGeneratedKeyColumns("id")

	fun save(officer: Officer) = 
		officer.apply {
			id = insertOfficer.executeAndReturnKey(
				mapOf("rank" to rank,
							"first_name" to first,
							"last_name" to last))			
		}

}
```
- Officer 인스턴스는 this로서 apply 블록에 전달됨
- 블록 안에서 rank, first, last 속성에 접근 (자기 자신)
- Officer의 id 속성은 apply 블록 안에서 갱신된 다음 Officer 인스턴스가 리턴 (수신객체 자체를 리턴)