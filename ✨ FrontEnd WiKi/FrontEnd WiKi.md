## CSS
* mainAxisSize: MainAxisSize.min -> 세로 사이즈 공간차지를 최소로 잡아서 중앙 정렬
mainAxisAlignment: MainAxisAlignment.center -> 중앙 정렬

### button
* foregroundColor: 글자 색 바꿀 때 사용

### padding
* EdgeInsets.all : 모든 방향 패딩
* EdgeInsets.only : 원하는 방향 패딩
* EdgeInsets.symmetric: 상하, 좌우 다르게

### margin
* SizedBox 위젯을 사용해 공간을 차지시켜 margin 되는 것 처럼 가능

### img
* color 써서 색 변경 및 투명화 가능
* Opacity를 사용하여도 가능하지만 성능적으로 좋지 않다 그래서 color 위주로 사용

### List
* List.of 배열 복사
* shuffle로 배열 랜덤 배치 가능
* ListView : 길이를 모르는 목록을 가져올 때 사용


### date
* intl 설치 후 사용
* DateFormat 




## 기능

### 실시간 댓글 
* 이걸로 가능할 듯
<img width="362" alt="image" src="https://github.com/Portunecookie/TiTo/assets/74090222/8fd785c8-44a9-4d77-b475-df57bae96e6f">

### navbar 만들 때 
* appbar사용

### TextField 글자 입력
* 로그인 유효성 검사 때 사용하면 될거 같음

### TextEditingController
* 입력된 텍스트를 수정가능
* 사용후 불필요하게 메모리를 잡아 먹어서 dispose해줘야한다
<img width="244" alt="image" src="https://github.com/Portunecookie/TiTo/assets/74090222/9259ff27-779b-486f-a111-7beb441ba92a">

### 좋아요 게시물 모아두기
 * 섹션 8


### 화면 넘어갈 때
* Navigator.of().push -> 위로 쌓이면서 뒤로 가면 빠져나감 게시물 상세 보기할 때 사용, 마이페이지나

### 로딩 스패너
- 229섹션 참고해서 만들기
### stack
*  shape 방지해서 shape혼자로는 모양 못 바꿈 clipBehavior도 해줘야함
* ![[Pasted image 20240702105953.png]]

## 백엔드 연동

### 데이터 없을 때 로딩 방지
![[Pasted image 20240704011648.png]]
### post 예시 
* shopping-list는 내가 보내는 이름
* body값을 보냄
* id는 자동생성해서 보내줄 필요 없음
* 비동기 사용하기
* context.mounted 추가해주기 화면에서 벗어났는지 확인
* 
* 추가해주기 ![[Pasted image 20240704002427.png]]
![[Pasted image 20240704002023.png]]
![[Pasted image 20240704002158.png]]
### get
* initState 추가해줘야 계속 갱신됨 
![[Pasted image 20240704002916.png]]
![[Pasted image 20240704003349.png]]
![[Pasted image 20240704003432.png]]
* 이 부분 아래처럼 수정
![[Pasted image 20240704003511.png]]
### DELETE
![[Pasted image 20240704011357.png]]

### 에러 메시지
- 섹션 233 
- try catch로 잡기
--- 
  
| @Junha, 2024.06.30.