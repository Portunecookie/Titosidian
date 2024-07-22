# Flutter 속 Widget이란 ?
Widget은 Flutter 앱의 모든 시각적 요소를 표현한다. 
`Button`, `Text`, `mage` 등 사용자 인터페이스의 모든 요소는 위젯으로 표현된다.

- 플러터에는 내장되어 있는 다양한 Widget들이 존재한다

---
### ListView와 AnimatedList
: 현재 프로젝트 중인 앱에서 자주 사용되는 ListView

1.`ListView`
- `ListView`는 단순한 목록 표시를 할 때 주로 사용함
- 추가 / 삭제가 빈번하지 않은 경우에 적합하다
- 주로 고정된 리스트나 동적변화가 적은 리스트를 다룰 때 사용된다 (예시_설정화면 항목들)
- `ListView` 에서도 `onTap`을 통해 처리가능하지만 수동으로 구현해야 함

2. `AnimatedList`
- 항목을 삽입 및 제거할 때 기본적으로 애니메이션 지원함
- 항목이 목록에 추가되거나 삭제될 때 사용자에게 부드러운 전환효과 제공함
- 동적으로 목록 변경하는 경우에 적합함 (예시_채팅 앱에서 새 메시지가 올 떄마다 메시지 추가 시)
- `AnimatedListState`를 통해 항목 삽입 및 제거 애니메이션 제어 가능함 (`insertItem`, `removeItem` 메서드 호출)
- `GlobalKey`를 사용하여 목록 상태에 접근가능하므로 상태관리에 유용함
- 채팅 앱에서 새 메시지가 들어오거나 삭제될 때 자연스러운 애니메이션 효과 제공 가능

---

### Accessibility widgets 
>Make your app accessibility

- `ExcludeSemantics` : 하위 항목들의 모든 의미를 삭제하는 위젯 
- `MergeSemantics` : 하위 항목들의 의미를 병합하는 위젯

```dart
    MergeSemantics (
    	child: Row (
        	children: const [
            	Text('Flutter'),
                Text('Mapp'),
                ],
            ),
        );
        
/*
원래는 양 옆으로 'Flutter'와 'Mapp' Text가 나란히 출력되어야 하지만, 
두 Text 항목이 병합되어 하나의 행 공간에 column 형태로 출력된다
*/
```

- `Semantics` : 사용자 UI의 부가적인 의미를 제공해준다 
	_ex ) textField, readOnly, button, image, hint, onScrollRight ...


### Animation and motion widgets 
> Bring animations to your app

- `AnimatedAlign` : 지정된 기간 동안 child에 해당하는 부분의 위치를 바꾸는 것
	- _ex) 토론 시작 전 설명 페이지에서 움직임 줄 때  
- `AnimatedBuilder`: 복잡한 위젯 자체에 애니메이션 포함 시 
	- 단순한 경우에는 AnimatedWidget 사용 권장
    - *`AnimatedWidget`, `AnimatedController` 등 사용 시 부드러운 애니메이션 구현 가능*
    - _ex) 처음 앱 켰을 때, 앱 로고 돌아가는 것, 깜빡거리는 거나 돌아가는 거 적용 시
- `AnimatedContainer` : borders, background images, shadows, gradients, shapes, padding, width, height, alignment, transforms 등..을 적용해 애니메이션 적용할 때
- `AnimatedCrossFade`: 2개 다 위젯형태이고, 비슷한 형식인데 점진적으로 다른 위젯으로 바뀔 때 사용
	- _ex) 같은 위젯 속에서 사진이 바뀔 때, Add to cart 버튼이었다가 다 팔리면 Sold out으로 바뀔 때, 같은 위치에 있을 때 fade out, fade in 형식으로 바뀔 때 (같은 위치나 위젯 간 크기가 달라도 가능)
- `AnimatedList` : 추가 및 삭제가 빈번히 이뤄지는 List 형식 일때
	- _ex) 채팅 화면, 차단자 목록화면, 알림화면 등.. 대부분의 리스트
- `AnimatedListState` : `insertItem`과 함께 항목을 삽입하면 애니메이션이 실행되기 시작하는데 항목의 위젯이 필요할 때마다 애니메이션은 `AnimatedList.itemBuilder`로 전달된다. 	`removeItem` 으로 항목을 제거하면 해당 항목의 애니메이션이 삭제되고 제거된 항목의 애니메이션은 `removeItembuilder` 파라미터로 전달된다. 
- `AnimatedOpacity` : Fade 애니메이션 적용
- `AnimatedPositioned` : Stack에서만 작동하며 위치만 변경된 하려할 때 (가로 / 세로는 바뀌어도 됨) 
- `AnimatedSize` : 주어진 기간이나 클릭 시, 위젯크기를 바꿔줌
- `AnimatedWidget` : Listenable 한 값이 변경될 때 rebuild 되는 Widget
- `DecoratedBoxTransition` : Box에 적용가능한 다양한 속성을 애니메이션화한 것
	- _ex) 휴대폰 홈 화면에서 앱의 둥근 정도가 어떻게 띄워질지 (핸드폰 메뉴창에서)
- `FadeTransiton` : Fade 애니메이션 
- `Hero` : 페이지 경로가 팝업되거나 밀리면서 전체화면이 바뀌는 것. 
	- _ex) 인스타에서 피드게시물 사진 클릭시 전체 게시물 뜨는 방식
- `ScaleTransition` : 위젯이 서서히 확대 및 축소되는 애니메이션
