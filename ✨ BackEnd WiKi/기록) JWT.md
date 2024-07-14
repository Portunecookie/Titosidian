
``` text
JWT : 토큰 발급하고 안에있는 정보를 가져오는 역할
```


JWT(JSON Web Token)는 웹 표준 (RFC 7519)으로, 주로 사용자의 인증 정보를 안전하게 전송하는 데 사용되는 컴팩트하고 독립적인 토큰입니다. JWT는 클라이언트와 서버 간의 정보 교환을 안전하게 하고, 인증 및 권한 부여에 유용합니다.

### JWT의 구조

JWT는 세 부분으로 구성됩니다:

1. **헤더(Header)**
2. **페이로드(Payload)**
3. **서명(Signature)**

이 세 부분은 각각 Base64Url로 인코딩되고, 마침내 점(.)으로 구분된 문자열로 결합됩니다.

예를 들어:


```css
header.payload.signature
```

#### 1. 헤더(Header)

헤더는 토큰의 유형과 해싱 알고리즘 정보를 포함합니다.

```json
{   
"alg": "HS256",   
"typ": "JWT" 
}
```

- `alg`: 해싱 알고리즘 (예: HMAC SHA256 또는 RSA)
- `typ`: 토큰 유형 (여기서는 JWT)

#### 2. 페이로드(Payload)

페이로드는 클레임(claims)라고 불리는 정보를 포함합니다. 클레임은 토큰에 포함된 정보의 조각입니다.

예시 페이로드:
```json
{ 
"sub": "1234567890", 
"name": "John Doe", 
"iat": 1516239022 
}
```
- `sub`: 주제 (subject, 주로 사용자 ID)
- `name`: 사용자 이름
- `iat`: 토큰 발행 시간 (issued at)

페이로드에는 세 가지 유형의 클레임이 있습니다:

- **등록된 클레임 (Registered Claims)**: `iss` (issuer), `exp` (expiration), `sub` (subject), `aud` (audience) 등.
- **공개 클레임 (Public Claims)**: 사용자 정의 클레임으로, 충돌을 피하기 위해 URI 형식으로 사용됩니다.
- **비공개 클레임 (Private Claims)**: 두 당사자 간에 합의된 클레임입니다.

#### 3. 서명(Signature)

서명은 헤더와 페이로드를 결합한 후, 주어진 비밀 키를 사용하여 생성됩니다. 서명은 토큰의 무결성을 확인하는 데 사용됩니다.

```css
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret)
```


### JWT 사용 예

1. **사용자 로그인**:
    
    - 사용자가 로그인을 시도하면, 서버는 사용자 자격 증명을 확인합니다.
    - 확인 후 서버는 사용자 정보를 포함한 JWT를 생성하고 클라이언트에게 반환합니다.
2. **클라이언트 요청**:
    
    - 클라이언트는 이후의 모든 요청에 이 JWT를 헤더에 포함시켜 서버에 전송합니다.
    - `Authorization: Bearer <token>`
3. **서버 검증**:
    
    - 서버는 요청을 받을 때마다 JWT의 서명을 확인하고, 토큰의 유효성을 검증합니다.
    - 유효한 경우, 토큰의 페이로드에서 사용자 정보를 추출하여 요청을 처리합니다.

### JWT의 장점

1. **컴팩트**: JWT는 작은 크기로, URL, 헤더 또는 쿠키에 쉽게 포함될 수 있습니다.
2. **자기 포함적**: JWT는 필요한 모든 정보를 자체적으로 포함하므로, 별도의 데이터베이스 조회 없이도 정보를 확인할 수 있습니다.
3. **보안**: 비밀 키를 사용한 서명 또는 공개/개인 키 쌍을 사용한 서명으로 JWT의 무결성을 보장할 수 있습니다.

### 예시 코드

Spring Security와 JWT를 사용하여 인증을 구현하는 간단한 예시입니다.

#### JwtService 예시

```java
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;

import java.util.Date;

@Service
public class JwtService {
    @Value("${jwt.secret}")
    private String secretKey;

    @Value("${jwt.expiration}")
    private Long expirationTime;

    // 토큰 생성
    public String generateToken(String username) {
        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(new Date())
                .setExpiration(new Date(System.currentTimeMillis() + expirationTime))
                .signWith(SignatureAlgorithm.HS512, secretKey)
                .compact();
    }

    // 토큰 검증
    public boolean validateToken(String token) {
        try {
            Jwts.parser().setSigningKey(secretKey).parseClaimsJws(token);
            return true;
        } catch (Exception e) {
            return false;
        }
    }

    // 토큰에서 사용자 이름 추출
    public String getUsernameFromToken(String token) {
        Claims claims = Jwts.parser()
                .setSigningKey(secretKey)
                .parseClaimsJws(token)
                .getBody();
        return claims.getSubject();
    }
}

```

이제 이 서비스를 사용하여 JWT 토큰을 생성하고 검증할 수 있습니다. 이처럼 JWT는 웹 애플리케이션에서 인증 및 권한 부여를 효율적으로 처리하는 데 매우 유용한 도구입니다.