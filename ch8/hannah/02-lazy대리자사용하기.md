### lazy 대리자 사용하기

- lazy
  - 처음 접근이 일어날 때 값을 계산하기 위해 만들어졌다
  - `fun <T> lazy(initializer: () -> T): Lazy<T>`
    - 기본. 스스로 동기화
  - `fun <T> lazy(mode: LazyThreadSafdtyMode, initializer: () -> T): Lazy<T>`
    - lazy 인스턴스가 다수의 스레드 간에 초기화를 동기화하는 방법을 명시
  - `fun <T> lazy(lock: Any?, initializer: () -> T): Lazy<T>`
    - 동기화 락을 위해 제공된 객체를 사용
  - 내부적으로 코틀린은 이 값을 캐시하는 lazy 타입의 ultimateAnswer$delegate라는 특별한 속성을 생성
  - LazyThreadSafetyMode 타입의 인자 enum을 받음
    - SYNCHRONIZED: 오직 하나의 스레드만 Lazy 인스턴스를 초기화할 수 있게 락을 적용
    - PUBLICATION: 초기화 함수가 여러 번 호출될 수 있지만 첫 번째 리턴값만 사용됨
    - NONE: 락이 사용되지 않음
