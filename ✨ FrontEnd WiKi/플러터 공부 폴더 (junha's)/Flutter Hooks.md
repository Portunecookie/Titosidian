
### 리액트 훅을 플러터에 맞게 구현
- 나랑 지은이가 플러터 라이프사이클을 제대로 이해하지 못한 상황이라 이걸로 코드를 다 바꾸면 좋을거 같음 -> 되면 바꾸자! dispose를 어디까지 어느정도 어떤걸 해야하는지 ex) controller 같은 건 페이지 나가도 계속 켜져있어서 성능 저하가 발생할 수 있음 따라서 dispose를 해줘야함

- StatelessWidget, StatefulWidget구분 없이 HookWidget으로 구현
- 라이프 사이클 고려하지 않아도 됨
- 보일러 플레이트 코드가 없다
	-  보일러 플레이트 코드는 최소환의 변경으로 재사용할 수 있는 코드
- 장점
	- 코드 길이가 줄어든다
	- 반환해야할 때 반환하지 못하는 등 깜빡할 때가 있음 쌓이고 쌓이면 성능저하가 될 수 있다
	- update, dispose 자동처리
	- - 라이플 사이클을 전혀 고려하지 않아도 자동으로 처리를 해줍니다.
	- UI와 상태관리에 초점을 맞춰서 개발을 할 수 있습니다.
	- 보일러 플레이트 코드를 없애주기 때문에 고민없이 사용할 수 있습니다.
- built-in 된 기능
	- useState -> 상태 변환 리액트랑 똑같은데?
	- useEffect -> Function, keyValue 사용, keyValue가 바뀔 때만 호출 된다, keyValue가 비어있으면 initState처럼 처음 들어올 때만 실행, []빈배열이라도 필수로 넣기 안넣으면 그냥 함수랑 똑같음
	- useMemoized -> 복잡한 연산을 저장해두고 사용 , 필터가 저장되어있고 필터가 바뀌지 않으면 API를 가져올 필요가 없을 때 사용, 함수+ 값 캐싱
	- useCallback -> 함수 호출 , 빈배열을 쓰면 계속 콜 함 -> 얘는 사용 안해도 될 듯
	- useTabController -> 탭관리 , navBar같은거
	- useSingleTickerProvider -> vsync
	- useAnimationController -> 애니메이션 컨트롤 시 사용
	- useReducer -> 좀 더 복잡한 상태관리, 비지니스 로직 분리 가능, 
	- useScrollController -> 스크롤
	- useTextEditingController -> 텍스트 필드
	- useFuture -> useMemoized로 캐싱을 잡아주자, 무한루프에 빠지지 않게
	- useStream -> useMemoized
# 그래서 사용해야하는가?

## 사용하는게 좋다
- 소스 코드의 양이 줄어든다
- 라이프 사이클 이해 없어도 된다
- UI나 상태관리에 집중할 수 있다

## 하지만 상태관리는 따로 하자

- useState, useMemoized, useCallback, useStream, useFuture는 사용하지 않는게 좋을거 같다 -> 이건 riverpod, Getx, bloc을 사용하는게 좋다