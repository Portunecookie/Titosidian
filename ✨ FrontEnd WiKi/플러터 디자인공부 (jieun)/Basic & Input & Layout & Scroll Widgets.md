# 프로젝트에서 사용할 Widget들 정리
---

# Basic Widets
- `FlutterLogo` : 앱 로고를 image말고 해당 위젯을 사용해야 할 것 같다
- `Placeholder` : 한 화면에 각각 다른 UI를 실행해야 할 때 이용되며 이 위젯은 가능한 모든 공간을 채울 수 있도록 확장된다
	- _ex) ListView 같은 상위요소 안에서 위젯이 자리를 차지하고 있는 경우에는 `fallbackWidth` , `fallbackheight`를 이용해 높이나 폭을 제한할 수 있다
    
---
   
# Input Widgets
- `Autocomplete` : 일부 텍스트를 입력하고 옵션 목록 중에서 선택하여 사용자가 선택할 수 있도록 도와주는 위젯
	- _ex) 검색 창 자동완성 기능
---

# Layout Widgets
- `FittedBox` : 이미지같은 것 해당 컨테이너에 맞도록 맞춰끼우는 것 
	- _ex) 토론리스트에서 보이는 이미지
- `CupertinoSliverNavigationBar` : Appbar가 있었는데 없었어요 느낌으로, 스크롤 올리면 appbar 가 올려져서 custom 한 대로 바뀌는 것
- `SliverChildBuilderDelegate` : GridView나 List들을 scroll되는 한 화면에 다 나타내고 싶을때

---

# Scroll Widgets
- `Scrollbar` : 스크롤 내릴 때 표시되는 스크롤 바
- `RefreshIndicator` : 아래로 scroll 시 새로고침 되는 화살표가 표시되는 방식
- `PageView` : Page자체가 옆으로 넘어가는 scroll처럼 보임 (단계설명 할때 적절할 듯) 
- `NestedScrollView` : scroll 위치가 고유하게 연결된 다른 scroll view를 중첩할 수 있다. 
	- _ex) 헤더에 TabBar르 포함해서 SliverAppBar를 포함하는 scroll를 사용하는 상황 (카테고리에 따른 scorll 가능한 list 표시시)
 
- `DraggableScrollableSheet` : scroll 하는 부분까지 화면 창이 올라오고 내려오고 하는 것
	- _ex) 실시간 채팅 scroll 해서 올리고 내리고 가능

---

# Touch Interaction Widgets
- `Dismissible` : 슬라이드 옆으로 하면 삭제되는 것 같은 리스트
- `GestureDetector` : 제스터를 감지하는 위젯
- `IgnorePointer` : Ignore를 걸면 버튼이 클릭안되는 것 같은 효과
