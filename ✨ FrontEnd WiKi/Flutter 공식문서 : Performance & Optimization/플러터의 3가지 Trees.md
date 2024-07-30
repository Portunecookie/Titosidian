# 1. Widget tree
`build()`에서 return한 Widget은 ***Widget tree*** 로 간다
![](https://velog.velcdn.com/images/oojoo/post/dee21945-1f65-4fe4-98ab-409f9cbd026a/image.png)

`MyApp` 클래스에서의 `build()`에서는 `MaterialApp`을 리턴하고, `HomeScreen()`의 `build()`에서는 `Scaffold` 를 리턴하고 있다

`build` 함수에서 리턴한 위젯은 위젯트리로 들어가는데, 이 위젯 트리의 root node는 `MyApp` 이다

- 플러터 전체에서 사용하는 widget tree는 단 1개뿐이다
- 각 `build()` 메소드의 리턴 결과는 `build()`를 실행할 당시에 있던 노드의 subtree로 들어간다
- `MyApp.build` 메소드 실행 이후에는, MaterialApp 노드가 child로 생기게 되고, `MaterialApp.build` 메소드 실행 이후에는 HomeScreen 노드가 child로 생긴다
![](https://velog.velcdn.com/images/oojoo/post/ae2b58de-810a-4f5a-a55b-4ef1a81257d2/image.png)

즉, 위젯을 만들 때 넘겨준 파라미터의 이름이 child 이였으므로, 하위 위젯이 return 으로 상위 위젯에게 넘겨주면, 하위 위젯이 상위 위젯의 child 노드가 되는 것이다

## SubTree
```dart
@override
Widget build(BuildContext context) {
  return Container(
    color: Colors.blue,
    child: Row(
      children: [
        Image.network('https://www.example.com/1.png'),
        const Text('A'),
      ],
    ),
 ```
코드에 보이는 widget은 `Container`와 `Row` 2개이지만, 플러터로 `build()` 메소드를 실행하면 subtree에 다른 widget이 추가로 들어가있다
![](https://velog.velcdn.com/images/oojoo/post/1d1384ba-6fac-4cfb-98b3-90f959bf2add/image.png)

위의 상황처럼, widget 하나를 그려도 플러터 내부적으로 사용하는 추가적인 widget이 widget tree에 들어가게 될 수 있다

---

# 2. Element tree

위젯에서 화면에 그려질 방식들을 지정하면, 플러터는 실제로 그려질 instance를 만들게 되는게 이것이 element 이다
- 플러터의 Lifecycle은 widget이 아닌 element 가 관리한다
`mount`, `update`, `performRebuild` 등 메소드 존재함

**element tree의 노드와 widget tree의 노드는 모두 1:1 대응이다**

<img src="https://velog.velcdn.com/images/oojoo/post/f763529c-be63-46b6-9ce2-2755dc7a3cdf/image.png" width="70%">
Element tree의 루트 노드인 `ComponentElement`는 Widget tree의 루트 노드인 `Container`와 대응되고, 그 아래의 `RenderObjectElement`는 `ColoredBox`에 대응된다

## Element tree에는 2가지 종류만 존재한다
- `ComponentElement` : 다른 element의 host 역할을 함으로써 대장역할이다. 플러터가 실제로 화면을 그릴 때 이 종류의 element는 참고하지 않는다
- `RenderObjectElement` : 실제로 화면에 그릴 때 플러터가 참고하는 element이다. 

![](https://velog.velcdn.com/images/oojoo/post/f3b011d7-9dca-4bff-a093-e4e9282892b8/image.png)

Element tree 는 widget tree와 RenderObject tree를 모두 참조한다.
즉, widget이 변한 경우 해당 변화를 감지하고 renderObject를 바꿀지말지 결정하는 것이다

---

# build 함수에서의 Context
모든 위젯은 `widget.buildContext`를 통해 자신과 연결된 element에 접근가능하다

`buildContext` 안에는 자신이 widet tree에 어떤 노드에 위치하는지 알려주는 정보가 있기 때문이다
- `build()` 함수에서 계속 넘겨주는 `(BuildContext context)` 의 `context`에서 현재 노드의 위치정보를 가지고 있다

```dart
showDialog(
  context: context,
  builder: (BuildContext context) {
    return AlertDialog(
  ...
);
```
Dialog를 띄울 때에도 현재 rendering 된 화면 위에 Dialog의 배경과 UI를 추가로 그린다는 것이므로, 현재 rendering 된 화면이 해당 앱의 트리에서 어디에 위치하는지 알아야 한다

트리 상에서의 위치정보를 `context` 가 가지고 있으므로 `build()` 함수에서 `context` 계속 넘겨준 것이었다

---


# 3. RenderObject tree
✏️ 개발자는 widget code를 작성하고, 플러터는 개발자의 widget code를 보고 element 를 만든다. element에는 2가지 종류가 있으며, host 역할인 element 말고 다른 element가 직접적으로 Rendering 에 관여를 한다

플러터가 마지막으로 만드는 트리로, renderObject tree는 element tree를 참고하여 만들어진다
![](https://velog.velcdn.com/images/oojoo/post/c5aece83-242f-41e3-8efa-f4698880a15f/image.png)

- renderObject tree에는 반드시 필요한 내용만 담겨진다
- reder tree의 노드가 될 수 있는 노드는 element tree 의 노드 중 RenderObjectElement 노드를 바탕으로 만들어진 노드 뿐이다 (host인 element 제외)

## Layout
Layout 단계에서는 render tree를 참고하면서 최종적으로 widget이 어느 위치에 그려질지 결정된다
- `Expended`, `Flexible` 등의 위젯들이 widget의 최상단에 감싸지는 이유이다
- 크기를 결정하게 되는 `child` 도 widget의 상단에 위치하는 이유이다

Layout 단계는 루트 노드부터 리프노드까지 내려가면서 Constraint 에 대한 정보만 확인한다 (예를 들어, size, position 등)

parent 노드로부터 제약 조건을 받은 child 노드는 해당 정보를 바탕으로 모양과 사이즈만 결정한다. 결정된 모양 및 사이즈 등의 정보를 다시 위쪽으로 올려보낸다. 이러한 정보만 오가는 것이 layout 의 작업이다.
Layout 의 작업이 끝나면 이제서야 화면에 그리는 paint 단계에 들어가게 되는 것이다
- 즉, 제한조건을 내려보내고, 사이즈를 올려보낸다 !!

- 플러터의 layout 작업은 1개의 frame 당 1번 일어난다
즉, 초당 60 frame을 지원하므로 초당 60번의 layout 작업이 일어나고 있는 것이다

---

# ✨ 플러터가 위젯을 렌더링하는 순서 
1. widget tree 만들기: build 함수 호출
2. element tree 만들기: widget tree 보고 만듬
3. render tree 만들기: element tree 보고 만듬
4. layout 작업: 1개의 프레임마다 1번의 layout 작업
5. 화면에 그리기 
