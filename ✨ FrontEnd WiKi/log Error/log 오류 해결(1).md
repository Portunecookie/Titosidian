## 📌 첫 화면 로딩 부분

### 1. 
```
W/Parcel  (13719): Expecting binder but got null!
```
원인 : 예상된 객체를 못 받은 경우에 발생. 찾아보니까 다른 사람들도 app이 처음 시작할 때 많이 겪게 되는 에러였다.

해결 ✅ : 해당 error 메시지는 andriod 자체에서 보내주는 메세지로, 앱에 도움이 되지 않는 문제라는 것을 찾았다. 이 문제는 플러터의 문제가 아니기 때문에 무시해도 되는 에러이다.

---

### 2. 
```
W/OnBackInvokedCallback(24004): OnBackInvokedCallback is not enabled for the application.
```
원인 : 안드로이드에서 자동으로 지원되는 기능이 아니기 때문에 발생

해결 ✅ : androidmanifest 파일에 해당 코드 추가하기  ```android:enableOnBackInvokedCallback="true"```

---

### 3.
```
I/flutter (18494): AnimatedSplashScreen -> error in jump to next screen, probably this run is in hot reload: Null check operator used on a null value
```
원인 : `AnimatedSplashScreen`을 사용하여 다음 화면으로 전환할 때 발생하는 오류이다. 
1. hot reload 의 상태관리 문제 : hot reload 자체가 애플리케이션의 상태를 유지한 채로 코드를 변경가능하게 해주는데, 해당 경우에서 이미 animation을 완료하고 화면을 전환하려할때 hot reload 가 발생하면 상태가 일치하지 않을 수 있다
2. Navigation 문제 : 다음화면으로 전환하는 네비게이션 로직에 문제가 있을 수 있다. 네비게이션 상태가 초기화되버리면 문제가 발생할 것이다

하지만 시도해본 결과, 진짜 원인은 뒷부분이었다. `Null check operator used on a null value` 이 부분이 문재였고, 해당 에러는 Flutter에서 ! 연산자를 사용해서 null 값을 허용하지 않는 변수나 속성에 null 값을 할당하려 할 때 발생한다

해결 ❌ : 
`nextScreen: const LoginMain()` 도 사용해보고, 
```
if (rootNavigatorKey.currentContext != null) {
        GoRouter.of(rootNavigatorKey.currentContext!)
            .go('/login'); // GlobalKey를 사용하여 페이지 전환
      } else {
        // 적절한 에러 처리 또는 디버깅 로그 추가
        print('Error: rootNavigatorKey.currentContext is null');
      }
```
방법으로 null을 잘못 사용할 경우를 피하기 위해 `if~else` 문을 사용해보았지만 아직 에러가 계속 뜬다. 

---

## 📌 로그인 로직 부분

1. 
```
I/flutter (19473): Error: The request connection took longer than 0:00:02.000000 and it was aborted. To get rid of this exception, try raising the RequestOptions.connectTimeout above the duration of 0:00:02.000000 or improve the response time of the server.
I/flutter (19473): Failed to sign up: DioException [connection timeout]: The request connection took longer than 0:00:02.000000 and it was aborted. To get rid of this exception, try raising the RequestOptions.connectTimeout above the duration of 0:00:02.000000 or improve the response time of the server.
```
✅
이 부분은 자주 발생하는 건데 내 컴퓨터에서만 발생한다. 나는 최악의 조건에서 돌려보고 있어서 안드로이드에서 제공하는 폰이 아니라 직접 사이즈를 지정한 애뮬레이터로 돌리고 있는데, 여기서 성능차이가 나서 로딩이 느려지는 것 같다.
회원가입을 한 후, 바로 로그인을 시도하면 잘 작동되지만 회원가입을 한 뒤 시간이 지나서 로그인을 하려 하거나, 들어간지 오래된 아이디와 비밀번호로 로그인을 시도하면 해당 에러가 뜬다. 플러터 문제가 아닌, 애뮬레이터 성능문제인 것 같다.

---

## 📌 토론리스트 부분

1. 
```
Another exception was thrown: A RenderFlex overflowed by 7.9 pixels on the right.
```

원인 : 내가 계속 해맸던 부분이었다. 해당 에러가 발생한 원인은 위젯이 주어진 공간을 넘어섰을 때 발생한다.

해결 ✅ : 해당 위젯을 스크롤 가능하게 만들거나 픽셀이 위젯공간을 넘지 않도록 수정해야 한다. 하지만 카테고리가 5개 뿐이므로 스크롤 가능하게 하는 것보다 픽셀을 수정해서 카테고리 선택바를 고정시키는 게 더 어울릴 것이라 생각했다.
Padding을 수평부분으로 이미 넣어둔 상태에서 컨테이너 크기를 지정했는데 이 과정에서 크기가 넘친 것이다
