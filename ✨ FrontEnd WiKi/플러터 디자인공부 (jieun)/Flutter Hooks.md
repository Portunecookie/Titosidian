- 플러터는 상태를 변경하지 않고 **변경된 상태를 기반으로 위젯을 다시 생성함으로써 변하는 상태로부터 어플리케이션을 제어**한다. 따라서, 플러터에서는 **위젯을 다시 생성하기 위해서는 StatefulWidget**을 사용해야 하며 상태가 변함에 따라 다시 UI을 고쳐지게 하기 위해 setState를 호출해주어야 한다

# Flutter Hooks
플러터훅은 플러터 상태관리 라이브러리인 riverpod을 만든 사람이 만든 라이브러리이다. 

`StatelessWidget`과 `StatefulWidget`을 대신하는 `HookWidget`을 제공하는 라이브러리이다.
`StatefulWidget`은 `initState`나 `dispose`라는 논리를 다시 사용하기에 어렵다.예를들면, AnimationController를 사용하려는 모든 위젯을 모든 논리를 처음부터 다시 구현해야 한다.  

- Hook : **'자주 사용하는 로직을 미리 정의하여 필요할 때 꺼내다 쓴다'** 

```dart
class Example extends StatefulWidget {
  final Duration duration;

  const Example({Key? key, required this.duration})
      : super(key: key);

  @override
  _ExampleState createState() => _ExampleState();
}

class _ExampleState extends State<Example> with SingleTickerProviderStateMixin {
  AnimationController? _controller;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(vsync: this, duration: widget.duration);
  }

  @override
  void didUpdateWidget(Example oldWidget) {
    super.didUpdateWidget(oldWidget);
    if (widget.duration != oldWidget.duration) {
      _controller!.duration = widget.duration;
    }
  }

  @override
  void dispose() {
    _controller!.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```
위의 예제를 `Hooks`를 이용해 다시 짠 것 

```dart
class Example extends HookWidget {
  const Example({Key? key, required this.duration})
      : super(key: key);

  final Duration duration;

  @override
  Widget build(BuildContext context) {
    final controller = useAnimationController(duration: duration);
    return Container();
  }
}
```
 `Hooks`는 widget과 완전히 독립적으로 재사용이 가능하다. 


### pub.dev
- widget의 life cycle 을 관리하는 새로운 종류의 객체
- widget간 코드 재사용성을 높여 코드 중복을 줄여 코드를 간단화하는 것이 목적


### Flutter Hooks 사용방법
1. 기존의 `StatelessWidget`와 `StatelessWidget`를 `HookWidget`으로 변경
2. flutter hook 객체를 build 메소드에서 생성

```dart
import 'package:flutter_hooks/flutter_hooks.dart';
import 'package:flutter/material.dart';

class App extends HookWidget {
	const App({super.key});
    
    @override
    Widget build(BuildContext context) {}
```

`StatefulWidget`을 `HookWidget`으로 바꿈으로써 `StatefulWidget`과 `State`로 나누어진 보일러플레이트 코드와 `initState`, `dispose` 라이프사이클 메소드가 필요 없어졌다. 

### Flutter Hooks 기능들
1.	useState	변수를 생성하고 구독합니다.
  - 내가 변수를 선언하고 그 변수의 상태가 변하게 된다면 자동으로 재빌드를 해주는 느낌으로 자동으로 setState(){}를 해주는 역할
2.	useEffect	상태를 업데이트하거나 선택적으로 취소하기에 유용합니다.
3.	useMemoized	다양한 객체의 인스턴스를 캐싱합니다.
4.	useStream	Stream을 구독합니다. AsyncSnapShot으로 현재상태를 반환합니다.
5.	useFuture	Future를 구독합니다. AsyncSnapShot으로 상태를 반환합니다.
6.	useAnimation	Animation을 구독합니다. 해당 객체의 value를 반환합니다.
7.	useReducer	state가 조금 더 복잡할때, useState 대신 사용할 대안입니다.
8.	useTextEditingController	TextEditingController를 생성합니다.
9.	useTabController	TabController를 생성합니다.
10.	useScrollController	ScrollController를 생성합니다.
11.	usePageController	ScrollController를 생성합니다.
