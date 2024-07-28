![[Pasted image 20240728080753.png]]

1. 구글 로그인 버튼 클릭
2. 구글 서버로부터 Access token을 받는다.
3. 서버로 Access token 정보를 담은 HTTP 요청
4. 서버에서 유저 정보 응답  
    4.1. AccessToken으로 profile 가져옴  
    4.2. email을 기준으로 기존에 회원가입 되어있는 유저인지 확인  
    4.3. 응답
    - 신규 회원인 경우 : email, name, picture 를 리턴해줌 -> 회원가입 페이지로 이동
    - 기존 회원인 경우 : 로그인 JWT 토큰 리턴 (gateway에서)
5. 유저가 회원가입 페이지에서 입력한 닉네임, github url, blog url, 자기소개, 태그 정보 등을 포함하여 서버로 전송
6. 서버에서 DB에 저장