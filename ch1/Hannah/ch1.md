1. kotlinc-jvm으로 컴파일 후 kotlin으로 실행
```shell
> kotlinc-jvm hello.kt // HelloKt.class 생성
> time kotlin HelloKt
```

2. 런타임을 포함해서 컴파일 후에 결과 jar 파일을 java로 실행
```shell
> kotlinc-jvm hello.kt -include-runtime -d hello.jar
> time java -jar hello.jar
```

3. kotlinc로 컴파일하고 GraalVM으로 네이티브 이미지를 생성한 다음 명령줄에서 실행
```shell
> native-image -jar hello.jar
> time ./hello
```

###


**느낀점**

1장은 설정하는거 나오고 컴파일을 해봤다
코틀린 프로젝트에서는 메이븐 프로젝트를 많이 쓸까, 그레들 프로젝트를 많이 쓸까라는 의문점이 생겼다

컴파일러 설치... 기타 라이브러리를 설치하면서 빨리 맥을 사고 싶다는 생각이 들었다...
끗
