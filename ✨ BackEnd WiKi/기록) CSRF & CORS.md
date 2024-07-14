CSRF와 CORS는 웹 보안과 관련된 중요한 개념입니다. 이 두 가지는 각각 다른 목적을 가지고 있으며, 서로 다른 문제를 해결합니다.

### CSRF (Cross-Site Request Forgery)

#### 정의

CSRF는 사용자가 의도하지 않은 행동을 하도록 하는 공격입니다. 공격자는 사용자가 인증된 상태를 이용하여 악의적인 요청을 서버에 보내는 방식입니다. 예를 들어, 사용자가 은행 웹사이트에 로그인한 상태에서 악의적인 사이트를 방문하면, 이 사이트가 사용자의 인증 정보를 이용해 은행 웹사이트에 요청을 보내 돈을 이체하는 등의 행동을 할 수 있습니다.

#### 예시

1. 사용자가 은행 사이트에 로그인합니다.
2. 사용자가 다른 탭에서 악의적인 사이트를 방문합니다.
3. 악의적인 사이트가 사용자가 로그인한 상태를 이용해 은행 서버로 요청을 보냅니다.
4. 사용자는 자신이 원하지 않는 행동이 실행됩니다.

#### 방어 방법

1. **CSRF 토큰**: 서버는 각 요청에 대해 고유한 CSRF 토큰을 생성하고 이를 클라이언트에 전달합니다. 클라이언트는 서버에 요청을 보낼 때 이 토큰을 함께 보내야 합니다. 서버는 토큰이 일치하는지 확인하여 요청의 정당성을 검증합니다.
2. **SameSite 쿠키**: SameSite 쿠키 속성을 설정하여, 쿠키가 동일한 사이트에서만 전송되도록 합니다.
3. **Referrer 검사**: 서버는 요청의 출처를 확인하여, 신뢰할 수 있는 출처에서 온 요청만 처리합니다.

#### Spring Security에서의 CSRF

Spring Security는 기본적으로 CSRF 보호 기능을 제공합니다. 이를 비활성화하려면 `http.csrf().disable()`을 사용합니다.

### CORS (Cross-Origin Resource Sharing)

#### 정의

CORS는 웹 브라우저에서 한 출처(origin)에서 실행 중인 웹 애플리케이션이 다른 출처의 리소스에 접근할 수 있도록 하는 메커니즘입니다. 기본적으로 브라우저는 보안상의 이유로 서로 다른 출처 간의 요청을 제한합니다. CORS는 이 제한을 완화하여, 특정 조건 하에서 다른 출처의 리소스에 접근할 수 있도록 합니다.

#### 예시

1. **동일 출처 정책**: 브라우저는 기본적으로 동일 출처(같은 도메인, 프로토콜, 포트)에서만 리소스를 요청할 수 있습니다.
2. **CORS 허용**: 서버는 CORS 헤더를 사용하여 특정 도메인에서 오는 요청을 허용할 수 있습니다.

#### CORS 헤더

1. **Access-Control-Allow-Origin**: 요청을 허용할 출처를 지정합니다.
2. **Access-Control-Allow-Methods**: 허용할 HTTP 메서드를 지정합니다.
3. **Access-Control-Allow-Headers**: 허용할 HTTP 헤더를 지정합니다.

#### Spring에서의 CORS 설정

Spring Security에서는 `http.cors()`를 사용하여 CORS 설정을 적용할 수 있습니다. 또한 `WebMvcConfigurer`를 구현하여 CORS 매핑을 추가할 수 있습니다.

```java
@Configuration
public class WebMvcConfiguration implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
            .allowedOrigins("http://example.com")
            .allowedMethods("GET", "POST", "PUT", "DELETE")
            .allowedHeaders("*")
            .allowCredentials(true);
    }
}

```



### 정리

- **CSRF (Cross-Site Request Forgery)**: 사용자가 의도하지 않은 행동을 하도록 하는 공격입니다. 이를 방어하기 위해 CSRF 토큰 등을 사용합니다.
- **CORS (Cross-Origin Resource Sharing)**: 웹 브라우저에서 다른 출처의 리소스에 접근할 수 있도록 하는 메커니즘입니다. 서버는 특정 도메인에서 오는 요청을 허용하기 위해 CORS 헤더를 설정할 수 있습니다.

이 두 개념은 웹 보안에서 중요한 역할을 하며, 각각의 목적과 방어 방법이 다릅니다.

---
| @eunsoA, 2024.07.12.