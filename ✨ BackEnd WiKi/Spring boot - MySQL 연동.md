참고자료
* https://www.jetbrains.com/help/idea/how-to-connect-to-mysql-with-named-pipes.html#step-2-configure-a-connection-in-ide
* https://tytydev.tistory.com/45
* https://spring.io/guides/gs/accessing-data-mysql
* https://changha-dev.tistory.com/147



### 원인:

에러 메시지에 따르면, 'tito_admin' 사용자가 'tito_test' 데이터베이스에 대한 접근 권한이 없기 때문에 발생한 문제입니다. 다음 부분이 이를 명확히 보여줍니다:

```error
Caused by: java.sql.SQLSyntaxErrorException: Access denied for user 'tito_admin'@'%' to database 'tito_test'
```



### 1차 해결 방법

1. **MySQL 사용자 권한 설정 확인 및 수정:** MySQL 서버에 접속하여 'tito_admin' 사용자에게 'tito_test' 데이터베이스에 대한 접근 권한을 부여해야 합니다. MySQL CLI 또는 MySQL Workbench와 같은 도구를 사용하여 다음 명령어를 실행합니다.
    
    ```sql
	GRANT ALL PRIVILEGES ON tito_test.* TO 'tito_admin'@'%'; 
	FLUSH PRIVILEGES;
```
    
2. **application.properties 또는 application.yml 파일 수정:** MySQL 데이터베이스 연결 정보가 올바르게 설정되었는지 확인합니다. 예를 들어, `application.properties` 파일에 다음과 같이 설정합니다.

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/tito_test
spring.datasource.username=tito_admin
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
```


    "application.yml" 파일을 사용하는 경우:

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/tito_test
    username: tito_admin
    password: your_password
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    hibernate:
      ddl-auto: update
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQLDialect
```


    
3. **MySQL 데이터베이스가 실행 중인지 확인:** MySQL 서버가 실행 중인지 확인합니다. MySQL 서버가 실행 중이지 않으면 애플리케이션이 데이터베이스에 연결할 수 없습니다.
    
4. **데이터베이스와 테이블 생성 확인:** 'tito_test' 데이터베이스가 존재하고 필요한 테이블들이 생성되어 있는지 확인합니다. 만약 초기화 스크립트가 필요하다면 해당 스크립트가 올바르게 실행되었는지 확인합니다.
    

위 단계를 수행한 후 애플리케이션을 다시 실행하여 문제가 해결되었는지 확인하십시오. 그래도 문제가 해결되지 않으면 MySQL 로그와 애플리케이션 로그를 통해 추가 정보를 확인하는 것이 좋습니다.



## 1차 결론
안됨 ㅅㅂ!!!!!!!!!!!!!!!!!!!!!!




### 2차 해결

1. intelliJ에 MySQL DB 연동
![[Pasted image 20240719154053.png]]



2. application.yaml 수정
```yaml
spring:  
  # MySQL Database  
  datasource:  
    driver-class-name: com.mysql.cj.jdbc.Driver  
    url: 'MySQL DB URL' 
    username: MySQL DB ID 
    password: MySQL DB PW  
  
  # JPA  
  jpa:  
    hibernate:  
      ddl-auto: create        # (none, create, create-drop, update, validate)  
    properties:  
      hibernate:  
        dialect: org.hibernate.dialect.MySQLDialect    # Dialect: MySQL DB? ?? SQL ??  
        format_sql: true      # SQL ???? ???  
        show_sql: true        # ???? SQL? ??? ??  
  
#  sql:  
#    init:  
#      mode: always  
#      schema-locations: classpath:db-init-scripts/schema.sql  
#      data-locations: classpath:db-init-scripts/data.sql  
  
  profiles:  
    active: default  
  
app:  
  jwt:  
    accessTokenValidMS: 3600000  # 1시간  
    refreshTokenValidMS: 604800000  # 7일  
  
server:  
  shutdown: graceful
```


원인 : 기존의 sql : init: 부분에서 jpa가 자동으로 테이블을 생성하는 과정과 data.sql, schema.sql 파일이 생성하면서 에러가 생기는 듯하다.

아무튼 위와 같이 h2-database를 mysql로 변경하고 sql 을 주석 처리했더니 해결 !





### 2차 결론
![[Pasted image 20240717140151.png]]

된다!


---
| @eunsoA, 2024.07.17.