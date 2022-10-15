> 객체를 사용하기 전에 생성자 인자만으로는 할 수 없는 초기화 작업이 필요
>

### 예시

officer이라는 엔티티를 저장하는 동안 데이터베이스가 기본 키를 생성한다면 제공된 객체가 새로운 키로 갱신 되어야한다.

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

- officer의 id 속성은 apply 블록 안에서 갱신된 다음 Officer 인스턴스가 리턴