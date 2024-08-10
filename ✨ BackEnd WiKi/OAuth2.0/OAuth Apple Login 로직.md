
## 2가지 방법

애플 로그인을 구현하기 위해서는 크게 2가지 방법을 취할 수 있다.

1. Frontend에서 모든 유저 정보가 담긴 `id_token` 을 받아서, 해당 `id_token` 이 유효한지 검증하기
2. Frontend에서 Authorization을 위한 `code` 를 받아서, 해당 `code` 로 애플 서버에서 `token` 과 `refresh_token` 받기

혹은 1과 2를 동시에 사용하여 조금 더 엄격한 인증 관리도 가능하겠다.
## Tito -> 1번 방식!!
### id_token 검증

![[Pasted image 20240811000151.png]]

이 방식의 핵심은 Client에서 API 서버로 건네주는 JWT 토큰인 `id_token` 의 payload 파트에 기본적으로 필요한 정보(`sub`, `iat`, `exp` 등)는 모두 포함되어 있다는 것이다.
-> 애플 서버에 별도의 요청을 할 필요가 없고, 이 토큰이 유효한지, 아니면 API 서버에 도달하기 전에 변조되지 않았는지 확인하면 됨.\

* 흐름 :
1. `id_token`(JWT)을 decode 해야 한다. 이 때, `complete` 옵션을 `true` 로 설정하여 `header` 까지 얻을 수 있는 형태로 decode 한다.
2. 애플의 `id_token` 의 `header` 에는 key ID인 `kid` 가 포함되어 있다. 이것이 필요하다.
3. 애플의 Public Key URL에서 JWKS(Json Web Key Set)을 가져온다.
4. 이 JWKS에는 우리가 `header` 에서 꺼내온 `kid` 에 대응되는 Key Set이 반드시 존재해야 한다. 이 Key Set으로부터 `signingKey` 를 가져온다.
5. 해당 `signingKey` 에서 `publicKey` 를 추출한다.
6. 이제 `publicKey` 를 이용하여 `id_token` 을 검증한다. 이 때 암호화 알고리즘은 `header` 에 들어있었던 알고리즘 정보를 사용하도록 한다.
7. 검증된 토큰의 payload를 반환한다.

`id_token` 의 Signature는 애플의 Private Key로 암호화 되어 있는 상태이고, 이것은 애플의 Public Key로만 복호화가 가능하다. 다르게 말해서, 애플의 Private Key가 아닌 임의의 Private Key로 암호화 되었을 경우, 애플의 Public Key로 복호화가 불가능 할 것이다.

이를 통해서 `id_token` 이 중간에 변조 되었는지, 아니면 네트워크 전송 과정에서 손상 되었는지 검증할 수 있다.

### code 검증

이 방법은 제약 사항도 몇 가지 더 생기고 애플 서버에 요청도 추가적으로 보내야 하는 단점이 있지만, 애플로부터 우리의 API 서버에 직통으로 토큰을 제공받기 때문에 해당 토큰을 100% 신뢰할 수 있다는 장점이 있다. 또한, 한 번 Refresh Token을 제공 받고, 이를 잘 관리하면 유저가 추가적으로 애플 로그인 모달을 보지 않아도 된다는 장점이 있다.
![[Pasted image 20240811000648.png]]

