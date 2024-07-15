Access Token과 Refresh Token은 JWT(JSON Web Token)의 두 가지 주요 타입으로, 각각 다른 목적과 사용 주기를 가지고 있습니다. 이 두 토큰은 인증과 권한 부여에 있어서 중요한 역할을 합니다. 이를 이해하기 쉽게 설명해볼게요.

### Access Token과 Refresh Token의 차이

#### 1. Access Token

- **목적**: Access Token은 클라이언트(사용자나 애플리케이션)가 서버에 요청을 보낼 때 사용됩니다. 주로 API 호출 시 인증을 위해 사용됩니다.
- **유효 기간**: 일반적으로 짧은 유효 기간을 가집니다 (몇 분에서 몇 시간 정도). 유효 기간이 짧기 때문에 보안성이 높습니다.
- **포함 정보**: 사용자 ID, 역할 등 인증에 필요한 정보가 포함됩니다.
- **사용 방법**: 클라이언트는 Access Token을 Authorization 헤더에 포함시켜 서버에 요청을 보냅니다.

#### 2. Refresh Token

- **목적**: Refresh Token은 Access Token이 만료되었을 때 새로운 Access Token을 발급받기 위해 사용됩니다.
- **유효 기간**: 비교적 긴 유효 기간을 가집니다 (며칠에서 몇 주 정도). 유효 기간이 길기 때문에 사용자가 자주 로그인하지 않아도 됩니다.
- **포함 정보**: Access Token을 갱신하는 데 필요한 최소한의 정보만 포함됩니다.
- **사용 방법**: 클라이언트는 Refresh Token을 사용하여 새로운 Access Token을 요청할 때 사용합니다.

### 회원가입과 로그인 시 Access Token과 Refresh Token의 생성 및 사용

#### 회원가입 (signUp) 시

회원가입 시에는 사용자가 처음으로 시스템에 등록됩니다. 이 과정에서 Access Token과 Refresh Token이 생성되어 클라이언트에게 반환됩니다. 회원가입 후 이 토큰들을 사용하여 사용자는 곧바로 인증된 상태로 서비스에 접근할 수 있습니다.

#### 로그인 (signIn) 시

사용자가 이미 등록된 상태에서 로그인을 시도할 때, 유효한 자격 증명을 제공하면 새로운 Access Token과 Refresh Token이 생성됩니다. 이 토큰들은 클라이언트에게 반환되며, 사용자는 인증된 상태로 서비스를 이용할 수 있습니다.

### 코드 분석

```java
@Transactional
public SignResponse signUp(String username, String email, String password, String introduction, Integer age, String link, String role) {
    Optional<User> existingUser = rawUserService.getOptionalUserByEmail(email);

    if (existingUser.isPresent()) {
        throw new IllegalArgumentException("이미 가입된 이메일입니다.");
    }

    password = passwordEncoder.encode(password);
    User user = rawUserService.createUser(username, email, password, introduction, age, link, role);

    AuthTokenDto accessToken = jwtService.createAccessToken(user.getId(), role);
    AuthTokenDto refreshToken = jwtService.createRefreshToken(user.getId(), role);

    user.setRefreshToken(refreshToken.getToken());

    return SignResponse.of(accessToken, refreshToken);
}

public SignResponse signIn(String email, String password) {
    User user = rawUserService.getOptionalUserByEmail(email)
        .orElseThrow(() -> new IllegalArgumentException("가입되지 않은 이메일입니다."));

    if (!passwordEncoder.matches(password, user.getPassword())) {
        throw new IllegalArgumentException("비밀번호가 일치하지 않습니다.");
    }

    AuthTokenDto accessToken = jwtService.createAccessToken(user.getId(), user.getRole());
    AuthTokenDto refreshToken = jwtService.createRefreshToken(user.getId(), user.getRole());

    user.setRefreshToken(refreshToken.getToken());

    return SignResponse.of(accessToken, refreshToken);
}

```

### 회원가입 (signUp)

1. 사용자가 입력한 이메일로 이미 가입된 사용자가 있는지 확인합니다.
2. 비밀번호를 암호화하여 저장합니다.
3. 새로운 사용자 정보를 데이터베이스에 저장합니다.
4. 새로운 Access Token과 Refresh Token을 생성합니다.
5. 생성된 Refresh Token을 사용자 엔티티에 저장합니다.
6. Access Token과 Refresh Token을 `SignResponse`로 반환합니다.

### 로그인 (signIn)

1. 사용자가 입력한 이메일로 사용자를 조회합니다.
2. 입력된 비밀번호가 저장된 비밀번호와 일치하는지 확인합니다.
3. 일치하면 새로운 Access Token과 Refresh Token을 생성합니다.
4. 생성된 Refresh Token을 사용자 엔티티에 저장합니다.
5. Access Token과 Refresh Token을 `SignResponse`로 반환합니다.

### 요약

- **Access Token**은 짧은 유효 기간 동안 API 요청을 인증하는 데 사용됩니다.
- **Refresh Token**은 Access Token이 만료되었을 때 새로운 Access Token을 발급받는 데 사용됩니다.
- **회원가입 시**: 새로운 사용자 등록 후 Access Token과 Refresh Token이 생성되어 반환됩니다.
- **로그인 시**: 사용자가 성공적으로 로그인하면 새로운 Access Token과 Refresh Token이 생성되어 반환됩니다.

이렇게 Access Token과 Refresh Token은 인증 및 권한 부여 시스템에서 중요한 역할을 하며, 각각의 목적에 따라 사용됩니다.






## 사용자 엔티티에 accessToken은 저장하지 않고, refreshToken만 저장하는 이유는?
---

사용자 엔티티에 Access Token을 저장하지 않고 Refresh Token만 저장하는 데는 여러 가지 이유가 있습니다. 주로 보안과 효율성을 위해 그렇게 설계됩니다. 다음은 그 이유들입니다:

### 1. Access Token의 짧은 유효 기간

- **유효 기간**: Access Token은 일반적으로 짧은 유효 기간(몇 분에서 몇 시간)을 가집니다. 이 짧은 유효 기간 덕분에 보안성이 높아지고, 저장할 필요성이 줄어듭니다.
- **짧은 유효 기간의 이점**: 유효 기간이 짧기 때문에 만료된 토큰은 자동으로 무효화됩니다. 만약 토큰이 탈취되더라도 짧은 시간 안에 무효화되므로 피해를 최소화할 수 있습니다.

### 2. Access Token의 자주 갱신

- **자주 갱신**: 클라이언트는 주기적으로 새로운 Access Token을 발급받아야 합니다. 따라서, Access Token을 데이터베이스에 저장할 필요가 없습니다. 클라이언트는 필요한 경우 새로운 Access Token을 요청하여 사용하면 됩니다.

### 3. Refresh Token의 긴 유효 기간

- **유효 기간**: Refresh Token은 상대적으로 긴 유효 기간(며칠에서 몇 주)을 가집니다. 이 때문에 데이터베이스에 저장하여 관리하는 것이 중요합니다.
- **안전한 저장**: Refresh Token이 탈취되면 긴 유효 기간 동안 악용될 수 있으므로, 이를 안전하게 저장하고 관리하는 것이 중요합니다.

### 4. Access Token의 상태 비저장 방식

- **상태 비저장**: Access Token은 상태 비저장(stateless) 방식으로 설계됩니다. 서버는 Access Token의 상태를 유지하지 않고, 토큰 자체에 모든 정보를 포함시킵니다. 서버는 토큰의 유효성만 검증하면 됩니다.
- **효율성**: 이 방식은 서버가 각 요청을 처리할 때마다 데이터베이스를 조회하지 않아도 되므로 효율적입니다.

### 5. 보안성

- **토큰 탈취 방지**: 만약 Access Token이 서버에 저장된다면, 데이터베이스가 해킹당했을 때 탈취된 토큰으로 인한 보안 위협이 커집니다. Access Token은 클라이언트에만 저장되며, 짧은 유효 기간 덕분에 탈취되더라도 영향이 제한적입니다.

### 요약

- **Access Token**:
    
    - 짧은 유효 기간을 가지며, 자주 갱신됩니다.
    - 상태 비저장 방식으로 사용되며, 서버는 토큰의 유효성만 검증합니다.
    - 데이터베이스에 저장하지 않습니다.
- **Refresh Token**:
    
    - 긴 유효 기간을 가지며, 데이터베이스에 저장되어야 합니다.
    - Access Token이 만료되었을 때 새로운 Access Token을 발급받기 위해 사용됩니다.
    - 안전하게 저장되고 관리되어야 합니다.

이러한 이유들로 인해, 사용자 엔티티에는 Refresh Token만 저장되고 Access Token은 저장되지 않습니다. 이는 시스템의 보안성과 효율성을 높이는 데 기여합니다.