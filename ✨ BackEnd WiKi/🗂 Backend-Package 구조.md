---

```md
🗂 node_modules
   
🗂 src/main/java/com/example/gdscforum

    - common 🗂

        - exceptions 🗂

        - configurations 🗂

        - dto 🗂

        - enums 🗂

        - 기타 글로벌 패키지 🗂

    - domain 🗂

        - auth 🗂

            - controller 🗂

                - request 🗂

                - response 🗂

            - service 🗂

            - repository 🗂

            - entity 🗂

            - dto 🗂

        - dev 🗂

            - 이하 동일 🗂

        - post 🗂

        - 비즈니스 도메인 단위 패키지 🗂
   

build.gradle
   
settings.gradle
   
gradlew
```

- 기본적인 프로젝트의 패키지 구조입니다. 위 구조를 기본으로 하되 상황에 맞게 적절히 변형하여 사용하면 됩니다.
- `common` : 공통으로 사용되는 패키지
    - 공통으로 사용되는 설정 및 클래스들이 위치하는 패키지입니다.
    - 하위에 상황에 맞는 패키지 구조를 추가할 수 있습니다.
- `domain` : 비즈니스 도메인 단위 패키지
    - 비즈니스 도메인 단위로 패키지를 구성합니다.
    - 각 도메인 패키지 하위에 상황에 맞는 패키지 구조를 추가할 수 있습니다.